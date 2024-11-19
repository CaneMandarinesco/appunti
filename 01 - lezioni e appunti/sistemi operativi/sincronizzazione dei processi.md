---
cssclasses:
  - wide-page
---

I processi molte volte hanno bisogno di **comunicare**: condividere dei dati. I processi hanno bisogno quindi di **sincronizzarsi**, e spesso nascono dei problemi di accesso alla memoria: **race conditions**. 
I thread possono **facilmente condividere dei dati**, ma anche thread di processi diversi. 
Un problema comunque e' quando per esempio un processo si aspetta di leggere un certo tipo di dato in una variabile, ma un'altro processo ha modificato quella variabile, compromettendo il funzionamento dell'altro processo. Potrebbe anche succedere che un processo legge una variabile mentre un'altro sta scrivendo.

Bisogna introdurre delle operazioni **atomiche** per evitare le race conditions. 
Alcuni requisiti per evitare le race conditions:
* due processi non possono entrare insieme nelle **regioni critiche** del codice: *risorse a disposizioni di tutti*
* non devo fare **ipotesi** sulla velocità e sul numero di CPU nel sistema: ossia affidarsi al "fato".
* nessun processo in esecuzione al di fuori della propria regione critica può **bloccare altri processi**.
* nessun processo deve aspettare all'**infinito** per l'accesso ad una regione critica.

> Per riassumere: se un processo sta scrivendo un dato, gli altri devono **aspettare la fine della scrittura**!

Alcune soluzioni al problema, **inaccettabili**:
* **disabilitare interrupt** del processo: potrebbe accadere che li disattiva e poi non li riattiva piu! Inoltre su una cpu multicore, dovrei disattivare gli interrupt di tutti i core.
* Usare delle variabili di **lock**: perché ora le race conditions potrebbero accadere sulle variabili che regolano l'accesso.

L'uso delle regioni critiche non sempre e' ottimale: un processo entra in regione critica ma non deve fare niente, e un'altro processo ha necessita di entrare!

>[!note] Busy Waiting
> un processo $B$ chiede di aspettare ad $A$ se ha finito di usare la regione critica. $B$ quando viene eseguito usa tutta la sua CPU per chiedere ad $A$ se ha finito. Bisogna evitare questi fenomeni perche' spreco CPU per delle **istruzioni inutili**.
### Alternanza rigorosa
```c
// -- processo A --
while(TRUE) {
	while(turn != 0);
	critical_region();
	turn = 1;
	noncritical_region();
}

// -- processo B --
while(TRUE) {
	while(turn != 1);
	critical_region();
	turn = 0;
	noncritical_region();
}
```
Non e' molto efficiente, utilizza il busy waiting, in questo caso si parla di **spin lock**. Andiamo ad implementare il **Peterson's algorithm** che e' leggermente meglio

### Peterson's Algorithm
Alice e Bob vogliono usare un'unica postazione in un ufficio con delle regole:
* una persona usa un computer per volta, Alice e Bob segnalano il loro interesse uno alla volta.
* se entrambi sono interessati, **il primo a iscriversi** sul foglio delle prenotazioni entra
* **chi non ha precedenza aspetta** che l'altro finisce
* quando uno finisce di usare il computer, lo **segnala**, e l'altra persona può iniziare.
```c
#define N 2
int turn;                       // indica di chi e' il turno
int interested[N];

void eneter_region(int process){
	int other;
	other = 1 - process;       // ottieni automaticamente chi e' l'altro
	interested[process] = TRUE;
	turn = process;            // e' il mio turno
	while (turn == process && interested[other] == TRUE); // aspetto con busy waiting
}

void leave_region(int process) {
	interested[process] = FALSE;
}
```


