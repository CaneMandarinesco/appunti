Un computer moderno, consiste di hardware molto complicato: più CPU, varie memorie (RAM e dischi fisici), tastiere, monitor, schede video, ecc... . Per questo motivo i computer sono generalmente equipaggiati con un sistema operativo che gestisce tutte queste periferiche e le fa lavorare insieme.

Il computer ha due modalità di esecuzione:
* **kernel mode**: il codice del sistema operativo viene eseguito in questa modalità senza alcun tipo di restrizione. Ha accesso completo alla memoria e puo' eseguire tutte le istruzioni possibili.
* **user mode**: tutto cio' che non e' parte del sistema operativo viene eseguito in questa modalità. Il set di istruzioni eseguibili e' ristretto, non si puo' accedere a tutti gli indirizzi di memoria senza passare prima per il sistema operativo.

> Una **flag** nel processore indica se sto eseguendo in kernel o user mode. 

Struttura di un computer:
![[Pasted image 20241018122409.png]]
> Nota che: Il sistema operativo viene eseguito direttamente sull'hardware.

> L'**User Interface Program** e' ciò che permette ai programmi utente di interfacciarsi con il Sistema Operativo.

Le funzionalita' dell'User Interface Program sono le Chiamate di Sistema, procedure che possono essere chiamate per richiedere l'accesso ai dispositivi di I/O, o per effettuare letture e scritture in memoria.

