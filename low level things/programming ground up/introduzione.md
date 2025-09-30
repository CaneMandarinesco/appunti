### metodi di accesso alla memoria
* `immediate mode`: i dati su cui lavorare si trovano nella codifica dell'istruzione stessa.
* `register addressing mode`: i dati si trovano in un registro.
* `direct addressing mode`: le istruzioni contengono indirizzi di memoria a cui accedere
* `indexed`: le istruzioni contengono un'indirizzo di memoria e un'registro usato come `index`. Posso anche specificare un `moltiplicatore` per l'index.
* `indirect addresing`: l'istruzione contiene un registro che punta ad una locazione di memoria.
* `base pointer`: come `indirect` ma posso specificare un `offset` da aggiungere all'index.

### primo programma in assembly per linux
```asm
.section .data
.section .text
.globl _start

_start:
	movl $1, %eax # sys call number
	movl $0, %ebx $ return status
	int $0x80
```

Ora questo codice deve essere **assemblato e linkato**.
```bash
as exit.s -o exit.o
ld exit.o -o exit
```


> **Nota**: `echo $?` mostra il codice di uscita del programma appena eseguito.

> **Nota**: in `Unix`, codici di uscita differenti da 0 indicano un errore.


### sintassi del programma 
> [!note] assembler directives
> Ogni cosa che inizia con un `.` non e' propriamente un'istruzione.
> Per esempio: `.section .data` suddivide il programma in sezioni, dove `.data` contiene dati da immagazzinare per il programma.

> **Nota**: analogamente `.section .text` indica l'inizio del testo, ossia dove vivono le istruzioni.

> [!note] `.globl _start`: **simboli**
> `_start` e' un simbolo, che verra' **rimpiazzato** durante l'assemblamento o il linking. Un **simbolo** marca un'indirizzo di memoria!

> **Nota**: noi non dobbiamo occuparci troppo di ricordare in che indirizzi si trovano le istruzioni e le sezioni, lo fa l'assemblatore e il computer per noi

> [!note] `.globl`
> indica che il simbolo che lo succede non deve essere scartato dopo l'assembly, fondamentale con `_start`, che definisce il punto di entrata per il linker!

Dunque, `_start:` e' una **definizione**, mi dice che `_start` e' una label che si riferisce all'indirizzo dell'istruzione o dell'oggetto che lo succede.

> [!note] `movl $1, %eax`
> mette il numero `1` nel registro `%eax`. Dunque `movl` ha due operandi: una sorgente e una destinazione. 

Alcune istruzioni potrebbero obbligarti ad usare un registro, per esempio `idivl` vuole il dividendo in `%eax` e `%edx` deve essere 0. Queste limitazioni dipendo da cpu e oggi nel 2025 potrebbero non esserci.

due tipi di registri:
* **general-purpose**: `eax, ebx, ecx, edx, edi, esi`
* **special-purpose**: `ebp, esp, eip, eflags`

di cui `eip, eflags` possono essere usati solo tramite istruzioni speciali.

>[!note] `$`
> Vuol dire che voglio usare `immediate mode` addressing, altrimenti avrebbe usato `direct addressing`


Con `int $0x80` facciamo la chiamata di sistema, e stiamo richiedendo la numero `1`, ossia `exit`. Altre chiamate di sistema potrebbero richiedere dei parametri che sono passati con i registri.

> [!note] `ebx` come parametro per `exit`
> Nel nostro caso, `ebx` contiene il valore di ritorno per `exit`. 

> [!note] `int $0x80`
> Ossia `interrupt`, dove `0x80` indica l'interrupt da usare. Viene interrotto il normale flusso di esecuzione per dare il controllo al kernel Linux,.

### `max`
 ```asm
.section .data
data_items:
	.long 3,67,34,222,45,75,54,34,44,33,22,11,66,0
.section .text

.globl _start
_start:
	movl $0, %edi
	movl data_items(,%edi, 4), %eax
	movl $eax, $ebx

start_loop:
	cmpl $0, %eax
	je loop_exit
	incl $edi
	movl data_items(,%edi,4), %eax
	cmpl %ebx, %eax
	jle start_loop
	
	movl %eax, %ebx
	jmp start_loop
	
loop_exit:
	movl $1, %eax
	int $0x80
```

### Addressing modes: come si scrivono
In generale si indirizza la memoria con il seguente formato: `ADDRESS_OR_OFFSET(%BASE_OR_OFF,%INDEX,MULTIPLIER)` dove l'indirizzo finale si ottiene cosi: `ADDR_OR_FF + %BASE_OR_OFF + MULTIPLIER*INDEX`

* **direct addressing**: `movl ADDRESS, %eax`. ho specificato solo `ADDRESS_OR_OFF`
* **indexed addressing mode**: `movl string_start(,%ecx,1), %eax` 
* **indirect addressing mode**: `movl (%eax), %ebx`
* **base pointer addressing** mode: `movl 4(%eax), %ebx`, ossia come *indirect addressing mode* ma aggiungo un **valore costante**.
* **immediate addressing**: `movl $12, %eax`, dove con `$` indico un valore immediato, altrimenti avrebbe fatto accesso all'indirizzo di memoria `12`.
* **register addressing mode**: prelevo dal registro o metto in un registro.

> **Nota:** Il registro `%eax` si divide in `%ax`, che  a sua volta diventa `%al` e `%ah`

> **Dunque**: `%eax -> %ax -> %ah | %al`