### Soluzione con `TSL`
L'istruzione `TSL` e' presente in molte CPU. Permette di leggere un'indirizzo di memoria, generalmente e' una variabile `lock`, e di impostare il suo valore a un numero diverso da zero. Il valore letto viene messo dentro un registro in modo da controllarlo:
* Se era 0: allora posso entrare in regione critica
* Se non era 0: allora qualcuno e' in regione critica
La lettura e la scrittura della variabile lock sono operazioni **indivisibili**, il bus viene bloccato durante l'accesso a questa variabile.
### Come evitare i busy waiting?
Le soluzioni viste fino ad ora usano il **busy waiting**, anche se l'algoritmo di Peterson limita il busy waiting al caso in cui richiedo la regione critica. Soluzione:
```c
void sleep(){
	set own state to BLOCKED;
	give CPU to scheduler;
}

void wakeup() {
	set state of process to READY;
	give CPU to scheduler;
}
```
I processi rilasciano volontariamente la CPU allo scheduler se non possono entrare in regione critica!

### Problema produttore-consumatore

Nel problema due processi: produttore e consumatore condividono un buffer. Il produttore inserisce informazioni nel buffer mentre il consumatore le preleva. Il produttore inserisce informazioni nel buffer, mentre il consumatore preleva.
* Il produttore si addormenta se il buffer e' pieno e viene risvegliato quando il consumatore preleva dati
* Il consumatore si addormenta se il buffer e' vuoto e viene risvegliato quando il produttore inserisce dati nel buffer.

```c
#define N 100
int count=0;

void producer(void){
	int item;
	while(TRUE) {
		item = produce_item();
		if(count==N) sleep();        // dormi se il buffer e' pieno
		insert_item(item); 
		count ++;
		if(count == 1) wakeup(cons); // sveglia il consumatore
	}
}

void consumer(void) {
	int item;
	while(TRUE) {
		if(count == 0) sleep();      // dormi se buffer e' vuoto
		item = remove_item();
		count--;
		if(count==N-1) wakeup(prod); // sveglia il produttore
		consume_item(item);
	}
}
```

Questo modello funziona per **costruzione**, ma esiste la seguente situazione sfavorevole:
* **consumer**: `count=0` e chiamo `sleep`.
* **producer**: comincia a produrre, e fa `wakeup` ma qualche millisecondo prima della chiamata di `sleep`! Potrebbe accadere per colpa dello scheduler che fa semplicemente il suo dovere.
* il consumatore dorme per sempre.

Per risolvere il problema potrei inserire un bit di **accumulazione** per lo stato del segnale di sleep, cosi una volta inviato, prima o poi verra' ricevuto. ma e' un `porkaround`. Piuttosto uso il **semaforo**.

### Semaforo
Creati da Dijkstra per gestire i wake up. Ho un **contatore** che ha due stati, per gestire il `wakeup:
* 0: non devo svegliare
* **positivo**: ho **uno o piu** wake up in attesa.

Sul contatore posso fare due operazioni:
* **down**:  
	* Se il valore del semaforo e' maggiore di zero, questo valore viene decrementato e il **processo continua la sua esecuzione**. 
	* Se il valore e' 0 allora chi invoca `down` viene fermato e **messo in coda nel semaforo**
* **up**:
	* Se il valore e' 0, ci sono processi nella coda di attesa, vengono svegliati.
	* Il valore viene incrementato e il processo continua la sua esecuzione.

Per costruzione sono **indivisibili**, **operazioni atomiche**. L'operazione e' atomica per come e' implementata in assembly.

### Semafori per produttore-consumatore
* il semaforo `mutex` regola l'accesso alla regione critica
* i semafori `empty` e `full` regolano l'accesso al buffer.
I semafori possono essere modificati grazie alle operazioni:
* `up`: incrementa il semaforo, se da 0 porta il valore ad 1, vuol dire che un'altro thread può andare in regione critica, viene scelto dal sistema operativo.
* `down`: decrementa il semaforo, se e' già 0 allora viene messo in coda nel sistema operativo per entrare in regione critica.
```c
#define N 100

typdef inc sema;
sema mutex=1;
sema empty=N, full=0;

