
# micro-architettura: java???
> fornisce una visione astratta per l'isa

e' costituito da circuiti, realizzati usando il livello logico-digitale:
* datapath
* registri
* ecc...

> non esistono principi di progettazione di questo livello.  

## data path
il percorso dati e' la parte della CPU che contiene:
* ALU
* registri
* I/O
### registri
* CPP
* TOS
* OPC
* Holding
questa architettura contiene dei registri **specifici**, in arm invece trovavamo molti registri di uso **generale**.  

### alu
ha 6 linee di controllo che servono a codificare le operazioni booleane.  potenzialmente possiamo codificare $2^6$ operazioni (ovviamente, non tutte sono utili).  
l'alu prende i valori dal registro `H` e dal bus `B`.  

### incremento di un registro
1. metto il registro sul bus `B`
2. disabilita `A`, e **incrementa** `B`
3. non fare scorrimenti! (magari mi serve per utilizzare una parte di un registro e salvarci il valore senza usare tutto il registro!)
4. scrivi il risultato in `H`

## formato delle micro istruzioni
* addr
* jam
* alu: che operazione dell'alu seleziono?
* control: vengono abilitati in base ai valori del registro di stato!
* Mem: che funzione di memoria uso ?
* B: quale registro scrivo


### mic-1
* **memoria di controllo**: memorizza le microistruzioni
* **decoder**: decodifica l'istruzione in base al opcode
> si trova in tutte le cpu!!!

## ijvm
### stack
> last-in-first-out

due istruzioni:
* push: inserisco, scrivo
* pop: estraggo, leggo

differenza tra stack e array:
> l'array e' un'area di memoria ben definita, **statica**, e posso accederci dove voglio

> lo stack e' un area di memoria **dinamica**! **cresce** verso l'alto o verso il basso inserendo i dati. lo posso usare per salvare dei salti ricorsivi.

> [!note] VETTORE
> e' una struttura dati dinamica!  (ma qualcuno tipo il simonetta potrebbe sbagliare nell'usare questo termine ????)

### modello di memoria ijvm
...

### insieme di istruzioni ijvm
* NOP
* ...
# livello isa (quello che avemo fatto su arm)
## overview 
* dal **livello isa** non si puo' conoscere il funzionamento della **microarchitettura** (perche' i livello di astrazione lo nascondono)

> e' l'interfaccia tra compilatori e hardware quindi: il linguaggio che entrambi possono comprendere! 

due modalità operative:
* kernel
* utente

### registri
alcuni registri sono visibili, altri no.  
* specializzati: 
	* pc, sp, modalita kernel, i/o
* uso generale
* registri di stato:
	* N
	* Z
	* V
	* ...
	* A: riporto oltre terzo bit
	* P: risultato pari

## core i7 (sta sul libro, non serve, abbiamo fatto arm)
* mantiene compatibilita fino al 8086 e 8088 (anni 70)
* nasce poi IA-32 su cui si fondano: x486, celeron, xeon, ecc...
* bus a 64 bit: x86-64

### modalita operative
3 modalita operative:
* reale: come 8088, se vengone eseguite istruzioni strane si blocca
* virtuale: esegue programm in 8088 in modo protetto e controllato virtualmente dall'SO
* protetta: esegue come core i7

### registri
* `EAX`: aritmetiche
* `EBX`: puntatore
* `ECX`: contenitore nei cicli
* `EDX`: moltiplicazioni e devisioni
possono essere usati a 16 o 8 bit.

> [!note] NOTA
> problemi di nomeclatura! ognuno usa i nomi che vole!!!!

* registri per la compatibilita con 8088
* EIP: Extended Instruction Register
* EFALGS: extended qualcosa

## tipi di dato
* interi: con segno, senza segno
* reali: virgola mobile (mantissa ecc...)
* boolean: vero, falso
* non numerici: caratteri, bitmap (ASCII, UNICODE), puntatori (SP, PC,LV,CPP del mic-1)

### formati di istruzione
> [!warning] FORMATI: ES. DEDURRE DAL FORMATO DELL'ISTRUZIONE A CHE TIPO DI ISTRUZIONE POTREI RIFERIRMI
> - OPCODE + ADRESS: istruzione JUMP
> - OPCODE + ADDR1 + ADDR2 + ADDR 3: potrebbe essere un ADD, SUB, ...
> - OPCODE + ADRR1 + ADDR2: non e' una mov rispetto a quello che abbiamo studiato

### criteri di progettazione delle istruzioni
* le istruzioni brevi sono preferibili a quelle lunghe
* bisogna memorizzare il piu ampio numero di istruzioni possibili dentro la cache.












