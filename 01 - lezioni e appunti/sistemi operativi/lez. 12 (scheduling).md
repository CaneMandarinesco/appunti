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

