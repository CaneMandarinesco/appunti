Per **multiprogrammazione** si intende l'abilita di un sistema di passare di programma in programma, facendo dei veri e propri salti.

In unix tutti quei programmi che vengono eseguiti in background sono chiamati **daemon**

I processi vengono **creati** in 4 contesti:
* inizializzazione del **sistema**
* esecuzione di una **chiamata di sistema** per la creazione di un processo (`fork()`)
* richiesta dall'utente di **creare un processo**
* inizio di un **job** (identifica uno script o un programma) in **modalità batch** (senza interazione con l'utente)

In **unix** l'unico modo per creare un processo e' usare `fork`. Crea un clone esatto del processo padre, ma dopo questi possono prendere strade separate.
Lo spazio degli indirizzi contiene gli stessi dati, ma la modifica di una variabile in uno non va a modificare l'altro.

Con `execve` posso specificare il programma nuovo da eseguire nel figlio.

quando termina un processo?
* Uscita normale, **volontaria**, senza errori
* Uscita a causa di un **errore**, *potrebbe essere volontaria*
* Errore fatale, e' **involontario**. (es: `segmentation fault`, ossia accesso ad un area di memoria protetta,  nel kernel si chiama `kernel panic`)
* Ucciso da un altro processo: **involontario** (`kill`)

In Unix per esempio il processo `init` viene avviato al boot. Questo a sua volta crea altri processi tanti quanti sono i terminali aperti sulla macchina, ognuno di questi aspetta che l'utente faccia il login.

In Unix i processi padre non possono diseredare i figli. In Windows non c'e' questo concetto, piuttosto la creazione di un processo ritorna un `Handler` al processo creato.

Stati di un processo:
* **Running**: il processo e' fisicamente nel processore
* **Pronto**: e' stato fermato (***per far eseguire un altro processo***) ma puo' essere eseguito
* **Bloccato**: e' successo qualcosa! Aspetto un **evento estrerno**

Posso entrare nello stato bloccato se richiedo per esempio la lettura di un file.
Seguendo il diagramma: da Running posso andare a Bloccato oppure nello stato Ready.

Lo **scheduler dei processi** decide quando mettere un processo da Running a Pronto. La velocità del processore ci da l'illusione che i processi si

> un processo non puo' dire allo scheduler chi deve essere eseguito dopo di lui. decide il sistema operativo. Esistono degli escamotage, per esempio se il sistema operativo usa dei criteri di priorita' un processo potrebbe modificare le priorita degli altri processi.

La **tabella dei processi** e' un area di memoria gestita dal sistema operativo che contiene informazioni relative ai vari processi in esecuzione (file aperti, PID, tempo di esecuzione, SP, PC), La lettura di questa tabella permette di eseguire un processo da dove era stato fermato.

Il **Vettore degli interrupt** contiene gli indirizzi delle procedure da chiamare per ogni determinato interrupt che il sistema operativo cattura.

### segnali e interrupt
Sono dei meccanismi usati nei sistemi operativi per gestire eventi **asincroni**. 
La differenza principale e' che i segnali sono eventi di natura software, gli interrupt invece sono di natura hardware.

L'**handler** per ogni segnale viene aggiunto dal compilatore di default, oppure posso definirli io.

Gli **interrupt** non sono segnali che vengono dai processi, ma vengono dal sistema operativo che li cattura dall'hardware. Esiste una tabella degli interrupt nel sistema operativo che associa ad ogni segnale cosa deve fare, e chiama lo scheduler.

* gestione **sincrona**: appena arriva gestisco.
* gestione **asincrona**: posso aspettare per gestire l'interrupt.
* interrupt vector: contiene tutte le procedure da chiamare per ogni interrupt

### gestione dei segnali
* `SIGINT` e' un **modo gentile** per chiedere ad un segnale di terminare.
* `SIGKILL` e' più **cattivo**, viene terminato e basta. (meglio evitare, potrei lasciare operazioni di scrittura a meta)

```c
void signalHandler(int signum){
	// gestisci
	printf("Interrupt signal %d ricevuto\n", signum)
	exit(signum)
}

int main() {
	signal(SIGINT, signalHandler);
	while(1){
		sleep(1);
	}
	return 0;
}
```

## Thread
La comunicazione tra segnali e' cpu intensive. E' stato introdotto il concetto di thread. Ogni processo ha $n$ thread in esecuzione.

Consentono una comunicazione e una sincronizzazione semplici. Se cambio tra thread in un processo, *non avviene il cambio di contesto*! (molto vantaggioso). 

Esempio: un server web e' un applicativo, quando arriva una richiesta viene avviato/creato un nuovo processo che genera dei dati per il cliente. Dovrei quindi assegnargli della memoria e inserirlo nello scheduler, per migliaia di richieste questo schema non e' sostenibile. Piuttosto genero dei thread (al caso limite vengono riutilizzati).

Se un processo e' in esecuzione, puo' essere che i thread possono anche essere in stato di **dormienza**. 

Un'applicazione **multihread** sceglie in *che ordine e come eseguire* i **thread**. Pero' potrebbero impicciarsi con i puntatori! Si devono sincronizzare.

I thread condividono la memoria del processo, e lo scambio di informazioni avviene tramite variabili condivise (piuttosto che mandare segnali), e l'accesso a queste variabili deve essere sincronizzato (problema dei 5 filosofi).  

Ciascun thread può chiamare una chiamata di sistema per conto del processo a cui appartiene.

Quindi esiste una distinzione tra informazioni del processo (condivise con i thread) e informazioni dei thread. 

### `pthreads`

> implementazione...


### ma chi gestisce i thread?
Possono essere implementati:
* **nello spazio utente**, nel processo: il processo decide come schedulare i thread (ovviamente, quando il sistema operativo glie lo permette, *secondo lo scheduling*). *Lo switch tra diversi thread, che appartengono allo stesso processo e' molto veloce*.
* **nel kernel**: un processo del kernel gestisce lo scheduling dei thread. Ma comunque i thread appartengono sempre e comunque ad un programma utente. Essendo tutto di proprietà del kernel, questo ha l'abilita di **riciclare** tra loro i thread.
* **implementazione ibrida**

in unix esiste un processo: `kthread` che gestisce tutti i thread. 

> Perché? In java i thread sono gestiti nello spazio utente, ma "recentemente" sono stati spostati e sono gestiti nel kernel. Nello spazio utente se un thread e' in stato di bloccato, **va in blocco l'intero processo** e gli altri thread associati a questo, nel kernel non succede questo.

Nello spazio utente, un **sistema run time** gestisce la tabella dei thread, in particolare il passaggio tra gli stati di pronto, bloccato e ready. L'implementazione nello spazio utente ha alcuni svantaggi: 
* come implemento le chiamate bloccanti (occupano inutilmente la CPU)? Creo delle procedure non bloccanti?
* Per applicazioni CPU bound, i thread non servono veramente, convengono di più per applicazioni che fanno parecchie chiamate di sistema e operazioni IO, ma bisogna gestire l'alternanza di più thread.
* impossibile un implementazione di scheduling tipo **round-robin**

Nell'implementazione lato kernel invece *non c'e' bisogno di avere più scheduler dei thread per ogni processo, e nemmeno di avere più tabelle dei thread*. Ho un unico scheduler che guarda un'unica tabella dei thread e può decidere qual'ora uno si blocchi chi eseguire dopo (un thread dello stesso processo o un'altro thread). Qui pero' tutte le procedure che potrebbero essere **bloccanti** (che nello spazio utente erano gestite da un sistema runtime) *sono tramutate in chiamate di sistema.*

