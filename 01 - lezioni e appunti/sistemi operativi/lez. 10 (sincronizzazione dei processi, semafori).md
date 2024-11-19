---
cssclasses:
  - wide-page
---

I processi molte volte hanno bisogno di **comunicare**: condividere dei dati. I processi hanno bisogno quindi di **sincronizzarsi**, e spesso nascono dei problemi di accesso alla memoria: **race conditions**. 
I thread possono **facilmente condividere dei dati** (in quanto eseguono lo stesso programma??), ma anche thread di processi diversi. Un problema comunque e' quando per esempio un processo si aspetta di leggere un certo tipo di dato in una variabile, ma un'altro processo ha modificato quella variabile, compromettendo il funzionamento dell'altro processo. Potrebbe anche succedere che un processo legge una variabile mentre un'altro sta scrivendo.

Bisogna introdurre delle operazioni **atomiche** per evitare le race conditions:
* due processi non possono entrare insieme nelle **regioni critiche** del codice: *risorse a disposizioni di tutti*
* non devo fare **ipotesi** sulla velocità e sul numero di CPU nel sistema: ossia affidarsi al "fato".
* nessun processo in esecuzione al di fuori della propria regione critica può **bloccare altri processi**.
* nessun processo deve aspettare all'**infinito** per l'accesso ad una regione critica.

> Per riassumere: se un processo sta scrivendo un dato, gli altri devono **aspettare la fine della scrittura**!

Alcune soluzioni al problema, **inaccettabili**:
* disabilitare interrupt del processo: non può essere bloccato!
* Bloccare le variabili con variabili 0/1 (true e false): perché ora le race conditions potrebbero accadere sulle variabili che regolano l'accesso.

L'uso delle regioni critiche non sempre e' ottimale: un processo entra in regione critica ma non deve fare niente, e un'altro processo ha necessita di entrare!
>[!note] Busy Waiting
> un processo $B$ chiede di aspettare ad $A$ se ha finito di usare la regione critica. $B$ quando viene eseguito usa tutta la sua CPU per chiedere ad $A$ se ha finito. Bisogna evitare questi fenomeni perche' spreco CPU per delle **istruzioni inutili**.



### Alternanza rigorosa
>[!multi-column]
>
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
Non e' molto efficiente, andiamo ad implementare il **Peterson's algorithm**

### Peterson's Algorithm
Alice e Bob vogliono usare un'unica postazione in un ufficio con delle regole:
* una persona usa un computer per volta
* se entrambi sono interessati, il primo a iscriversi sul foglio delle prenotazioni entra
* chi non ha precedenza aspetta che l'altro finisce
```c
#define N 2
int turn;                       // indica di chi e' il turno
int interested[N];

void eneter_region(int process){
	int other;
	other = 1 - process;
	interested[process] = TRUE;
	turn = process; 
	while (turn == process && interested[other] == TRUE); // aspetto con busy waiting
}

void leave_region(int process) {
	interested[process] = FALSE;
}
```

### Come evitare i busy waiting?
Le soluzioni viste usano i busy waiting, l'algoritmo di Peterson limita il busy waiting al caso in cui richiedo la regione critica. Soluzione:
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
I processi rilasciano volontariamente la CPU allo scheduler

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
		item = produco_item();
		if(count==N) sleep(); // dormi se buffer e' pieno
		insert_item(item);
		count ++;
		if(count == 1) wakeup(cons); // sveglia il consumatore
	}
}

void consuner(void) {
	int item;
	while(TRUE) {
		if(count == 0) sleep(); // formi se buffer e' vuoto
		item = remove_item();
		count--;
		if(count==N-1) wakeup(prod); // sveglia il produttore
		consume_item(item);
	}
}
```

Questo modello funziona per **costruzione**, ma esiste la seguente situazione sfavorevole:
* **consumer**: `count=0` e chiamo `sleep`.
* **producer**: comincia a produrre, e fa `wakeup` ma qualche millisecondo prima dello `sleep`!
* il consumatore dorme per sempre.

Per risolvere il problema potrei inserire un bit di accumulazione per lo stato del segnale di sleep, ma e' un `porkaround`. Piuttosto uso il **semaforo**.

### Semaforo
Creati da Dijkstra per gestire i wake up. Ho un contatore che ha due stati:
* 0: non devo svegliare
* **positivo**: ho un wake up in attesa.

Sul contatore posso fare due operazioni:
* **down**:  
	* Se il valore del semaforo e' maggiore di zero, questo valore viene decrementato e il processo continua la sua esecuzione. 
	* Se il valore e' 0 allora chi invoca `down` viene fermato e messo in coda nel semaforo
* **up**:
	* ...

Per costruzione sono indivisibili, operazioni atomiche. L'operazione e' atomica per come e' implementata in assembly.

### Semafori per produttore-consumatore
```c
#define N 100

typdef inc sema;
sema mutex=1;
sema empty=N, full=0;
...
```


### Problemi lettori e scrittori
* $N$ processi accedono ai dati
* In qualsiasi momento ho $R$ lettori, ma solo 1 scrittore.
Soluzione al problema:
```c

```


### Mutex e threads
In Linux esiste un'implementazione per mutex e semafori