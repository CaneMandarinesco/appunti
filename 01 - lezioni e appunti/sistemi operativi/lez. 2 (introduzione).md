* Dimensione software (dall'alto verso il basso)
	* **Processi**
	* **User interface program**: mette a disposizione delle librerie per l'utente, per interfacciarsi con l'**OS**.
	* **Sistema operativo**: traduce istruzioni che arrivano dai livelli piu alti, per l'hardware. Mette a disposizione le risorse hardware, e funge da **arbitro** per i processi e gli utenti. I processi devono essere eseguiti in modo **esclusivo** (*mappatura della memoria*).
		* Deve gestire più utenti (per esempio in un **server**) e processi
		* Deve gestire le risorse: **tempo** (CPU, stampante) e **spazio** (memoria, disco)
* Dimensione hardware

Nel processore c'e' un **flag** che indica se sto eseguendo codice utente o del sistema operativo, hanno diversi privilegi di accesso alla memoria.

### esempio di chiamata
Per esempio quando eseguo una lettura, chiamo una funzione del sistema operativo: `read`. Ora e' il sistema operativo che e' in esecuzione (il processo originale e' in pausa, e lo stato dei registri viene salvato in memoria), ha dei diritti di accesso speciali per la lettura. Quando il sistema operativo finisce, *ripristina lo stato del processore* (registri) e viene rieseguito il processo originale. Avviene un **cambio di contesto**: *questo cambio costa risorse*.

## breve storia!!!!
1. **valvole termoioniche**, non esiste un sistema operativo. i computer ce mettevano i secondi a calcolare qualcosa.
	* programmazione manuale tramite **linguaggio macchina**
	* vari secondi per fare un calcolo
	* Negli anni 50 si passa schede perforate. 
2. **transistor e sistemi batch**: enormi e grosse, ma eseguono in termini di **batch o job**.  
	* Una **macchina legge** (mette il programma su disco magnetico ??), un'altra **esegue i calcoli** e un'**altra li stampa**: l'organizzazione a pipeline e' utile per condividere la macchina.
	* generalmente queste macchine sono dei mainframe
	* grazie ai transistor diventano piu affidabili e vengono usati all'universita

il programma batch e' diviso in diverse schede (aree di memoria):
* JOB: tempo massimo di esecuzione
* FORTRAN: compilatore fortran
* Programma
* LOAD
* RUN: esegue con dei parametri specificati
* END

3. IC e Multiprogrammazione (anni 70): nascono i primi sistemi operativi, che cercano di rendere il codice portabile tra diversi sistemi. Nasce il concetto di **famiglia di computer**, le versioni dopo sono compatibili con quelle prima.
	* memoria per job multipli
	* computer portatili, grazie ai circuiti integrati.
	* **time sharing**, `mulitics`: uno dei primi sistemi operativi, era molto complesso, aveva caratteristiche innovative utilizzate ancora oggi.
	* **unix**: inizialmente era una versione modificata di `multics` ma monoutente. la terza versione e' scritta interamente in `C`
	* nascono diversi sistemi operativi: nasce lo standard `POSIX` (e altri)
	* `minix`: variante di `unix`, sistema operativo di Tanenbaum, creata per gli studenti. solo `11800` righe di codice. `Linux` nasce come variante di `minix`

### il processore!
* `PSW`: contiene informazioni sullo stato del processore, fondamentale per `I/O`
* `SP`: puntatore alla cima dello stack, area di memoria che contiene procedure, parametri e variabili locali.
* `PC`
* il multiplexing temporale della CPU e' gestito dal sistema operativo.
* e' organizzato a pipeline, il processore decodifica piu istruzioni insieme e le mette dentro un buffer in attesa di essere eseguite.
* le architetture spesso presentano 2 o piu processori nello stesso socket.
* cache: L1, L2, L3 ecc... Possono essere condivise tra i vari core o essere privati.

### dispositivi i/o
* due componenti
	* controller: si interfaccia con il SO grazie al proprio **dirver**.
	* dispositivo: complicata da pilotare per un computer qualsiasi
* il driver **mappa** il controller alla memoria, e permette di fare operazioni di tipo `in/out`.
	1. il processo fa una chiamata di sistema
	2. kernel chiama il driver
	3. driver effettua operazione di I/O, lo interroga per vedere se ha completato l'operazione o gli chiede di inviare un **interrupt** quando ha finito
* oppure fa una **DMA**: il controller accede alla memoria del computer senza parlare con la CPU.

### bus
un sistema moderno ha diversi bus, con velocità di trasferimento di versi. 
* DDR4/5 per la memoria
* PCIe per le periferiche esterne (come scheda grafica)
	* fatta di connessioni punto punto

### avvio del sistema (boot)
* una memoria sulla scheda madre contiene il **firmware**
* scansiona le periferiche, inizializza ram e periferiche
* avvia i servizi base per i/o
* il BIOS cerca la tabella delle partizioni per avviare il boot-loader
* il boot-loader avvia il sistema operativo