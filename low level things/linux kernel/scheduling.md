> [!note] process scheduler
> sottosistema del kernel che mette i processi a lavoro. Componente fondamentale in sistemi multitasking e multi utente. 
> Da l'impressione che il computer stia facendo molte cose "contemporaneamente" (cio' e' parzialmente vero).

> [!note] sistema operativo multitasking
> E' un sistema che puo' simultaneamente cambiare programma in esecuzione, dove in una macchina con un core, da l'impressione di eseguire piu programmi insieme, anche se ho una unita di elaborazione.

> **Nota**: su sistemi a piu CPU, lo scheduler serve comunque per orchestrarale!

Due tipi di multitasking:
* `cooperative multitasking`: il processo decide quando fermarsi: `yelding`. Non permette allo `scheduler` di fare scelte **globali**.
* `preemptive multitasking`: modello adottato da `Unix` e `Linux`, si sceglie quando fermare un processo per eseguirne un'altro in stato di `running`.

> `Preemption`: fermare un programma in esecuzione inavvertitamente e involontariamente.

Il tempo  per cui un processo  esegue prima di essere interrotto e' detto `timeslice`. (uno slice del tempo del processore).

## scheduler de linux
* fino alla `2.4` lo scheduler era banale e semplice da capire, ma funzionava male con molti processi (**scalava male**)
* `2.5`: scheduler `O(1)` perche' esegue tutte le operazioni in tempo costante. Si calcola lo `slicetime` in tempo costante ma non sia adattava bene a applicazioni `latency-sensitive`, ossia applicazioni `interattive`. Tuttavia era ideale per i server.
* `2.6.23`: `CFS`, Completely Fair Scheduler.

## Policy 
> [!note] Policy
> Ossia, il **comportamento dello scheduler**, decide cosa viene eseguito e quando.

I processi possono essere:
* `I/O bound`: aspetto tanto su operazioni di `I/O`, quindi il processo viene eseguito poche volte, la maggior parte del tempo il processo deve attendere. 
* `Processor Bound`: eseguo tanto codice, e non tendo ad essere bloccato da richieste di `I/O`.

> Per `I/O` si intende **lettura e scrittura** su un qualsiasi `device a blocchi`.

E ovviamente  ci sono applicazioni che tendono ad essere **entrambe** (come il server grafico oppure un word processor).

Bisogna quindi scegliere una policy che da ottime performance su entrambi i tipi di processi (responsivo in caso di `I/O`, offra la potenza necessaria in caso di `processor bound`)

## Priority
>[!note] Priority
> Scheduling basato su priorità. Faccio un rank dei processi basati sul loro bisogno di CPU, i processi con priorità più alta eseguono prima. 

Eseguo in round robin per ogni livello di priorita. La priorita e' stailita usando
* `nice`: un valore da `-20 a +19` dove, un valore alto vuol dire bassa priorita. Di default e' 0. Standard in `Unix`
* `rt priority`: un valore che di default va da `0 a 99`, inclusi. Valore alto indica una priorita alta, applicazioni real-time dovrebbero avere valori alti.

>[!note] `nice` vs `rt priority`
> * `nice`: alto vuol dire bassa priorita, `-20 a +19`.
> *  `rt priority`: alto vuol dire alta priorita, `0 a 99`.

> `ps -eo state,uid,pid,ppid,rtprio,time,comm` per vedere come se la passano i processi, la colonna `rtprio` indica la priorità.

## timeslice
>[!note] timeslice
> valore numerico che indica per quanto tempo un task viene eseguito prima di essere interrotto.
> * **troppo alto** -> sistema poco reattivo
> * **troppo basso** -> troppo overhead da parte dello scheduler, cambio processo e' un'operazione che occupa risorse.

> **Nota**: applicazioni io bound non hanno bisogno di timeslice lunghi, e cpu bound non hanno bigosno di timeslice corti.

Linux assegna **porzione** della CPU al processo, calcolato come una funzione del carico del sistema.

Ma con lo scheduler CFS: la porzione e' calcolata in base a **quanta CPU, il processo ha usato nell'ultima iterazione**. Se ha consumato poca CPU rispetto al processo **correntemente in esecuzione**, esegue subito, facendo `preempting`. altrimenti eseguira piu' tardi.

