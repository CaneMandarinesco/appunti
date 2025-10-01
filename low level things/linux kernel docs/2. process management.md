>[!note] process
> e' un programma (ossia *object code* caricato in memoria) rappresentato nel corso della sua esecuzione. Un processo e' composto da varie informazioni satelliti:
> * segnali
> * memory mapping
> * threads
> * data section

> **Nota**: in Linux con c'e' differenza tra thread e processi. sono entrambi rappresentati da `task_struct`, ma differiscono per le flag di creazione che definiscono le risorse condivise.

Le informazioni piu' importanti di un processo sono: 
* **program counter**
* **stack**
* **insieme dei registri del processo**

Un processo **inizia** quando viene creato da una `fork`, ossia la duplicazione del processo chiamante, ritornando due valori diversi per distinguere padre dal figlio, una volta duplicato l'**address space** (e l'object code) puo' essere **sostituito** da un'altro programma grazie ad una delle funzioni `exec` Un processo termina con la chiamata `exit()`.
Un processo attende la terminazione di un figlio con `wait4()`.

> `fork` e' implementata tramite la chiamata `clone`

> `task` e `processi` sono la stessa cazzo di cosa

## List
Alcune delle strutture usate per implementare tipi di liste prese da
[<linux/types.h>](https://codebrowser.dev/linux/linux/include/linux/types.h.html#list_head)
```c
struct list_head{
	struct list_head *next, *prev;
}

struct hlist_head {
	struct hlist_node *first;
}

struct hlist_node {
	struct hlist_node *next, **prev
}
```
## Process Descriptor e Task Structure
La lista di processi e' una `circular doubly linked list` chiamata *task list*, dove ogni elemento e' un `struct task_struct` [definito](https://codebrowser.dev/linux/linux/include/linux/sched.h.html#task_struct) in `<linux/sched.h>`.

**Questa contiene**:
* `void *stack`
* una sezione con variabili per `SMP`
* `int exit_code, exit_state, exit_signal`
* dichiarazioni tipo `unsigned xyz`, ossia `unsigned int`.
* `pid_t pid, tgid`;
* `struct task_struct __rcu *real_parent, *parent;`
* `struct list_head children, sibling;` dove `list_head` ha riferimenti al *precedente e al successivo*!
* e tanta altra roba...

![[Pasted image 20250720162837.png]]

`task_struct` e' allocata usando lo `slab allocator` (in modo da favorire il riutilizzo della memoria allocata).

In `x86`, alla ~~fine~~ dello `stack` (o meglio, alla base) di ogni processo risiede la `struct thread_info`, definita (per `x86`) in `<asm/thread_info.h>`:
```c
struct thread_info {
	struct task_struct *task;
	struct exec_domain *exec_domain;
	__u32 flags;
	__u32 status;
	__u32 cpu;
	int preempt_count;
	mm_segment_t addr_limit;
	struct restart_block restart_block;
	void *sysenter_return;
	int uaccess_err;
}
```

> **Nota**: `thread_info` e' una struttura a parte rispetto a `task_struct` probabilmente perche' l'allocazione di `thread_info` dipende dall'architettura mentre `task_struct` e' una struttura piu generica. 

> **Nota**: `thread_info` contiene quindi un set ristretto di informazioni utili e piu facili da gestire e reallocare in caso. 

> **Nota**: `thread_info` in oltre e' allocato nello stack del processo!

![[Pasted image 20250720163255.png]]

## PID
> **opaque type**: tipo di dato la cui rappresentazione fisica e' irrilevante. 
> Puo' essere un `short int` oppure essere da `4 milioni`.
> https://codebrowser.dev/linux/linux/include/linux/types.h.html#list_head

> Nota: `/proc/sys/kerne/pid_max` controlla il valore massimo per un pid

Per lavorare con un processo basta conoscere il puntatore alla sua `struct task_struct`, per ottenere il task corrente si usa la macro `current`.

Il modo in cui `current` e' implementato varia a seconda del processore. *In x86* si ottiene mascherando i 13 bit meno significativi dello stack pointer, da cui ottengo la `thread_info`.

Tutto cio' viene fatto da `current_thread_info()` il cui assembly e' di 2 istruzioni, `current` e' definito come `current_thread_info()->task`;

> Quindi lo `stack` deve essere di `8KB ~= 8192`. Questo e' vero generalmente in quanto la grandezza potrebbe variare a seconda dell'architettura 

Altre piattaforme come PowerPC immagazzinano il riferimento alla `task` corrente in un registro. *La stessa cosa non succede in x86 perche' si tende ad avere meno registri liberi* ma piu potenza di calcolo (credo).

> [!note] recap su x86
> * maschera i primi *13 bit di SP* -> ho ottenuto `thread_info`
> * da `thread_info->task` contiene il riferimento alla struttura

### state
![[Pasted image 20250720165934.png]]

Il campo `state` indica lo stato del processo: 
* `TASK_RUNNING`: sta **eseguendo**, o in **attesa** di essere eseguito.
* `TASK_INTERRUPTIBLE`: sta **dormendo**, in attesa di una **condizione** o di un `segnale`.
* `TASK_UNINTERRUPTIBLE`: come la sua controparte ma non si sveglia a seguo di un `segnale`, generalmente meno usato perché "*non reattivo*".
* `__TASK_TRACED`: nel caso di un **debugger** che ne fa il `tracing`.
* `__TASK_STOPPED`: non in esecuzione e non eleggibile per l'esecuzione. questo stato si ha quando riceve un segnale (`SIGSTOP, SIGSTP, SIGTTIN, SIGTTOU`).

> Nota: se `TASK_INTERRUPTIBLE`, quando la condizione e' soddisfatta il kernel imposta lo stato a `TASK_RUNNING`.

Lo `state` si manipola con `set_task_state(task, state);` che crea una **barriera sulla memoria** in caso di processori `SMP`, altrimenti e' equivalente a fare: `task->state = state;`

> Nota: esiste la procedura `set_current_state(state);`

## Context
Il codice da eseguire e' letto da un `executable file`.
L'esecuzione avviene normalmente in `user-space`, quando avviene una system call si entra in `kernel-space`.

Di fatto le system call sono le **interfacce** per eseguire codice privilegiato.

## Family Tree
Tutti i processi sono discendenti di `init` che ha `PID=1`, questo processo viene avviato alla fine del processo di `boot`. 

Si occupa di:
* eseguire `initscripts`
* eseguire vari programmi che completano il `boot`

> Ogni processo ha un **parente** e 0 o piu' **figli**, di conseguenza esiste la relazione di fratellanza da due processi.

Ogni `task_struct` ha un puntatore alla `task_struct` del padre.

Per iterare sui figli di un processo:
```c
struct task_struct *task;
struct list_head *list;

list_for_each(list, &current->chidren) {
	task = list_entry(list, struct task_struct, sibling);
}
```

> **Nota**: il process descriptor per `init` e' allocato **staticamente** come `init_task`.

**Esempio**: in questo codice si risale dal processo corrente fino al padre, e l'esecuzione del for termina sempre!
```
for(task = current; task != &init_task' task = task->parent);
```

>[!note] `current`
> `current` e' implementato come `#define current get_current()`


>[!note] navigare una lista di processi
> * `list_entry(task->tasks.next, struct task_struct, tasks)` ossia la macro `next_task(task)`
> * `list_entry(task->tasks.prev, struct task_struct, tasks)` ossia la macro `prev_task(task)`
> * per **iterare** si usa la macro `for_each_process(task) { ... }

**Esempio**: stampa nome e pid di ogni processo. (**Nota** che iterare su tutti i processi potrebbe essere **un'operazione expensive**).
```c
struct task_struct *task;

for_each_process(task) {
	printk("%s[%d]\n", task->comm, task->pid);
}
```

## Creazione
In `Unix` generalmente si crea un "*nuovo*" processo usando `fork` e poi `exec` (il che e' inusuale). 

Dopo la `fork`, tutte le risorse del padre sono duplicate, e il figlio ne ha una copia, e capiamo gia da subito che questo meccanismo e' molto costoso computazionalmente, e a livello di memoria.

`fork` e' implementata infatti con `copy-on-write` pages, ossia si usa la tecnica `COW` nella quale una pagina viene copiata per intera e modificata solo quando c'e' effettivamente una modifica.

> Qundi chimare `exec` subito dopo una `fork` comporta che *nessuna pagina* del padre e' stata copiata.

L'unico **overehead** della `fork` consiste nella *duplicazione della page table* e della creazione di un PID unico.

---

### forking
Il fork avviene dunque tramite `clone`, la chiamata di sistema ce chiama `do_fork` definita in `kernel/fork.c`, quest'ultima chiama `copy_process` e poi esegue il processo in esecuzione.

`copy_process` fa:
1. `dup_task_struct`: crea nuovo stack kernel, `thread_info`, `task_struct` con valori identici a quelli del padre.
2. controlla se non eccedo i **limiti di risorsa** per l'utente
3. ==modifica vari campi del processo figlio==
4. imposta `state` a `TASK_UNINTERRUPTIBLE`: assicurati che non venga eseguito!
5. con `copy_flags()` aggiorna le `flags` in `task_struct`. `PF_SUPERPRIV` indica se una task ha usato privilegi super-user. `PF_FORKNOEXEC` e' un processo che non ha chiamato `exec`.
6. ritorna il puntatore al nuovo figlio.

> [!note] `copy_process`
> * `dup_task_struct` per copiare le strutture dal padre
> *  `TASK_UNINTERRUPTIBLE` per assicurasi che il figlio non venga eseguito
> * `copy_flags` per aggiornare le flag (`PF_SUPERPRIV`, `PF_FORKNOEXEC`)

## `vfork`
> [!note] `vfork`
> Come `fork`, ma non copia la page table del padre. 
> Il figlio esegue come thread nel address space del padre, di conseguenza il padre e' **bloccato** finché' il figlio non esegue `exec` o  fa `exit`.

> Nota: l'unico benefit di `vfork` rispetto a `fork` e' che non si copia la page table entry

E' implementata cosi, grazie ad una **special flag**:
1. `copy_process`: imposta `vfor_done` a `NULL`
2. `do_fork`: se la **special flag** e' stata passata, allora `vfork_done` punta ad un indirizzo specifico
3. Il padre per ritornare aspetta il segnale passato tramite `vfork_done`.
4. `mm_release`: usata quando un task esce da uno spazio di memoria. Se `vfork_done` non e'  `NULL` allora il padre riceve il segnale
5. `do_fork`: ritorna ed il padre e' risvegliato.

## Threads
permettono di eseguire in modo **concorrente** piu' parti di uno stesso programma e della sua memoria: piu' thread accedono ad **un'unico address space condiviso**.

>[!note] Thread
> Ogni thread e' per il kernel un processo che **condivide** risorse con altri processi. Permettono in modo semplice e veloce (lightweight) di condividere risorse tra piu unita di esecuzione.

### creazione
avviene come per i processi ma: `clone(CLONE_VM | CLONE_FS | CLONE_FILES | CLONE_SIGHAND, 0)`, passando delle flag che definiscono un thread.

Nota che:
* `fork` e' implementata come: `clone(SIGCHLD, 0)`
* `vfork` e' implementata come: `clone(CLONE_VFORK | CLONE_VM | SIGCHLD, 0)`

### kernel threads
> [!note] kernel threads
> Il kernel potrebbe volere eseguire delle **operazioni in background**, grazie ai `kernel threads`, processi che esistono solo in `kerne-space`

Non hanno uno spazio degli indirizzi: `mm` e' `NULL`; non fanno context switch verso lo spazio utente e sono **preemptable**.

Alcune task del kernel sono: `flush` e `ksoftirqd`, puoi vederli con `ps -ef`.

i kernel threads sono creati al boot da altri thread del kernel: *un kernel thread puo' essere creato solo da un'altro kernel thread*.

Un kernel thread e' forkato a partire da `kthreadd`. L'interfaccia che li implementa si trova in `<linux/kthread.h>`:
```c
struct task_struct *kthread_create(int (*threadfn) (void *data), void *data, const char namefmt[], ...)*
```
dove `namefmt` e' una stringa in `printf` style.

La nuova task e' creata tramite la `clone()`, chiamata dal processo `kthread`, e il nuovo processo eseguira `threadfn`.

> Nota: `kthread_create`, crea ma **non sveglia** il processo creato.
> `kthread_run` invece lo esegue.

> Nota: `kthread_run` e' una **macro** che chiama `kthread_create` e che poi chiama `wake_up_process`


### morte di un thread
`do_exit` gestisce tutto:
1. `PF_EXITING` e' impostata nelle flag della struct
2. `del_timer_sync` rimuove timer del kernel
3. con `exit_mm` rilascia la `mm_struct` se l'indirizzo non e' condiviso.
4. `exit_sem`: rilascia i semafori `IPC` su cui sono in attesa
5. `exit_files` e `exit_fs`: decrementa il *contatore d'uso* per le risorse, se arriva a 0 la risorsa puo' essere distrutta e rilasciata.
6. imposta `exit_code` nella `task_struct`
7. `exit_notify` per mandare segnali al padre ...
8. `do_exit` chiama `schedule` per cambiare processo in esecuzione, e `do_exit` **NON RITORNA MAI!**

Alla fine:
* tutti gli oggetti sono liberati (se il processo ne era l'unico utilizzatore)
* Il processo non e' eseguibile ma e' in stato di zombie
* Il pid del processo ancora esiste ed e' associato 

Questo permette di ottenere informazioni dal processo figlio **anche dopo la sua terminazione,** per esempio nel caso delle `wait` in cui c'e' bisogno di fornire al padre: `exit_code` e `PID` del figlio che ha scatenato l'evento.

`release_task` viene chiamato dopo che il kernel si e' assicurato che il padre abbia ricevuto le informazioni del figlio:
1. `__exit_signal`: chiama `__unash_process()` che chiama `detach_pid()`, ossia si rimuove il processo dalla **pidhash** e rimuovo il processo dalla **task list**.
2. il processo e' morto ora: libero le ultime risorse usate.
3. con `put_task_struct` libero le pagine del kernel stack e la `thread_info` e dealloco la `slab cache` che contiene la `task_struct`.

### il problema del padre
> **Problema**: il padre muore prima dei figli.

Bisogna `riparentare`, il figlio viene reparentato verso un'altro processo nel **gruppo thread corrente.**

Alternativamente con `find_new_reaper()` (chiamato a cascata da `do_exit`

```c
static struct task_struct *find_new_reaper(struct task_struct *father, struct task_struct *child_reaper)
{
	struct task_struct *thread, *reaper;

	// cerca un thread nello stesso gruppo
	thread = find_alive_thread(father);
	if (thread)
		return thread;

	if (father->signal->has_child_subreaper) {
		unsigned int ns_level = task_pid(father)->level;
		for (reaper = father->real_parent;
		     task_pid(reaper)->level == ns_level;
		     reaper = reaper->real_parent) {
			if (reaper == &init_task)
				break;
			if (!reaper->signal->is_child_subreaper)
				continue;
			thread = find_alive_thread(reaper);
			if (thread)
				return thread;
		}
	}

	return child_reaper;
}
```

si tenta di:
* cercare un thread dello stesso gruppo per l'adozione (ossia un fratello?)
* cercare un thread tra i padri che e' `child_subreaper`
* oppure ritorno il `child_reaper` default passato alla chiamata. (**fallback**)