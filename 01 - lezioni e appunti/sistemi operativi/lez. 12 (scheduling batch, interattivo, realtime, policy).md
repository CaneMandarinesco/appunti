Lo scheduling e' fondamentale in contesti tipo server. Infatti inizialmente quando si eseguivano principalmente programmi su un nastro magnetico lo scheduler non era fondamentale e sui personal computer non era cosi importante lo scheduling dei processi dato che nella maggior parte dei casi viene eseguito un programma per volta (quello a schermo) e questo spesso e' un processo limitato dall'io. Raramente un utente di un personal computer e' interessato a fare computazioni che usano in modo massiccio la CPU.

E' importante anche in contesti mobile: cambiare processo molte volte e' oneroso in termini di consumi (devo effettuare il context switch).

Bisogna notare che oggi le CPU forniscono ai sistemi le risorse necessarie per eseguire la maggior parte dei compiti.

### Problemi durante lo scheduling
Posso avere due tipi di processi:
* **CPU bound**, **long burst di istruzioni**: eseguono molte istruzioni e dipendono poco dall'IO
* **IO bound, brevi burst di istruzioni**: la CPU viene spesso interrotta dall'IO.

L'algoritmo di scheduling dei processi non puo' essere sempre lo stesso: dipende dal contesto.

### Situazioni di scheduling
* **Creazione del processo**: quando faccio un fork devo scegliere se eseguire prima il padre o il figlio (sono entrambi processi identici), e sono entrambi nello stato di pronto.
* **Terminazione**: il processo termina la sua esecuzione. Ora devo scegliere chi eseguire.
* **Blocco**: a causa di un mutex, una barriera, un `pthread_join`, ..., devo scegliere chi eseguire. Magari sono in attesa del'io
* **Interrupt** i/o: devo eseguire il destinatario dell'io o magari continuo a eseguire i thread in lista?
### Tipo di scheduling
* **Preemptive**: lascia che i processi vengano eseguiti per un certo periodo di tempo, allo scadere del tempo bloccane l'esecuzione e scegli un'altro processo.
* **Non Preemptive**: esegui il processo fino a che non viene rilasciato volontariamente o finché non si blocca.

Lo scheduling preemptive e' utile nel kernel: posso bloccare l'esecuzione di una chiamata di sistema o di un driver che impiegano troppo tempo per essere eseguiti. 

