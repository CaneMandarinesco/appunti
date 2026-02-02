>[!note] Unix
>Famiglia di sistemi operativi che implementano tutte un'API **simile** e le stesse decisioni di design:
> * **Everything is a file**: accedo alle risorse come fossero file.
> * **C**: per la portabilita del codice su altre piattaforme.
> * **IPC**: piccoli programmi che comunicano tra di loro.

**BSD** e' stato sviluppato alla Berkley, inizialmente era un insieme di patch per Unix.

>[!note] Linux
>E' un sistema **Unix-Like**, ma non e' Unix.
> * implementa **POSIX**
> * prende idee da **Unix**
> * *ma non e' sviluppato a partire dal sorgente di Unix*.
> * non e' un prodotto commerciale.

Un processo puo' in qualsiasi momento:
* essere in user-space eseguendo codice utente
* essere in kernel-space eseguendo per conto del codice utente
* essere in kernel-space eseguendo per conto di un interrupt

## kernel Linux VS kernel Unix 
* Entrambi sono un'unico e grande file eseguibile (kernel monolitico)
* Unix richiede una MMU. *Linux **può** essere eseguito su sistemi privi di MMU tramite versioni specifiche come **uClinux**, ma il kernel standard presuppone la presenza di una MMU.*


>[!nota] Microkernel
> Nel design del microkernel, invece di avere un'unico processo che rappresenta l'esecuzione del kernel, si hanno piu' server che eseguono lato kernel che comunica attraverso messaggi. Ogni server puo' invocare messaggi.
> * Windows NT adotta un'architettura **ibrida**, ispirata al design microkernel ma con *componenti monolitici*.
> Linux non e' basato su microkernel ma: supporta thread lato kernel e i kernel modules (binari caricati dinamicamente nel sistema)

* supporto per SMP
* scheduling preemptive nel kernel
* thread kernel e lato utente sono uguali: il sistema operativo non vede differenza tra i due.
* e' gratis, e lo sara' per sempre.

## versioni del kernel
Ci sono due versioni: **stable e development**.

>[!note] versioni
> In `2.6.26.1`
> * `2` indica la **Major version**
> * `6` indica la **Minor version**
> * `26` indica la **revisione**
> * `1` indica la versione stabile 

## patching
I cambiamenti effettuati al kernel vengono condivisi tramite patch incrementali

```bash
patch -p1 < ../patch-x.y.z
```
## configurare, compilare e installare il kernel
Bisogna definire le `CONFIG_FEATURE`, es: `CONFIG_SMP`.
Una config feature puo' essere:
* `booleana`
* `tristate`: yes, no, module.
* `interi` e `stringhe` che non controllano il processo di build.

`module` indica che la feature e' da compilare come modulo, un oggetto caricabile dinamicamente separato dal kernel.

per facilitare la configurazione si usano le  utility `make config`, `make menuconfig` e `make gconfig`.

Con `make defconfig` si genera la configurazione standard per l'architettura in esecuzione.

`make oldconfig` si usa per aggiornare la configurazione dopo aver modificato il `.config`.

>[!note] `CONFIG_IKCONFIG_PROC`
> L'opzione comprime e carica in `/proc/config.gz` la configurazione del kernel in esecuzione.
> `zcat /proc/config.gz > .config` e `make oldconfig` per caricarla!

* per compilare: `make`.
* per ridurre il `build noise`: `make > ../detritus` oppure `make > /dev/null`.
* di solito si fa `make -jn` dove `n` e' il doppio dei processori nella macchina.

Dopo aver compilato il kernel:
* bisogna copiare l'immagine di boot nel posto giusto per il bootloader
* `make modules_install`

in `System.map` si trovano le associazioni tra indirizzi di memoria e funzioni / nomi di variabili utili per il debug.

## Programmare il kernel
Bisogna notare che:
* non si ha accesso alla C library e agli header di C
* si usa GNU C
* non c'e' protezione di memoria
* non si possono eseguire facilmente operazioni floating point
* ogni processo ha uno stack di dimensione fissa.
* bisogna pensare alla sincronizzazione (SMP, interrupt, preemptive)
* il codice deve essere portabile

Non si usa la C standard library perche:
* **troppo grande e spesso inefficiente**
* alcune funzioni della C std lib sono implementate nel kernel.

Si usa `printk` al posto di `printf` per scrivere nel buffer del log di sistema, questa permette anche di specificare una priorità al log.

La GNU C extension e' inclusa nel compilatore gcc.
### funzioni inline
Il compilatore gcc supporta funzioni **inline**, evitano l'overhead per l'invocazione di funzione (ossia ripristino dello stack  e modifica dei registri). Sono usate per `small time-critical` functions.

Una funzione inline e' definita come: `static inline void wolf(unsigned long tail_size)`, ossia usando i modificatori `static` e `inline`.

Nota: la dichiarazione si deve trovare sempre prima dell'uso, oppure il compilatore piange.

### inline assembly
con la direttiva `asm()` e' possibile eseguire istruzioni `x86` 

### branch annotation
il compilatore e' in grado di ottimizzare le operazioni di branching con le macro `likely` e `unlikely`

Considerando il seguente codice:
```c
if (error){
...
}
```

Possiamo dire al compilatore che il branch viene eseguito raramente:
```c
if(unlikely(error)){

}
```

Oppure il contrario con `likely`. Attenzione: bisogna usarle bene queste macro altrimenti si avranno dei problemi di performance.

### nessuna protezione di memoria 
Il kernel protegge gli accessi di memoria dei processi, ma non c'e' nessuno che protegge il kernel da se stesso. Si generano quindi gli `oops`, delle violazioni di memoria che portano ad un `major kernel error`.

### floating point
Quando in spazio utente devo fare operazioni con la virgola il processore deve entrare in tale modalita, nello spazio utente questo viene fatto grazie ad una trap, ma fare cio' in kernel modo e' complicato quindi: *e' meglio non usare operazioni con la virgola in spazio kernel*!

### stack kernel
mentre lo stack utente puo' crescere dinamicamente, quello del kernel ha sempre grandezza fissa.

### sincronizzazione 
il kernel e' suscettibile alle race conditions, dovuto a:
* **scheduler preemptive**: il codice utente viene schedulato di continuo nel corso dell'esecuzione.
* **processori SMP**: eseguire contemporaneamente su due porcessori codice kernel puo' essere pericoloso.
* gli interrupt avvengono in modo asincrono e possono generare race conditions
* Il kernel e' preemptive: il codice kernel puo' essere messo in pausa per eseguire altro codice.