Usare le chiamate di sistema comporta un **cambio di contesto** (istruzione **TRAP**: cambio tra modalita' utente e modalita kernel):
* Programma utente chiama Sys. Call, per esempio: leggi input della tastiera
* Il Controllo dell'esecuzione viene passato al Sistema Operativo
* Il Sistema Operativo salva in memoria lo stato del processore, quindi i registri del programma utente
* Il Sistema Operativo esegue la procedura
* Viene ripristinato lo stato originale del processore e Il controllo viene passato al programma utente

Questo cambio di contesto **consuma molte risorse**.

Il codice del sistema operativo, a differenza dei programmi utente, e' molto complesso. E' composto da milioni di righe di codice. (Questo spiega anche perché' i sistemi operativi moderni non sono altro che versioni modificate di OS gia esistenti, non conviene crearne uno da zero, e' troppo complesso).

## Cos'e' un sistema operativo?
E' un software che mette a disposizione un set astratto di risorse

Puo' essere inteso come un'**estensione della macchina**: permette di accedere alle periferiche hardware senza dover preoccuparsi dei dettagli di implementazione di queste, migliorando la qualità del codice utente, che diventa cosi piu' leggibile e piu' corto. 

Puo' essere inteso anche come un **gestore delle risorse** hardware: permette a più programmi di essere eseguiti "contemporaneamente", quindi esiste un meccanismo di **Multiplexing** (condivisione di Memoria e CPU ossia tempo e spazio). Una risorsa multiplexata viene condivisa tra diversi programmi.

Esempio di multiplexing: **stampante**, puo' essere usata da diversi utenti, diversi processi, la sua memoria viene condivisa.

## Breve storia dei sistemi operativi
### 1a generazione: valvole termoioniche
Computer enormi, poco preformanti. La programmazione avveniva attraverso il linguaggio macchina, collegando migliaia di cavi. Generalmente venivano eseguite solo calcoli numerici.

### 2a generazione: transistore e sistemi batch
In transistor permettono di creare macchine piu affidabili, il mercato e le università cominciano a comprare questi enormi computer.
Queste macchine erano programmate per eseguire dei JOB, o comunque un programma BATCH, scritto in FORTRAN.

Il programma per essere eseguito doveva passare per 3 macchine diverse:
* Una macchina leggeva il programma scritto su un foglio forato e lo metteva su un **nastro megnetico**
* Il programma letto veniva dato in input al calcolatore, l'output era messo su un nastro magnetico.
* una macchina stampava l'output preso dal nastro magnetico

Il programma aveva delle sezioni di memoria:
* JOB: tempo massimo di esecuzione
* FORTRAN: contiene il compilatore
* Programma vero e proprio
* RUN: parametri di esecuzione
* END

> Si nota che nasce il concetto di: pipeline (mentre un programmatore fa leggere il programma, un'altro lo esegue, e un'altro ancora legge l'output) e di divisione del programma in aree di memoria.

### 3a generazione: Integrated Circuits
Nascono i primi sistemi operativi, il concetto di **famiglia di computer** (e quindi la necessita di creare sistemi operativi e software compatibili con diverse CPU e memorie).

Ci si rende conto che mentre il computer attende che le operazioni di IO siano completate si puo' cambiare al volo il processo in esecuzione e fare qualcos'altro: **multiprogramming**. Una delle prime soluzioni era quella di partizionare la memoria per diversi job.

Il **time sharing** e' una variante della multiprogrammazione: diversi utenti connessi posso interagire con il sistema operativo dalla shell.

* Il primo sistema operativo a introdurre il **time sharing** e' `multics`, molto complesso ma con caratteristiche innovative che vengono riprese ancora oggi. 
* `unix` invece era una versione **monoutente** di `multics`, la cui 3a versione era scritta interamente in `C`.
* `minix` e' una variante di `unix` creata da Tanenbaum per i suoi studenti. Aveva una codebase relativamente piccola rispetto ad altri sistemi operativi.
* `Linux` nasce come variante di `minix`

Il codice di `unix` e' stato **stuprato** da tante persone, dando vita e' molti sistemi operativi basati su `unix`. Nasce la necessita di imporre uno standard che i sistemi operativi devono rispettare: **POSIX**, definisce le chiamate di sistema che un sistema operativo deve includere, in modo che i programmi siano portabili da un sistema all'altro.

## processore
Una CPU e' organizzata a pipeline: mentre legge un'istruzione ne esegue un'altra.

 Intel introduce il **multithreading**: i thread contengono le informazioni per l'esecuzione di un thread. La CPU puo' cambiare thread nell'ordine dei nanosecondi.
 
Oltre al multithreading, sono state introdotte le CPU multicore, ovvero CPU con diverse unita di elaborazione all'interno, indipendenti l'une dalle altre. (Le GPU fanno massicciamente uso di questo tipo di architettura)

## memorie
I registri della CPU sono le memorie piu veloci che ci sono, ma sono anche molto costose.

Generalmente vale che: più una memoria e' lenta e più il suo costo scende. Per questo motivo le tecnologie più lente sono usate per creare i grandi dischi di archiviazione (esempio: Hard Disk), un disco da 1 TB fatto di registri o di cache sarebbe costosissimo.

Subito dopo i registri, la **cache** e' la memoria più veloce montata su un computer. E' composta di **cache lines**, ogni cache lines contiene all'incirca 64 indirizzi. La cache line 0 contiene gli indirizzi: 0-63, la cache line 1 contiene gli indirizzi 64-123 e cosi' via. Le linee a cui la CPU fa accesso piu di frequente si trovano nella cache L1 (veloce ma piccola), le altre si trovano nella L2 (meno veloce ma piu grande).  Altre CPU potrebbero avere una cache L3 e  il modo in cui queste sono collegate ai core della CPU dipende dall'architettura.

Dopo la cache, la memoria piu' veloce  e' la RAM.

**Esempio di accesso alla cache**: 
* il programma deve leggere un dato oppure un'istruzione, fornisce un'indirizzo
* controllo se l'indirizzo e' salvato nella cache L1
	* se c'e' lo prelevo da qui, evitando di doverlo cercare in RAM
* se non c'e', cerco in L2 ed eventualmente lo porto in L1
	* se c'e' lo prelevo da L2, evito sempre di cercare in RAM

La cache nasce con l'obbiettivo di immagazzinare i dati che piu' probabilmente vengono usati dal programma in esecuzione, evitando di doverli andare a prelevarli dalla RAM su richiesta (**on the fly**)
## Dispositivi IO
un dispositivo di input/output e' composto generalmente da un **controller** e il **dispositivo stesso**. Dato che esistono svariati tipi di dispositivi IO generalmente viene installato un **driver** sul sistema operativo, in modo da fornire al computer gli indirizzi di memoria a cui **mappare** i registri del driver. 

Il driver una volta installato, deve poter essere eseguito in kernel mode. Spesso bisogna riavviare il computer quando viene aggiunto un driver, perché il driver deve essere **linkato** al  kernel.

E' possibile scrivere nei registri del controller grazie a delle operazioni di tipo IN/OUT, disponibili solo in **kernel mode**. Quando faccio una richiesta  al controller, posso:
* chiedere di ricevere un **interrupt** quando l'elaborazione e' finita
* controllare in **loop** lo stato del controller, causando eventualmente altrettante TRAP quanti sono i controlli che faccio (poco efficiente)
* **DMA** (Direct Memory Access): il controller scrive in memoria senza chiedere permesso alla CPU.

>[!note] interrupt
>segnale fisico che arriva alla CPU. segnala che e' successo qualcosa, per esempio: un device ha finito di leggere un file, e' stato inserito un CD, e' stata inserita una chiavetta o e' stato premuto un tasto. Di conseguenza la CPU quando riceve un interrupt deve fermare l'esecuzione del programma corrente e avviare una procedura che gestisca l'interrupt.

Gli interrupt sono gestiti da un interrupt controller. La CPU non e' in grado di gestire da sola gli interrupt, ecco uno scenario possibile:
* arriva un interrupt: `evento1`
* la cpu comincia a elaborare `evento1`
* arriva un'altro interrupt: `evento2`
* la cpu quindi dovrebbe interrompere `evento1` per eseguire `evento2`?

Piuttosto quando la cpu riceve un interrupt, li disabilita, in modo da non poter essere disturbata durante l'esecuzione della procedura. Alla fine della procedura vengono riabilitati gli interrupt. 

## Boot 
Sulla scheda madre un chip contiene il **BIOS**. Questo software scansiona le periferiche, e inizializza la RAM. Avvia anche i servizi base per l'IO: input da tastiera, mouse e video.
Successivamente il BIOS cerca la cartella delle partizioni e avvia il boot-loader. successivamente il boot-loader avvia il sistema operativo.

## Diversi tipi di sistemi operativi
### Mainframe
Enormi computer, sono usati principalmente in banche e web server, o comunque dove c'e'  bisogno di gestire piu richieste da diversi utenti. I sistemi operativi per Mainframe si basano sul **timesharing**.

### Server
Di solito sono come dei personal computer, ma potenti abbastanza da gestire un'alto carico di richieste. Forniscono servizi di File Sharing, Database, stampa, hosting, ecc...  .

* Windows
* OSX
* Linux

### Smartphone e Table
* IOS
* Android

### IoT
Per gestire dispositivi Embedded, come controllori per frigoriferi e telecamere o dispositivi come Smartwatch.
* Embedded Linux
* QNX
* TinyOS

### Realtime
Per gestire applicativi la cui esecuzione dipende dal tempo, per esempio: respiratori per ospedali, droni da combattimento, sensori di rilevazione, ecc... .

### SmartCard
Carte per pagare, una volta alimentate, la maggior parte di queste eseguono un ambiente  basato su Java.

# Concetti  dei sistemi operativi
## Processi
Un processo e' la rappresentazione dell'esecuzione di un programma. Ogni processo ha un **address space** associato, dove e' contenuto il programma da eseguire, le sue variabili e lo stack.

Un Sistema Operativo oggi gestisce l'esecuzione di piu processi insieme, questi sono immagazzinati nella **process table**.

I processi possono creare uno o piu' processi: **child processes** e possono comunicare tra loro grazie alla **interprocess communication**.

Inoltre ogni utente ha un **UID**, e ai processi *viene assegnato l'UID dell'utente* che ha creato il processo. Un utente puo' far parte di un gruppo, che ha un suo GID
## Files
Il Sistema Operativo offre un'astrazione per creare, eliminare e modificare i file, attraverso delle chiamate di sistema. Generalmente esiste una **radice**, ovvero la cartella in cima alla gerarchia delle directory.
Ogni processo ha una current **working directory**. Prima di modificare un file va aperto, e aprire un file richiede un controllo dei privilegi dell'utente, se posso accedere al file, il sistema operativo mi da il **file descriptor**.

In UNIX e' possibile **montare** i file system che risiedono su un CD o una chiavetta USB direttamente sul file system del sistema operativo. Il file system montato diventa navigabile dalla mia **root directory**

In UNIX esistono tipo speciali di file che rappresentano device: **block special files** (permette di leggere blocchi da un disco) e **character special files** (device che leggono o scrivono caratteri volta per volta, come stampanti, modem e porte seriali)

Un'altra importante feature di UNIX e' il **pipe**, con il quale e' possibile connettere due processi: l'output di uno diventa input dell'altro.

### Protezione
In UNIX, ogni file e' protetto usando 9 bit (**rwx-rwx-rwx**):
* 1o gruppo di 3 bit: indica i permessi dell'utente che detiene il file
* 2o gruppo di 3 bit: indica i permessi del gruppo dell'utente
* 3o gruppo di 3 bit: indica i permessi degli altri utenti.
Potrebbe esercizi un'altra tupla di bit iniziale che indica che tipo di file sto trattando.

significato dei bit:
* `r`: read
* `w`: write
* `x`: execute

## System Calls
Il modo in cui funzionano varia da sistema a sistema. Generalmente e' come una procedura qualsiasi, solo che viene eseguita in **kernel mode**.

Quando per esempio in c eseguo:
```c
count = read(fd, buffer, nbytyes);
```

Quello che succede e':
* metto nello stack i parametri, oppure nei registri della cpu
* chiamo la procedura `read`
* `read` mette in un'area  di memoria o in un registro il codice che identifica la System Call da effettuare
* chiamo l'istruzione `TRAP` per andare in **kernel mode**
* Il Sysstem call handler gestisce l'esecuzione della trap
* Una volta finito, ritorno il valore e pulisco lo stack

## Lista di system calls

| chiamata                                                                                                                    | descrizione                                         |
| --------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------- |
| `pid = waitpid(pid, &statloc, options)`                                                                                     | attendi che finisca un figlio                       |
| `s = execve(name, argv, environp)`                                                                                          | sostituisci l'immagine del processo                 |
| `fd = open(file, how, ...);`<br>`s = close(fd);`<br>`s = read(fd, buffer, nbytes);`<br>`s = write(fd, buffer, nbytes);`<br> | apertura, chiusura, lettura e scrittura di un file. |
| `s = kill(pid, signal)`                                                                                                     | manda un segnale ad un file                         |
| `s = time(&seconds)`                                                                                                        | tempo in secondi dal 1 Gennaio 1970                 |
In Windows le chiamate non sono le stesse, e sono anche di più.
# Struttura di un sistema monolitico
## Sistemi monolitici
Il sistema operativo e' un unico **blocco monolitico**, il codice eseguibile e' compilato e linkato in un unico eseguibile. Di conseguenza tutte le procedure sono visibili a tutti. In un sistema non monolitico alcune procedure potrebbero essere nascoste ad alcune parti del sistema operativo.

In questo tipo di sistemi le system calls vengono  richieste mettendo i dati e il codice della call dentro un'area di memoria specifica dal sistema, e poi viene chiamata una TRAP.

Quindi il sistema operativo:
* ha un **programma principale** che invoca la procedura
* Un set di **procedure di servizio** che eseguono le system calls
* Un set di **procedure di utility** che aiutano le procedure di servizio.

Oltre alle system calls esistono in UNIX le **shared libraries**, in Windows (DLL, Dynamic Linked Library), che aggiungono funzionalità al sistema operativo.

Perché la maggior parte dei sistemi operativi sono monolitici:
* e' più facile avere un unico programma piuttosto che diversi servizi connessi tra loro. 
* se voglio disaccoppiare il software, potrebbe essere necessario renderlo meno efficiente (attivare e disattivare eventuali moduli). Dovrei aver bisogno di gestire invio e ricezione di messaggi tra moduli (aggiunge un livello di complessità: quello di comunicazione).
* se non fosse monolitico, alla fine dovrei compilare in un unico eseguibile e collegare tutti i moduli tra loro.
* in un sistema operativo non monolitico, potenzialmente, ogni programma o modulo potrebbe chiamare altre procedure di altri moduli. E' più vulnerabile e sensibile.
* se cambio processo, ho un cambio di contesto ogni volta che eseguo procedure di moduli diversi.

## Microkernels
il sistema operativo e' diviso in micro-kernel e **moduli**, e ogni modulo viene eseguito su un processo diverso. Quindi un errore in uno di questi non farebbe crashare l'intero sistema come succede nei kernel monolitici. Questi sono usati nel real-time, sistemi industriali, sistemi di aviazione, droni, ecc...

*Tutti i moduli e programmi vengono eseguiti in user mode* e sono raggruppati in: User Programs, Servers e Drivers. I Server fanno la maggior parte del lavoro, ad esempio abbiamo il File Server, Process Server, ecc...

## Modello Client-Server
Una macchina funge da cliente, e fa richieste ad una macchina server. Abbiamo uno scambio di messaggi dove il server manda ai cliente le risposte.

## Macchine Virtuali
Per essere possibile, le cpu devono essere **virtualizzabili**. La macchina virtuale e' un programma che viene eseguito in user mode, ma che di fatto deve eseguire sul processore istruzioni in kernel mode. Se provo ad eseguire istruzioni kernel, mentre sono in user mode, queste vengono ignorate.

La virtualizzazione di **tipo 1**, permette di eseguire il sistema operativo direttamente sull'hardware.

La virtualizzazione di **tipo 2**, usa un **Machine Simulator** per dialogare con il Kernel del Sistema Operativo Host per creare i processi, allocare risorse, creare file ed eseguire operazioni in kernel mode. Far passare tutte le istruzioni non e' efficiente, per migliorare le performance esistono dei moduli del kernel che permettono di bypassare la traduzione.

## Container
I container sono un tipo di virtualizzazione leggera, virtualizzo un sistema  usando il kernel dell'host. Quindi host e container devono essere compatibili. Un container e' logicamente separato dal sistema operativo sul quale sta eseguendo: un programma in esecuzione dentro al container non ha modo di comunicare con l'host, non puo' nemmeno usare il suo File system. 

## exokernel
Tipo di virtualizzazione in cui divido le risorse hardware a priori tra le macchine virtuali che voglio eseguire.

## Il mondo secondo C
In `c` ogni file viene compilato in un file **oggetto**, contiene tutte le funzioni definite all'interno del file, ma mancano i riferimenti alle funzioni definite in altri file. Infatti una volta compilato tutti in file oggetti, i file vengono presi dal **linker** e tutti i riferimenti esterni vengono risolti, aggiungendo gli indirizzi per la chiamata delle funzioni.

Prima della compilazione in file oggetto, il **preprocessore** si occupa di gestire i comandi come: `#define`, `#ifdef` e `endif`

