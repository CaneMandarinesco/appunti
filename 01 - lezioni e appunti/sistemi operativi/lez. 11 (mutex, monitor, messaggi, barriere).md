### introduzione al mutex
Un mutex e' una versione esplicita e semplificata dei semafori, si usa quando non o bisogno di contare gli accessi alla variabile. Puo avere infatti due stati:
* **locked**, che si imposta con `mutex_lock`, richiedo l'accesso alla regione critica
* **unlocked**, che si imposta con `mutex_unlock`, esco dalla regione critica, libero la risorsa.

Il mutex non usa **busy waiting**, viene usata all'interno del mutex la chiamata `thread_yield` per dire alla CPU di rilasciare il thread e di passare ad un altro. Questi vengono implementati grazie alle istruzioni macchina `TSL` e `XCHG`, queste permettono di modificare e leggere la variabile di lock in un'unica operazione atomica (questo avviene perché il bus durante un `TSL` viene bloccato). 

### `pthread`
`pthread` e' una libreria che fornisce funzioni di sincronizzazione dei thread.

| chiamata                | descrizione                                                                 |
| ----------------------- | --------------------------------------------------------------------------- |
| `pthread_mutex_init`    | Inizializza un oggetto mutex.                                               |
| `pthread_mutex_destroy` | Rilascia le risorse associate al mutex.                                     |
| `pthread_mutex_lock`    | Blocca un mutex, sospende l'esecuzione se questo e' gia bloccato            |
| `pthread_mutex_trylock` | Tenta di bloccare il mutex, ma evita di sospendere l'esecuzione del thread. |
| `pthread_mutex_unlock`  | rilascia il mutex, deve essere chiamato dal thread che detiene il mutex.    |

### semafori o mutex?
Quindi cosa e' meglio usare? Dipende:
* Un mutex e' usato per **garantire l'esclusione mutua**. Protegge l'accesso ad una risorse ed non ammette per costruzione più thread vi accedano nello stesso momento.
* Il Semaforo può essere usato per controllare l'accesso ad una risorsa condivisa ma e' **più versatile**, può essere usato per altri scopi di sincronizzazione
* Un mutex e' di proprietà di un singolo processo, gli altri possono solo leggerlo. il semaforo invece può' essere modificato da tutti.

Possono sorgere comunque dei problemi se uso un mutex:
* se due thread accedono ad una variabile condivisa il mutex ne blocca una.
* ma se un thread aspetta che quella variabile raggiunga un certo valore? Il mutex non mi garantisce la sincronizzazione del codice

### variabili condizionali
Le variabili condizionali (`pthread_cond`) permettono di sincronizzare i processi basandosi su delle condizioni:
* permettono di gestire i thread in base a degli eventi (es: una variabile cambia valore, allora il thread riparte)
* nel problema produttore-consumatore questo sistema mi permette di notificare quando il produttore aggiunge dei byte nel buffer.
* con la chiamata `pthread_cond_wait` su una variabile condizionale finché una condizione specifica non e' soddisfatta.


| chiamata                 | descrizione                                                                                                       |
| ------------------------ | ----------------------------------------------------------------------------------------------------------------- |
| `pthread_cond_init`      | Inizializza una variabile condizione `pthread_cond_t`. La variabile viene associata ad un mutex                   |
| `pthread_cond_destroy`   | Distruggi una variabile condizione, libera le risorse associate. Da chiamare quando non ci sono thread in attesa. |
| `pthread_cond_wait`      | Blocca il thread chiamante in attesa di una condizione.                                                           |
| `pthread_cond_signal`    | Risveglia un thread in ascolto su una variabile condizione.                                                       |
| `pthread_cond_broadcast` | Risveglia tutti i thread in attesa su una condizione.                                                             |

codice...


### Monitor

La comunicazione tra processi usando `mutex` non e' sempre semplice, bisogna fare attenzione per evitare **race conditions** e **dead lock**. I monitor sono meglio! Sono dei costrutti propri di un linguaggio di programmazione: permettono di raggruppare procedure, variabili e strutture dati. Di conseguenza i processi possono chiamare i metodi dei monitor che garantiscono la mutua esclusione (il tutto con l'aiuto del compilatore, che e' in grado di ottimizzare il codice e riduce gli errori del programmatore)

I monitor hanno due segnali:
* `wait`
* `signal`

Linguaggi come Java supportano i monitor. Vengono implementati grazie alla funzioni dichiarate come `syncrhonized` in modo che solo un thread possa accedervi.  

### Monitor per produttore-consumatore
Andiamo a implementare la struttura monitor che gestisce l'inserimento e la rimozione di elementi dal buffer.
```c
monitor ProdCons {
	condition full, empty;
	int count=0;
	void enter(int item) {
		if(count==N) wait(full);
		insert_item(item);
		count++;
		if(count==1) signal(empty);
	}

	void remove(int* item){
		if(count==0) wait(empty);
		*item = remove_item();
		count--;
		if(count==N-1) signal(full); 
	}
}
```

ora il codice di producer e consumer:
```c
void producer() {
	int item;
	while(TRUE){
		item = produce_item();
		ProdCons.enter(item);
	}
}

void consumer() {
	int item;
	while(TRUE) {
		ProdCons.remove(&item);
		consume_item(item);
	}
}
```

Rispetto al meccanismo `sleep/wakeup` (visti in precedenza) le chiamate `wait/signal` sono protetti dalla mutua esclusione all'interno del monitor.

In `C` non esiste il costrutto dei monitor, ma devono essere implementati in altri modi. I monitor sono dei costrutti propri di linguaggi di alto livello.


### Messaggi
Sono fondamentali nello studio delle reti, ma presentano molti problemi per quanto ne riguarda la gestione:
* Possono essere persi
* Per evitare di essere persi ho bisogno di mandare piu volte lo stesso messaggio, come distinguo messaggi duplicati?
* Devo verificare l'autenticità del pacchetto.

Si implementano con le chiamate: `send` e `receive`.

```c
void producer(void) {
	int item;
	message m;

	while(TRUE) {
		item = produce_item();
		receive(consumer, &m); // asppetta un buffer vuoto
		build_message(&m, item);
		send(consumer, &m);    // spedisci il messaggio
	}
}

void consumer(void) {
	int i;
	message m;

	for(int i = 0; i < N, i++) send(producer, &m); // manda N buffer vuoti
	while(TRUE) {
		receive(producer, &m); // prendi il buffer pieno
		item = extract_item(&m);
		send(producer, &m);    // manda il buffer vuoto
		consume_item(item);
	
	}
}
```

### Barriere
Quando un processo raggiunge una barriera, si blocca finché' non la raggiungono tutti gli altri processi coinvolti. Riguarda piu processi.

### Read-Copy-Update
...