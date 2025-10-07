### gcc cross-compiler
Il compilatore deve conoscere la corretta `target-platform`: ossia cpu e sistema operativo di destinazione.

> **Nota:** Se siamo su `x86` e vogliamo compilare per sistemi a `32bit` dobbiamo scaricare il compilatore per `i386-elf`.

> **Nota**: Ma perche' scriviamo il nostro primo kernel usando un'architettura a `32-bit`? Perche' `grub` capisce solo quella! (sospetto che poi il kernel possa essere per `x86` ma intanto deve essere scritto a `32-bit`).

###  boot
`GRUB` si occupera di caricare il kernel, e questo deve essere in grado di continuare l'esecuzione una volta che `grub` ha finito.

Quando `grub` finisce: non c'e' uno stack, la memoria virtuale non e' abilitata, hardware non inizializzato, ecc...

Per rendere il kernel eseguibile bisogna seguire il `Multiboot Standard` (implementato da `grub`), bisogna inserire dei `magic values` nel `multiboot header`

### `boot.s`
```asm
.section .multiboot
.align 4
.long MAGIC
.long FLAGS
.long CHECKSUM
```
Vogliamo creare la sezione `multiboot` dove si trovano Il magic number, le flag, e la checksum! Questa e' la firma che viene cercata nei primi 8KiB del file del kernel.

```asm
.section .bss
.align 16
stack_bottom:
.skip 16384 # 16 KiB
stack_top:
```
Ora vogliamo creare spazio per uno stack che si trova immediatamente dopo l'header. Secondo il `System V ABI Standard` lo stack deve essere allineato a `16-byte`.

```asm
.section .text
.global _start
.type _start, @function
_start:
```

ora definiamo il testo del programma, dove `_start` e' il punto di entrata che sara chiamato dal bootloader.
Quando siamo in `_start`:
* `32-bit protected mode`:
* Interrupt disabilitati
* paging disabilitato
* stato del processore ben deciso
* istruzioni floating point e estensioni non inizializzate.
* alcune feature di `c++` richiedono per esempio supporto al `runtime`! 

```asm
mov $stack_top, %esp
```
Cosi impostiamo il puntatore allo `stack`, che in `x86` **cresce dall'alto verso il basso**, il quale indirizzo e' fornito da `stack_top` definito in precedenza.

```asm
call kerel_main

	cli
1:  hlt
	jmp 1b
```
Dove con `cli` disabilito gli interrupt, e con `hlt` attendo l'arrivo di un interrupt, dunque il computer si blocca. Potrebbero esserci degli interrupt che non possono essere **mascherati**, dunque si ritorna ad `hlt`.

```asm
.size _start, . - _start
```
Definisco la grandezza del simbolo `_start` dove `.` e' l'indirizzo corrente.

> **Nota**: le cose da fare di importante sono dunque: definire header per il kernel, impostare lo stack, e fare la chiamata al kernel.

### `freestanding` e `hosted`
Ora passiamo al kernel vero e proprio. Notiamo pero' che non saremo in un `hosted environmento`, ossia, niente librerie `C` da poter usare a parte quelle usate dal compilatore stesso.

Scriviamo dunque un programma che usa la `VGA` text mode del BIOS per scrivere qualcosa.