## La scheduling policy in azione!
Scenario, stanno eseguendo:
* `text editor`: **altamente io bound**, aspetto l'utente che preme la tastiera.
* `video encoder`: nonostante legge continuamente dati dalla memoria, esegue per tutto il tempo operazioni sulla cpu, quindi `cpu bound`. usare `ffmpeg` puo' tranquillamente usare tutta la cpu.

**Come vogliamo ripartire la cpu?** il `text editor` dovrebbe avere molto tempo a disposizione per essere **reattivo**, e vogliamo che faccia `preemting` sul video encoder quando ottengo input da tastiera.

**Quindi**, generalmente nei sistemi operativi: *diamo al text editor slice piu grandi e priorita piu alta.*

**In Linux**: garantiamo al text editor uno slice della cpu. 
Banalmente, se entrambi hanno stesso nice level, allora la ripartizione e' del `50%` (immaginando questi due come gli unici processi da eseguire), ma il `text editor` non usera mai tutta la sua percentuale, e l'encoder tendera a saturarla.

Cosa succede quando il text editor si sveglia? Bisogna eseguire immediatamente e CFS nota che e' stato usato meno del 50% della cpu, e quindi che il text editor ha eseguito per meno tempo rispetto al video encoder.

Sulla base di questa osservazioni, CFS poi fara preempting sul video encoder per eseguire il text editor.

Quindi con CFS: il video encoder esegue sempre, e il text editor esegue ogni tanto in modo reattivo quando si risveglia.

> Perche' il text editor e' in stato di **blocco**, in attesa di input, quando passa a `RUNNING`, CFS se ne accorge e blocca il video encoder, per eseguire il text editor dandogli poca CPU.

## Linux scheduling algorithm
Le parti fondamentali di `CFS` sono:
* time Accounting
* Process Selection
* Scheduler Entry Point
* Sleeping and Waking Up

Codice in: `kernel/sched_fair.c`?????
### time accounting
Ad ogni `tick` del clock di sistema, il timeslice e' decrementato, quando arriva a 0 eseguo un'altro processo con timeslice non zero.

```c
struct sched_entity {
	struct load_weight load;
	struct rb_node run_node;
	struct list_head group_node;
	unsigned int on_rq;
	u64 exec_start;
	u64 sum_exec_runtime;
	
	u64 vruntime; // importante!
	
	u64 prev_sum_exec_runtime;
	u64 last_wakeup;
	u64 avg_overlap;
	u64 nr_migrations;
	u64 start_runtime;
	u64 avg_wakeup;
}
```

### virtual runtime
> [!note] `vruntime`
`vruntime` contiene il `virtual runtime` *ossia il tempo (in nanosecondi) di esecuzione **pesato** sul numero di processi in stato di running*. Questo valore viene usato per modellare il ***multitasking processor ideale***.

```c
static void update_curr(struct cfs_rq *cfs_rq)
{
	struct sched_entity *curr = cfs_rq->curr;
	struct rq *rq = rq_of(cfs_rq);
	s64 delta_exec;
	bool resched;

	if (unlikely(!curr))
		return;

	// calcola il tempo corrente di esecuzione 
	delta_exec = update_curr_se(rq, curr);
	if (unlikely(delta_exec <= 0))
		return;

	// incrementa il vruntime con il tempo PESATO
	// wrapper per __delta_fair
	curr->vruntime += calc_delta_fair(delta_exec, curr);
	resched = update_deadline(cfs_rq, curr);
	update_min_vruntime(cfs_rq);

	if (entity_is_task(curr)) {
		struct task_struct *p = task_of(curr);

		update_curr_task(p, delta_exec);

		if (dl_server_active(&rq->fair_server))
			dl_server_update(&rq->fair_server, delta_exec);
	}

	// fai accounting!
	account_cfs_rq_runtime(cfs_rq, delta_exec);

	if (cfs_rq->nr_queued == 1)
		return;

	if (resched || did_preempt_short(cfs_rq, curr)) {
		resched_curr_lazy(rq);
		clear_buddies(cfs_rq, curr);
	}
}
```

Questa procedura e' chiamata periodicamente e al cambio di stato del processo (va in stato di `RUNNING` o si blocca).

Scelgo di eseguire un processo in base chi ha il `vruntime` piu piccolo, implementato grazie ad un `red-black tree`, `rbtree` in linux, un tipo di albero bilanciato per la ricerca.

### Scegliere il prossimo task