In base al tipo di sistema ho esigenze diverse:
* **batch**: mi aspetto di eseguire una serie di batch, uno dopo l'altro. Mi deve gestire l'esecuzione di task periodici. Sono usati in sistemi come banche e in contesti finanziare (es: compravendita di azioni).
	* devo mantenere la CPU sempre attiva
	* devo ridurre il tempo di **turnaround** (tempo effettivo dall'inizio del processo fino alla sua fine)
	* so quanto tempo ci impiega un task per  eseguire, mi aspetto di poterlo eseguire in un determinato lasso di tempo
	* devo gestire processi CPU-bound e IO-bound.
* **interattivo**: bisogna usare uno scheduling **preemptive**, ho il rischio che un processo monopolizzi tutta la cpu e non permetta l'esecuzione di altri task importanti.
	* devo soddisfare le richieste dell'utente in modo reattivo.
* **real-time**: devo orchestrare i processi in modo da garantire il rispetto delle scadenze.
	* nei sistemi multimediali e' fondamentale per evitare di degradare la qualità dei dati elaborati.

In tutti i sistemi uno scheduler mi garantisce:
* **Equità**: cerca di garantire un'equa condivisione delle risorse.
* **Policy**.
* **Bilanciamento**: mantieni tutte le componenti del sistema attive.


## Scheduling in sistemi Batch
### First-Come, First-Served
* **non preemptive**
I processi vengono eseguiti a seconda dell'ordine in cui arrivano, se quello in esecuzione va in stato di bloccato viene messo in fondo alla coda.
Le prestazioni non sono ottimali in scenari dove ho processi che usano CPU e IO in modo equo. Se sto eseguendo un job io bound, i processi dopo potrebbero aspettare molto tempo prima di essere eseguiti

### Shortest Job First
* **non preemptive**
Devo conoscere i tempi di esecuzione in anticipo. Il job più breve tra quelli arrivati viene eseguito per primo. Questo algoritmo minimizza il **tempo di turnaround medio** quando tutti i job sono disponibili contemporaneamente. 

Puo' non essere ottimale in base all'ordine di arrivo dei job.

### Shortest Remaining Time Next
* com SJF ma, **preemptive**
Seleziona il processo con il tempo rimanente piu breve per il completamento e il tempo di esecuzione deve essere noto in anticipo.

## Scheduling in sistemi interattivi
* devo massimizzare il tempo di risposta 
* soddisfare le aspettative di tutti gli utenti

### round robin
Ogni processo ha un intervallo di tempo detto **quanto** per eseguire, ogni processo. Se il processo non termina entro il quanto viene interrotto (**preemptive**). 

> Per implementarlo basta avere una lista di processi eseguibili che scorro.

Devo scegliere un valore per il **quanto** giusto: se per esempio ho un quanto fissato per tutti i processi a $4\text{ms}$ e il cambio di contesto richiede $1\text{ms}$ vuol dire che il $25\%$ della CPU e' sprecato in **overhead**:
* **Quanto lungo**: peggiora la reattività, ma spreco meno cpu per il cambio di contesto
* **Quanto corto**: migliora la reattività, ma spreco piu cpu

### priority
Nel round robin tutti i processi hanno la stesa importanza. Ha piu senso introdurre il concetto di priorita nello scheduling: un daemon che controlla gli aggiornamenti e' meno importante del programma in esecuzione a schermo.

> La CPU esegue prima i processi con priorità più alta.


### Shortest Process Next con Aging
Voglio applicare l'algoritmo "Shortest Job First" nei sistemi interattivi. Il problema e' che in un sistema interattivo mi capitera raramente di conoscere il tempo di esecuzione di un programma (potrebbe non finire mai). 

La soluzione e': **fare delle stime**. 
* $T_0$ e' il tempo di esecuzione misurato la prima volta
* $T_{0}/2 + T_1 /2$ e' la media pesata del tempo di esecuzione del tempo $T_0$ e il tempo $T_1$ misurato alla seconda esecuzione.
* $T_0 /4 + T_1 /4 + T_2 / 2$ e' la media pesata dopo che ho misurato il tempo alla 3a esecuzione
* ...
### Guaranteed scheduling
Faccio delle promesse concrete agli utenti del mio sistema, per esempio se ho $n$ utenti posso assegnare a ciascuno $1/n$ delle risorse dalla CPU.

### Scheduling a lotteria
Assegno dei biglietti per una vera e propria lotteria per le risorse del sistema. Estraggo casualmente un biglietto per decidere chi usa la risorsa.

Se ho processi con priorita piu alta posso assegnare piu biglietti rispetto agli altri, in'oltre questo sistema permette anche di scambiare biglietti per far coperare tra loro i processi

### Scheduling fair-share
Ogni processo e' oggetto di scheduling individualmente. La CPU viene divisa equamente tra gli utenti (o magari posso scegliere che un utente abbia diritto a piu cpu), anche se uno potrebbe avere meno processi di un'altro utente.

Se l'utente uno ha 4 processi: A, B, C, D e l'utente due ne ha uno solo: E e divido la cpu in modo equo, la timeline di esecuzione sara cosi: A -> E -> B -> E -> C -> E -> D -> ...

## Scheduling realtime
Possono essere:
* **Hard** Real-Time: Scadenze da rispettare.
* **Soft** Real-Time: posso tollerare qualche scadenza.
Ho di solito processi il cui comportamento e' prevedibile, e sono noti in anticipo 
Ho degli **eventi** che possono essere: **periodici** o **non periodici**. 

C'e' una condizione da rispettare in un sistema Real-Time:
$$
\sum_{i=1}^{m} \frac{C_{i}}{P_{i}} \leq 1
$$
dove, per $m$ eventi **periodici**:
* $C_i$ e' il tempo di gestione dell'evento $i$-esimo
* $P_i$ e' il periodo con cui l'evento $i$-esimo

Gli algoritmi possono essere:
* **statici**: prima di eseguire so come schedulare
* **dinamici**: durante l'esecuzione effettuo lo scheduling

## Processi e Thread
Nei thread a livello utente potrebbe accadere che un thread del processo in esecuzione occupi tutto il quanto di esecuzione. Bisogna implementare uno scheduler dei thread all'interno del processo (round-robin o a priorita). Il kernel (che esegue lo scheduler) non vede i thread a livello utente, si  orchestrano da soli.