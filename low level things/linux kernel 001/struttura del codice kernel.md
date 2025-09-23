### `boot/boot.s`
Quando compiliamo, il punto di entrata si trova a `0x0` (puo' essere scelto dal programmatore).

Comunque, il processore comincia ad eseguire da un indirizzo specifico, dove verrà effettuato un salto verso la rom. A questo indirizzo si trova la **boot ROM**, che scansiona ogni dispositivo per cercare un settore avviabile, cosi da eseguire il **boot loader**!

> **Nota**: il `boot loader` si trova fisicamente sul nostro medium di avvio!


Il `boot loader` si occuperà dunque di prendere l'immagine del kernel e di metterla all'indirizzo base del kernel, nel nostro caso `0x0`, poi viene dato il controllo al kernel.

### `boot/head.S`
Questo e' l'inizio del codice del kernel in quanto viene fatto setup di IDT,GDT,LDT fantocce, poi si passa in protected mode e poi si entra nel main del kernel. E' fondamentale anche fare il setup del paging!

> **Nota**: Dopo di cio' vengono attivati gli interrupt!

> **Nota**: infine si inizializzano console, hard disk, file system etc...

### `kernel/system_call.s`
Questo file descrive cosa fare quando ottengo un interrupt con codice `0x80`!

### `kernel/*`
* `kerne/sys.c`: implementazione di alcune chiamate di sistema
* `kernel/asm.s`: codice per alcune eccezioni
* `kernel/traps.c`: codice per impostare il vettore delle eccezioni descritte in `asm.s`
* `kernel/panic.c;mktime.c;vsprintf.c;printk.c`: non fondamentali, ma utilissime!
* `kernel/hd.c;keyboard.s;rs_io.s`: gestiscono interrupt hardware, buffer e queuing jobs!
* `kernel/fork.c;exit.c`: **il codice di `fork` e' a detta del libro bellissimo.**
* `kernel/sched.c`: il cuore del sistema dopo che ho inizializzato il kernel.

### `mm/*`
* `mm/page.s`: gestione del page fault.
* `mm/memory.c`: codice per allocare nuova memoria tipo.

### `fs/*`
* `fs/super.c`: punto di entrata, lettura del super blocco del sistema! 
* `fs/buffer.c`: gestione dei buffer per la lettura e scrittura
* `fs/bitmap.c, fs/inode.c, fs/namei.c, fs/truncate.c`: gestione di `inode`.
* `fs/block_dev.c;file_dev.c;char_dev.c`: gestione dei vari tipi di dispositivi.
* `fs/file_table.c`: listone di file aperti.
* `fs/open.c;read_write.c`: ultimo livello di astrazione del file system, apertura di un generico file, indipendentemente dal tipo e supporto alla lettura e scrittura.
* `fs/exec.c`: **il secondo file piu bello dopo** `fork.c`

> **Nota**: alcuni header contengono una sequenza di istruzioni assembly, come `string.h`.
### cose random
Direttamente da `vsprintf.c`
```c
/* vsprintf.c -- Lars Wirzenius & Linus Torvalds. */
/*
* Wirzenius wrote this portably, Torvalds fucked it up :-)
*/
```