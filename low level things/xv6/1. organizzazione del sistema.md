> `time-share`: condividere le risorse nel tempo tra i processi.

> `isolazione`: i processi devono eseguire isolati dagli altri, senza che possano interferire tra loro accidentalmente o in modo malevole.

La CPU `RISC-V` ha 3 stati di privilegi:
* `macchina`: la CPU, all'avvio, si trova in questo stato
* `supervisor`: appena la CPU ha eseguito le prime istruzioni fondamentali, passa in questo stato.
* `utente`

In `supervisor` mode, la cpu puo' abilitare e disabilitare gli interrupt, leggere e scrivere i registri che tengono l'indirizzo di una page table.

> [!warning] esecuzione di istruzioni privilegiate in `user` mode
> Se viene eseguita un'istruzione privilegiata quando non dovrebbe, si passa in `supervisor` mode per uccidere l'applicazione!

Per passare in supervisor mode, si usa un'istruzione `RISC` che permette di eseguire le chiamate di sistema: `ecall`

> [!note] page table 
> Le page table sono implementate via hardware

Lo spazio di indirizzi di un processo e' di fatti di $2^{38}$ (`MAXVA` in `riscv.h`), gli altri sono usati dall'hardware di paging.

Nello spazio degli indirizzi troviamo in cima:
* `trampoline` page
* `trapgrame` page
usate per la transizione da e verso il kernel.

In `proc.h`, `struct proc` si occupa di mantenere lo stato di un processo, `p->pagetable` mantiene il riferimento alla pagetable.

Ogni processo ha due stack: utente e `p->kstack`, ossia kernel:
* durante l'esecuzione normale viene usato lo stack utente
* durante l'esecuzione normale `kstack` e; vuoto.
* il codice kernel esegue nello stack kernel del processo.
* lo stack kernel e' separato e protetto!

`ecall` cambia i privilegi di esecuzione hardware e imposta il program counter verso un indirizzo specificato dal kernel. 

> Dunque: E' impossibile per un programma utente cambiare privilegio con `ecall`

`sret`: cambia i privilegi di esecuzione e ritornando in user space

* `p->state` indica lo stato del processo: in esecuzione, aspetta I/O, terminazione, bloccato.
* `p->pagetable` mantiene la page table nel formato che si aspetta il registro `RISC-V`

> Un processo consiste di: un **address space** e di un **thread** per l'esecuzione parallela.

## avvio del sistema (setup della cpu e della modalità supervisor)
Il bootloader carica il codice del kernel in memoria, ed la CPU inizia ad eseguire ad `_entry` in `entry.S`: a questo punto dell'esecuzione, il paging hardware e' disabilitato.

> Il codice kernel si dovrebbe trovare a partire da `0x800000000`

Le prime istruzioni di `_entry` impostano uno stack, in modo da eseguire codice `C`. `stack0` in `start.c` e' lo spazio dichiarato per lo stack iniziale.

Il codice carica `sp` con il valore `stack0+4096`, dato che cresce dall'altro verso il basso in RISC-V.

```c
// [start.c]
__attribute__ ((aligned (16))) char stack0[4096 * NCPU];
```
* `__attribute__ ((aligned (16)))`: garantisce che lo stack sia allineato ad indirizzi multipli di 16, importante per la CPU, alcune istruzioni richiedono questo allineamento.

In `start.c`:
* si imposta `MPP` (Machine Previous Privilege) in modo che sia impostato a `supervisor`
* si imposta `mpec`, cosi che `mret` mi porti ad eseguire il main del kernel
* si imposta `medeleg` e `mideleg` per delegare tutti gli interrupt (eccezioni e interrupt) machine mode al supervisor
* si imposta `sie` per abilitare external, timer e software interrupts.

> [!note] Machine Mode vs Supervisor Mode
> Non si vede bene dal codice la differenza tra `machine mode` e `supervisor mode`, dunque:
> * in supervisor mode, non posso usare certe istruzioni che modificano registri importanti
> * in machine mode, posso accedere a tutta la memoria sul sistema
> 
> E' dunque necessario inserire un livello di protezione per il firmware del sistema: si vuole evitare che un kernel rotto, distrugga fisicamente il pc (es: cancellando il firmware).
> * in caso di errore, in `Machine Mode` e' possibile recuperare da un errore del kernel (`Supervisor Mode`) 

> [!note] Portabilita' grazie alla `Machine Mode`
> Come il `supervisor` per il codice `utente`, la `machine mode` assume il ruolo di astrarre le risorse della macchina grazie all'interfaccia `sbi`, ossia un insieme di istruzioni da implementare in `machine_mode` da chiamare come se fossero delle `syscall`.

> Perché abbiamo bisogno di un'interfaccia `sbi`? Banalmente, una La `CPU A` potrebbe avere indirizzi diversi dalla `CPU B` per alcuni registri che hanno il medesimo comportamento, `sbi` astrare queste differenze con delle procedure che sono implementate in modo differente per ogni `cpu`.


* Con `pmpaddr0` si imposta una regione di memoria protetta da certe regole
* Con `pmpcfg0` si imposta i permessi di accesso all'area `addr0`, ossia `RWXL...`, i classici read, write, execute.

In particolare, impostiamo i registri in modo che il supervisor abbia accesso a tutta la memoria.

> [!note] `mret` 
> Infine, con `mret` si va quindi nel **main del kernel**, in supervisor mode e si avvia il sistema. Modificando `MPP` e `mpec` abbiamo definito che:
> * `mret` porta la cpu in `supervisor` mode, usando `MPP`.
> * `mret` imposta il `PC` al `main`, usando l'indirizzo in `mpec`

E' stato inoltre impostato anche il registro che ricorda l'id della CPU, in modo che ogni CPU possa riconoscere se stessa con `cpuid`.


