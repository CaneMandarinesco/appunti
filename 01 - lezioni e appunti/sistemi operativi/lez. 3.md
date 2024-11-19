### diversi tipi di sistemi operativi
* Mainframe:
	* Batch processing: esecuzione di processi senza interazione con utente
	* Elaborazione di transazioni: gestione di numerose richieste
	* **Time-sharing**: diversi utenti si collegano remotamente alla stessa macchina
* Server:
	* forniscono: File sharing, database, stampa, hosting, ecc...
	* esempi: Linux, Windows Server, FreeBSD e Solaris.
* Smartphone e Tablet
* per **Personal Computer**:
* **IoT**
	* os in esecuzione su piccoli circuiti (frigoriferi, telecamere, smartwatch)
	* Embedded Linux, QNX, TinyOS
* **Real Time**: processi con scadenze di tempo. Hard o Soft Realtime.
* **SmartCard**:
	* sono Java oriented.

In generale un sistema operativo nasconde l'implementazione hardware e le astrae in oltre le gestisce: protegge la macchina stessa da un uso dannoso e scorretto (sia un buona fede che in cattiva fede).

Un sistema operativo offre funzionalità attraverso le **chiamate di sistema**, che implementano dei servizi (es. il File System e la Gestione dei Processi). Mette a disposizione i processi per astrarre l'esecuzione fisica di un programma, e lo mette a disposizione dell'utente, ne astrae anche la memoria creando un suo **spazio di indirizzamento virtuale**. Inoltre generalmente un Sistema Operativo permette di gestire dei file.
## Processo
* ha un suo **spazio degli indirizzi**
* un insieme di **risorse** assegnate: registri, file, allarmi,...
* e' un **contenitore** di informazioni, che servono per gestirne l'esecuzione.
#### spazio indirizzi: layout
* **stack**: contiene gli indirizzi delle procedure/chiamate
* **data**: contiene le variabili
* **text**: contiene il programma eseguibile
#### cliclo di vita di un processo
* `child`: viene creato tramite una duplicazione, rispetto al padre.
* il sistema operativo tiene una **astrazione**, tipo grafo ad albero della parentela dei processi.

#### proprieta
* il processo e' proprieta' di un utente, identificato col suo **UID**
* gli utenti possono appartenere ad un **GUID**

## File
* astrazione del disco fisico
* leggo fornendo al driver: indirizzo di lettura e quanti byte devo leggere
* in unix ogni cosa viene astratta come un file
* gerarchia: `/` radice
* accedo ai file tramite **path assoluta** o **relativi**
* posso montare altri *file system* nella **root**

### Diritti di accesso
i file sono protetti in base a: proprietario, gruppo e altri utenti.
* `r`: read
* `w`: write
* `x`: execute

* primi 3 bit: proprietario
* seconda tupla di 3 bit: gruppo
* terza tupla di 3 bit: altri utenti
```bash
-rwxr-x--x myuser mygroup
```

> ho altri caratteri in base al tipo di risorsa che il file astrae.

> formattazione: come sono organizzati i dati