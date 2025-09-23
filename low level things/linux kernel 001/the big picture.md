Il codice del kernel e' diviso in 3 parti:
* file system
* kernel (scheduler, syscall, signals)
* memory manager

I file piu importanti ad alto livello sono:
* `init/main.c` avvia il kernel
* `boot.s` (bootloader per il kernel) e `head.s` (inizializzazione kernel in modalita protetta)
* `lib/*` contiene le chiamate di sistema

> [!note] Makefile
> Ci sono 3 Makefile differenti che producono rispettivamente `mm.o`, `fs.o`, `kernel.o`.
> Nota che `boot.s` viene usato per ottenere il file binario `boot`


## boot sector
Se nel primo settore del floppy, i byte `511` e `512` valgono `0x55` e `0xaa` allora  i primi `510` byte sono codice eseguibile.

> **Nota**: se cosi non fosse, il `BIOS` cercherebbe un'altro dispositivo il cui primo settore sia bootabile.

Una volta trovato un settore di boot, viene copiato in una locazione di memoria fissa, e qui vogliamo che venga caricato `boot.S`. Guardando i commenti vediamo che il settore verrà caricato a partire da `0x7c00`.

Il momento in cui comincio l'esecuzione a partire da `0x7c00` **per qualche motivo** vuole copiarsi da solo e dunque spostarsi a `0x9000`. 

`boot.S` inoltre inizializza una semplice `GDT` e `IDT`.l

> **Nota**: il codice assembly e' necessario perche' un compilatore `C` *assume la presenza di uno stack*, che dobbiamo creare usando istruzioni specifiche per l'architettura.
### fase 1
* `head.S`: si lavora in protected mode, inizializzata con una `GDT` e una `IDT` rudimentale. Si lavora con interrupt disabilitati! Si abilita il paging e poi si passa a...
* `init/main.c`, qui viene completato il setup e avviato il kernel

### fase 2
* `init/main.c`: si vuole popolare correttamente la `IDT` prima di attivare gli interrupt.
* `fork`: il kernel si fa `fork` di se stesso! E una copia delle due inizializza tra le varie cose, il **file system**.
* viene generato il processo per la `shell` (`fork+exec`).
* ora possiamo eseguire i programmi che vogliamo.

> **Nota**: fino alla `shell`, tutto il codice e' caricato in memoria dal boot-loader! E si tratta solo di puro codice e istruzioni.

### chiamate di sistema

> **Nota**: il kernel e' un insieme di funzioni e dati comuni a tutti i processi. Dunque non e' di norma un'entita in esecuzione!

Sono implementate usando software interrupt, che vanno riscontrate nella IDT, con la numero `0x80` e alla chiamata il registro `ax` dovrebbe contenere il numero della chiamata di sistema.

> **Nota**: quando viene eseguito del codice kernel, e' il processo utente che lo fa!

### timer interrupt
Il timer interrupt e' il cuore dei sistemi operativi preemptive. Ogni `cpu` ha un timer che puo' essere programmato per emettere segnali periodicamente.

> **Nota**: Di solito, il timer interrupt ha la priorita piu alta!

*Che cosa fa la* `ISR`? 
* viene incrementato il tempo di esecuzione della task corrente.
* Se il tempo di esecuzione eccede un certo limite: qualcun'altro deve essere eseguito.

> **Nota**: la `idle` task viene eseguita quando, non c'e' un processo da eseguire.

> [!note] TLB exception
> Nel 386 vuol dire **page fault**.

