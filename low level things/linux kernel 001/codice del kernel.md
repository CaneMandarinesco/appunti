### `boot/boot.s`
```asm
| boot.s is loaded at 0x7c00 by the bios-startup routines, and moves itself
| out of the way to address 0x90000, and jumps there.
|
| It then loads the system at 0x10000, using BIOS interrupts. Thereafter
| it disables all interrupts, moves the system down to 0x0000, changes
| to protected mode, and calls the start of system. System then must
| RE-initialize the protected mode in it's own tables, and enable
| interrupts as needed.
```

Dunque, `boot.S` e' caricato a `0x7c00`, e vogliamo spostare il programma a `0x90000` mentre caricheremo il kernel a `0x10000`.

Una volta disabilitati gli interrupt, posso caricare il kernel a `0x0`.

> **Nota**: aspetto di disabilitare gli interrupt perche' sono gestiti dal BIOS! Se caricassi il kernel a `0x0` mentre gli interrupt sono abilitati in fase di boot potrebbero succedere cose stranissime.

> **Nota**: il primo `1MB` di spazio da `0x0` non e' paginato.

Il punto di entrata in `boot.s` non fa altro che copiare il bootloader a `0x90000`, *poi viene mostrato un messaggio sullo schermo*!

 > **Nota**: l'istruzione `jmpi go,INITSEG`, l'esecuzione si sposta a `0x90000`

Poi in riga 82:
```asm
| ok, we've written the message, now
| we want to load the system (at 0x10000)

	mov	ax,#SYSSEG
	mov	es,ax		| segment of 0x010000
	call	read_it
	call	kill_motor
```
viene caricato il sistema all'indirizzo `0x010000`.

Successivamente:
* `cli`: disabilita interrupt
* muovi il kernel a 0x0