void producer(void) {
	int item;
	while(TRUE) {
		item = produce_item();
		down(&empty);        // decrementa empty, se empty e' 0 allora vengo bloccato, devo aspettare che il consumer faccia up(empty)
		down(&mutex);        // l'accesso ad item e' mutualmente esclusivo
		insert_item(item);   // codice critico
		up(&full);           // incrementa full, se da 0 passa ad 1, allora il consumer inizia a leggere
		up(&mutex);          // rilascia la regione critica
	}
}

void consumer(void) {
	int item;
	while(TRUE) {
		down(&full);          // blocca se il buffer e' vuoto
		down(&mutex);         // l'accesso ad item e' mutualmente esclusivo
		item = remove_item(); // codice critico!
		up(&mutex);           // rilascia l'accesso alla regione critica
		up(&empty);           // rilascia l'esecuzione per il producer, incrementa empty
		consume_item(item);   // non e' codice critico
	}
}
```

* Il thread del consumer si blocca quando il buffer e' vuoto
* il thread del producer si blocca quando il buffer e' pieno
* un thread blocca l'altro quando uno accede alla regione critica.

Per rendere chiaro il funzionamento, una chiamata di `down` decrementa il semaforo. Se avviene un passaggio da 1 a 0 allora posso entrare in regione critica, ma se ho un valore negativo vuol dire che devo essere messo in coda per entrare in regione: il sistema operativo sceglie quando svegliarmi.
Quando un processo smette di usare una risorsa dopo che ha fatto un down, deve eseguire un up.

In questo caso i semafori sono utilizzati anche per regolare la lettura e scrittura nel buffer: e' inutile leggere se il semaforo full e' 0, ed e' inutile scrivere se empty e' 0. Di conseguenza uno tra produttore e consumatore vengono messi in sleep finché l'altro non legge o scrive nel buffer, liberandolo o riempiendolo.

> Nei semafori, l'uso di istruzioni `TSL` e' utile in sistemi a più core. 
> Le istruzioni `down` e `up` sono chiamate di sistema (implementate grazie a delle librerie) che permettono di fare operazioni atomiche sui semafori

### Problema lettori e scrittori con i semafori
Nel problema dei lettori e scrittori ho:
* $N$ processi accedono ai dati
* In qualsiasi momento ho $R$ lettori, ma solo **1 scrittore**.
L'obbiettivo e' permettere quindi a più processi di leggere su un database, ma solo 1 alla volta scrive. Il funzionamento e' il seguente:
* Il primo lettore **blocca** l'accesso al database (in scrittura)
* Lettori successivi **incrementano un contatore** (ossia il semaforo)
* L'ultimo lettore libera l'accesso al database per gli scrittori.
Soluzione al problema:
```c
typdef int sema;
sema mutex = 1;
sema db = 1;
int rc = 0;

void reader() {
	while(TRUE) {
		down(&mutex); // faccio down, se == 0 posso accedere
		rc++;
		if(rc==1) down(&db); // l'accesso al semaforo db deve essere 
		                     // sotto regione critica
		                     // rc == 1 vuol dire che posso leggere
		up(&mutex);
		read_db();    // leggi il database
		down(&mutex);
		rc--;
		if(rc==0) up(&db);
		up(&mutex);
		use_data_read();
	}
}

void writer() {
	while(TRUE) {
		think_up_data();
		down(&db);  // accedi al db se db = 0
		write_db();
		up(&db);    // rilascia
	}
}
```


### Mutex e threads
I **mutex** sono una versione semplificata dei semafori. E' una variabile, spesso un intero, che posso modificare con le istruzioni:
* `mutex_lock`: imposta il mutex a 1. se il mutex era sbloccato entro in regione critica, altrimenti vengo bloccato dal sistema operativo.
* `mutex_unlock`: imposta il mutex a 0, sbloccalo.
L'implementazione di queste istruzioni e' fatta in assembly grazie all'istruzione `TSL`. Inoltre nella chiamata `mutex_lock`, se non posso entrare in regione critica, faccio una chiamata allo **scheduler di thread in spazio utente** (`thread_yeld`).

> L'implementazione dei mutex può essere fatta nel kernel, ma la modifica richiede l'uso di chiamate di sistema.