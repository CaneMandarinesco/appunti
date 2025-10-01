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

E altre cose incomprensibili, scritte in assembly `386` che mi sembra abbastanza inutile da studiare in modo approfondito.

### `kernel/fork.c`
#### `verify_area`
```c
void verify_area(void * addr,int size)
{
	unsigned long start;
	
	start = (unsigned long) addr;
	size += start & 0xfff;
	start &= 0xfffff000;
	start += get_base(current->ldt[2]);
	while (size>0) {
		size -= 4096;
		write_verify(start);
		start += 4096;
	}
}
```

Vogliamo controllare se c'e' una pagina fisica corrispondente all'indirizzo virtuale nel range `[addr;addr+size]`. 
Nelle prime righe viene allineato l'indirizzo fornito e poi per ogni pagina virtuale si crea, se non esiste la corrispondente fisica con `write_verify`

#### `copy_mem`

```c
int copy_mem(int nr,struct task_struct * p)
{
	unsigned long old_data_base,new_data_base,data_limit;
	unsigned long old_code_base,new_code_base,code_limit;
	
	code_limit=get_limit(0x0f);
	data_limit=get_limit(0x17);
	old_code_base = get_base(current->ldt[1]);
	old_data_base = get_base(current->ldt[2]);
	
	if (old_data_base != old_code_base)
		panic("We don't support separate I&D");
	if (data_limit < code_limit)
		panic("Bad data_limit");
	
	new_data_base = new_code_base = nr * 0x4000000;
	set_base(p->ldt[1],new_code_base);
	set_base(p->ldt[2],new_data_base);
	
	if (copy_page_tables(old_data_base,new_data_base,data_limit)) 
	{
		free_page_tables(new_data_base,data_limit);
		return -ENOMEM;
	}
	
	return 0;

}
```

Questa funzione e' usata per copiare la memoria del processo per la fork utilizzando la tecnica `COW`.

```c
int copy_process(int nr,long ebp,long edi,long esi,long gs,long none,long ebx,long ecx,long edx, long fs,long es,long ds, long eip,long cs,long eflags,long esp,long ss)
```

Dove questa procedura e chiamata dal gestore di chiamate di sistema, ed e' per questo che abbiamo cosi tanti argomenti: ossia lo stato dello stack prima della chiamata.

Vogliamo ottenere una nuova pagina per allocare la nuova `task_struct`, che sara poi una copia di `*current` opportunamente modificata.

Con `p->tss.esp0 = PAGE_SIZE + (long) p` diciamo dove si trova il `kernel stack`, ossia subito sopra la `task_struct`. 