Comunque pero' viene lasciata libera interpretazione a vari problemi, come per esempio: quale thread si dovrebbe occupare di gestire i segnali?
### implementazioni ibride
lasciamo perdere...

# Concetti fondamentali
* **Cos'e un processo**? E' l'astrazione delle'esecuzione di un programma. Questo contiene lo spazio degli indirizzi del programma, ossia la definizione dell'area dello **Stack**, dei **Dati** e del **Testo** del programma. Altre informazioni fondamentali contenute sono: i **file aperti**, tempo di esecuzione, PID, UID del proprietario, processi figli, numero di thread, **segnali**, **interrupt**, e lo stato **dei registri del processore**. Quest'ultimo e' utile per permettere l'esecuzione "parallela" dei processi.
* **Quando viene creato un processo**? Inizializzazione, chiamate di sistema per la creazione di processi, richiesta utente (tramite bash) e avvio di un lavoro in batch.
* **Quando viene terminato un processo**? Normale, errore, errore fatale, ucciso da un altro processo. Possono essere terminazioni volontarie o involontarie.
* **Segnale**: inviati in asincronamente, sono segnali custom che possono essere inviati e controllati dal programmatore. Possono **essere generati dal SO** o da un'altro processo.
* **Interrupt**: segnali di origine hardware, l'interrupt vector contiene le procedure associate ad ogni interrupt.
* **Esecuzione di un segnale**: il kernel riceve e invia il segnale, viene fermata l'esecuzione del codice (salvo i registri ecc...), **eseguo l'handler del segnale**, ripristino il contesto.
* **Cos'e' un thread**: Rappresenta la vera e propria esecuzione del processo. Posso avere diversi thread associati ad un unico thread. Di conseguenza **ogni thread a un suo Stack, Registri hardware  e Stato**. Altre informazioni come Spazio degli indirizzi invece sono di proprietà del processo. **Il processo esegue le syscall per conto del thread.**



