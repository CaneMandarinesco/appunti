> [!note] processo
> e' un programma in esecuzione! E' l'**astrazione** fondamentale in un sistema operativo. Per gestirli dobbiamo essere in grado di **allocare**, **contabilizzare** e **limitare le risorse**.

Ogni processo potrebbe voler richiedere tanti gigabyte di memoria, se ho tanti processi potrei andare oltre il quantitativo di memoria di cui l'hardware dispone, come faccio? 

La cpu quindi deve alternare l'esecuzione di più processi, dovendo cambiare contesto. Si alternano perche' ad ogni istante $t$ in verità sto eseguendo un solo processo.

> Nei sistemi realtime, e' garantito che almeno ogni tot tempo il processo venga eseguito.

### creazione dei processi
vengono creati in 4 contesti:
* inizializzazione del sistema
* esecuzione di una chiamata di sistema per la creazione di un processo (`fork()`)
* ...
* ...

quando termina?
* Uscita normale, **volontaria**, senza errori
* Uscita a causa di un **errore**, *potrebbe essere volontaria*
* Errore fatale, e' **involontario**. (es: `segmentation fault`, ossia accesso ad un area di memoria protetta,  nel kernel si chiama `kernel panic`)
* Ucciso da un altro processo: **involontario**

### gestione dei processi
`fork`: crea un nuovo processo, clonando il processo che chiama `fork`, condivide alcune risorse con il genitore ma per esempio il process id e' diverso dal padre. 

> se il padre inizializza un array, poi eseguo un fork e il figlio modifica l'array, stanno facendo l'accesso ad aree di memoria diverse (viene anche copiata la memoria ram).

* `exec`: permette di eseguire un nuovo processo.
* `exit`: causa terminazione volontaria del processo
* `kill`: invia un segnale


### stati di un processo
* **Running**: il processo e' fisicamente nel processore
* **Pronto**: e' stato fermato ma puo' essere eseguito
* **Bloccato**: e' successo qualcosa!

Posso entrare nello stato bloccato se richiedo per esempio la lettura di un file.
Seguendo il diagramma: da Running posso andare a Bloccato oppure nello stato Ready.

### informazioni associate ad un processo
* ID (PID),

### segnali e interrupt
Sono dei meccanismi usati nei sistemi operativi per gestire eventi **asincroni**. 
La differenza principale e' che i segnali sono eventi di natura software, gli interrupt invece sono di natura hardware.

L'**handler** per ogni segnale viene aggiunto dal compilatore di default, oppure posso definirli io.

Gli **interrupt** non sono segnali che vengono dai processi, ma vengono dal sistema operativo che li cattura dall'hardware. Esiste una tabella degli interrupt nel sistema operativo che associa ad ogni segnale cosa deve fare, e chiama lo scheduler.

* gestione **sincrona**: appena arriva gestisco.
* gestione **asincrona**: posso aspettare per gestire l'interrupt.

* interrupt vector: contiene tutte le procedure da chiamare per ogni interrupt

### implementazione dei processi

fa una serie di cose (operazioni nello stack, parametri, gestione del PC, gestione delle aree di memoria). 

le letture in dal disco, scrivo tramite DMA, i dati in un buffer.

> un processo non puo' dire allo scheduler chi deve essere eseguito dopo di lui. decide il sistema operativo. Esistono degli escamotage, per esempio se il sistema operativo usa dei criteri di priorita' un processo potrebbe modificare le priorita degli altri processi.

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
* nello spazio utente, nel processo
* nel kernel
* implementazione ibrida

esiste un processo: `kthread` che gestisce tutti i thread. Perché? In java i thread sono gestiti nello spazio utente, ma "recentemente" sono stati spostati e sono gestiti nel kernel. Nello spazio utente se un thread e' in stato di bloccato, va in blocco l'intero processo e gli altri thread associati a questo, nel kernel non succede questo.

Comunque nel kernel il thread condivide la memoria del processo a cui appartiene ma lo scheduling e' gestito dal kernel.

### implementazioni ibride
lasciamo perdere...




