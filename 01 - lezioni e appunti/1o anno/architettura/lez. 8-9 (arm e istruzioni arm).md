...> capitolo 3 e 4 simonetta

# contesto
ARM non produce direttamente i processori ma licenzia l'uso del suo design, e ne fornisce il supporto per sviluppare l'hardware.  

> proprio per questo alcuni processori potrebbero differire tra loro.

# architettura
abbiamo più tipi di architetture baste su arm, la piu recente che studiamo e' `ARMv7`

### instruction set

>[!note] DEF.
> e' l'insieme di istruzioni che servono a programmare il codice.  quando parliamo di instruction set ci sono procedure/termini che non consideriamo in linguaggi di alto livello.  

come sono fatte queste istruzioni? arm usa istruzioni `RISC` e sono tutte a `32-bit` perché il processore e' a `32-bit`. esistono anche altri tipi di istruzioni:
* `Thumb` a `16-bit`
* `Jazelle` a `8-bit`: per eseguire java byte code direttamente su hardware

le istruzioni assembly possono essere viste come blocchi sequenziali di bit costituiti da:
* **opcode**: codice operativo, dice al processore che istruzione sto eseguendo.
* **operandi**

esiste un registro di stato: `SPSR`, utile per le istruzioni condizionali (`if`, `else`, `loop`).

sono presenti in arm:
* istruzioni di autoincremento e autodecremento
* precondizione incorporata nell'esecuzione dell'istruzione

arm e' adatta per:
* **applicazioni**
* **real-time**: sistemi per freni, apparati di rete.
* **micro-controllori**: dove e' richiesta un'interpretazione efficiente degli interrupt. 

> le cpu arm possono funzionare sia in `little-endian` che `big-endian`.

> le istruzioni di shift e rotazione sono accessibili solo tramite le operazioni aritmetiche. + efficiente. in altri linguaggi potrebbe non essere cosi.
### eccezioni (interrupt)
possono essere di tipo:
* **hardware**: la periferica hardware non e' disponibile
* **software**: `SWI`
* **errore**: malfunzionamento durante fase di `prefetch` o durante la lettura dei dati in memoria.
* **reset**

due priorita per gli interrupt:
 * `IRQ`: bassa 
 * `FIQ`: alta

### modalità di funzionamento
* **USER**: non privilegiata, i programmi sono protetti dal sistema operativo
* `FIQ`: ricevuto un interrupt ad alta priorita'
* `IRQ`: ricevuto un interrupt a bassa priorita'
* `Supervisor`: interrupt software
* `Undef`: gestisce violazioni di memoria
* `System`: `USER` ma con privilegi.  

### priorita delle eccezioni
se arrivano due eccezioni contemporaneamente vengono messe in coda e vengono gestite in base alla priorita'.


# come se programma???
abbiamo:
* 16 registri (general porpouse): `R0` ... `R15`
	* `R0` ... `R12`
	* `SP`: indirizzo dello stack
	* `LR`: link register, contiene il ritorno dell'istruzione.
	* `PC`: program counter.
* registro di stato: `CPSR` diviso in:
	* flag: 31-24
	* stato: 23-16
	* estensioni: 15-8
	* controllo: 7-0
* 20 registri non accessibili, duplicati. sono accessibili solo con privilegi.  
	* `R8_FIQ` - `R12_FIQ`: per interrupt ad alta priorita
	* 10 registri per accedere a `SP` e `LR` a seconda dell'eccezione generata.  
	* 5 registri per salvare lo stato del `CPSR` durante un'eccezione

> nota: ogni registro puo' contenere `32-bit`.

> nota: alcuni registri general porpouse dovrebbero essere usati per operazioni specifiche ma anche sticazzi.

quando il processore esegue istruzioni aritmetiche, i bit di `CPSR` vengono aggiornati in corrispondenza dei bit corrispondenti alle seguenti **flag**, in base al risultato:
* `N`: il risultato e' negativo, corrisponde al bit `31`
* `Z`: il risultato e' zero
* `C`: il risultato ha un riporto
* `V`: overflow error
* `Q`: errore di saturazione, un operazione supera il limite di `2^32` o scende sotto `0`.

gli altri bit del `CPSR` sono:
* bit greather or equal
* tipo di endianess
* `imprecise data`: e' possibile disabilitare la cattura delle eccezioni per dati imprecisi, utile per esempio quando si sta gia gestendo un'eccezione, quindi per non sfanculare il processore si disabilita.
* `interrupt` e `fast interrupt` enable/disable
* `T=1` (se `J=0`) viene abilitata la modalita' `Thumb` e il `PC` viene salvato nei 31 bit piu significativi del `CPSR`
* `J=1` (se `T=0`) viene abilitata la modalita `Jaselle`
* `M0-M4`: modalita di funzionamento (`User`, `Sistema`, ecc...)


> le istruzioni che normalmente non scrivono su `CPSR` possono farlo con il prefisso `S`.

inoltre in `CPSR` possiamo:
* abilitare/disabilitare le eccezioni: `I`, `F`, `A`
* abilitare/disabilitare: `Thumb` e `Jaselle`

### operazioni aritmetiche
in arm le operazioni aritmetiche vengono usate come **precondizioni**, determinano quindi se un'operazione verra' eseguita o se al posto suo verra' eseguito un `NOP

> nota: `CPSR` (Current Proc Status Reg) e `SPSR` (Saved Proc Status Reg) compongono il `PSR`.

> nota: l'accesso ai bit del `PSR` e' limitato in base alla modalità, comunque ci sono i bit **basici** che possono essere usati sempre.

## barrel shifter
### logical shift left (LSL)
* shift dei dati a partire dal bit meno significativo, aggiungendo `0`. 
* equivale a moltiplicare per potenze di 2. 
* l'operatore richiede il numero di shift da effettuare.

### logical shift right (LSS)
come per left ma a partire dal bit piu' significativo. il bit che esce dallo spazio viene messo dentro `C`.

### arithmetic shift right (ASR)
si applica ad un registro, richiede il numero di volte di cui eseguire lo shift verso destra *inserendo come valore, quello del bit più significativo*

### rotate right (ROR)
il bit meno significativo viene inserito al posto di quello piu significativo: **rotazione verso destra**, in piu' `C` viene aggiornato col valore del bit meno significativo spostato.  

### rotate right con estensione (RRX)
il registro `C` viene usato come estensione del registro.  

### rotate left con estensione
non esiste ma si puo' usare `ADCS` per ottenere lo stesso effetto.

### gestione eccezioni
quando si verifica un **eccezione** il comportamento del processore cambia e vengono eseguite in generale le seguenti istruzioni: 
* **salvataggio dello stato** del processore memorizzando nel `CPSR_` abilitato per il tipo di eccezione
* **modificare** `CPSR_` in modo **congruo** all'eccezione (disabilitando per esempio gli interrupt)
* **salvare l'indirizzo di ritorno** in `LR_` (abilitato per l'eccezione)
* caricare nel `PC` l'indirizzo della tabella vettore delle eccezioni per portare il processore alla *sequenza di istruzioni che gestisce l'eccezione* (routine). 
* eseguire le istruzioni della **routine**
* **ripristinare** lo **stato** della macchina caricando `SPSR_` in `CPSR`
* ripristinare il `PC` usando il valore di `LR_`

> nota: l'eccezione di reset viene abilitata anche quando si verifica un'istruzione di **salto** all'indirizzo `0x00000000`
## tipi di dati e indirizzamento
* interi in diverse basi
* virgola mobile
* bool
* caratteri
* stringhe

non ci interessa pero' sapere il tipo di dato dato che in assembly interagiamo con i singoli bit.  

### modalità di indirizzamento
indicano come il processore deve individuare e prelevare gli operandi per l'istruzione.  

modi: (???)
1. indirizzamento nelle istruzioni di elaborazione dati
2. indirizzamento nelle load/store su word/byte senza segno
3. indirizzamento nelle load/store su byte, word o doubleword
4. indirizzamento nelle load/store multiple
5. indirizzamento nelle load/store con coinvolgimento dei coprocessori

l'indirizzamento nelle `load/store` usano tutte un offset:
* offset **immediato**: ho un registro base e vado ad un offset **dichiarato**
* offset a **registro**: l'offset e' dichiarato dentro un registro general porpouse
* offset a **registro scalato**: calcolato per traslazione o rotazione

# set di istruzioni
## elaborazione e trasferimento dati
le istruzioni di questo tipo possono essere:
* aritmetiche e logiche
* aritmetiche e logiche con saturazione
* di confronto
* SIMD (Single Instruction Multiple Data)
* selezione dei byte
* moltiplicazione
* trasferimento dati

## aritmetiche e logiche
il primo degli operandi e' sempre un **registro** e il secondo puo' essere: 
* esplicitamente dichiarato
* contenuto in un registro
* o su un registro applicando lo shift

sintassi
```arm
<MNEM>{<PreCond>}{<S>} <Rd>, <Rn>, OP2
```

| elemento    | descrizione                                                                     |
| ----------- | ------------------------------------------------------------------------------- |
| `MNEM`      | codice mnemonico                                                                |
| `<PreCond>` | **opzionale**, precondizione da verificare per eseguire l'istruzione            |
| `S`         | **opezionale**, forza la scrittura in `CPSR`                                    |
| `<Rd>`      | registro in cui **salvare** il risultato                                        |
| `<Rn>`      | registro da cui **prelevare** il primo operando                                 |
| `OP2`       | secondo operando, puo' essere prelevato, immediato o il risultato di uno shift. |
tutte queste informazioni si trovano in una `word` composta da `32-bit`:


| bit:         | `31..28`  | `27 26 25` | `24..21` | `20` | `19..16` | `15..12` | `11..0` |
| ------------ | --------- | ---------- | -------- | ---- | -------- | -------- | ------- |
| significato: | `PreCond` | ` 0  0  I` | `OpCode` | `S`  | `Rn`     | `Rd`     | `OP2`   |

i bit `27 26` specificano che l'istruzione e' di elaborazione dati.  
il bit `25` specifica che `OP2` e' a valore **immediato** (`b25=1`), altrimenti e' un valore preso usando lo **shift**

### indirizzamento immediato
`12` bit per indirizzare `2^32` locazioni di memoria non sono abbastanza (usando l'offset posso arrivare a `+4096` oppure `+-2048`).  che si sono inventati allora?

i 12 bit sono divisi in:
* 4 di **posizione**
* 8 di **valore**

ed il numero descritto viene ottenuto:

> traslando verso destra i bit in **valore**, per un numero di posizioni pari al doppio di quelle in **posizione**

>[!note] ES.
>`<1101 10000001>` corrisponde a traslare di `13*2=26` (`1101=13`) volte verso destra il valore `10000001`: ovvero `...xx10000001xxxxxx`

questo sistema ha una **limitazione**, ovvero che alcuni indirizzi non sono esprimibili con questo metodo. ma ci sono dei metodi per superare la limitazione (che vengono adottati in automatico dal compilatore)

#### es 1
```asm
MOV R0, #0xFFFFFFFF
```

durante la compilazione viene generato un errore: non si puo' esprimere `0xFFFFFFFF` in forma ruotata.  

ma posso pero usare l'istruzione `MVN` che carica il valore negato di `0` (che corrisponde proprio ad `0xFFFFFFFF`):
```arm
MVN R0, #0 ; R0 = NOT 0X00000000 = 0XFFFFFFFF
```

#### es 2
```arm
MOV R0, #55550000 ; errore di compilazione!
```

piuttosto:
```asm
MOV R2, #0X55          ; R2 = 0x00000055
ORR R2, R2, R2, LSL #8 ; R2 = 0x00005555
LSL R2, R2, #16        ; R2 = 0x55550000
```

dove alla seconda riga faccio un `OR` tra `0x00000055` e `0x00005500`, ottenendo `0x00005555`. basta eseguire uno shift di `#16` per ottenere `0x55550000`

>[!note] OSS.
> alcuni compilatori potrebbero essere in grado di risolvere autonomamente questo tipo di problemi, sostituendo il tuo codice, con codice funzionante.

### ldr e esempi di istruzioni aritmetiche
```asm
LDR R2, =0X55550000 ; R2=0X55550000
```
con `LDR` posso esprimere una espressione utilizzando il simbolo di assegnazione per trasferire un valore dentro a un registro.

altre istruzioni:
```asm
ADD R0, R1, #5     ; R0 = R1 + 5
SUB R5, R3, #11    ; R5 = R3 + 11
RSB R2, R5, 0XEE00 ; R2 = 0XEE00 - R5
```

### indirizzamento con shift immediato e shift a registro
se il bit 25 vale zero, nei bit 5 e 6 e' scritto il tipo di shift da applicare tra quelli disponibili nel barrel, mentre il bit 4 indica il tipo di indirizzamento.  

```asm
						; SHIFTING IMMEDIATO
ADD R3, R3, R3, LSL #2  ; R3 = R3 + R3 * 4
RSB R2, R2, R2, LSL #4  ; R2 = R2 * 16 - R2
AND R0, R1, R2, RRX     ; R0 = R1 AND RRX(R2)
```

```asm
ADD R0, R1, R2, LSL R3 ; RO = R1 + R2*(2^R3)
ORR R0, R0, R2, LSL R3 ; RO = R0 OR R2 *(2^R3)
AND R0, R0, R2, LSR R3 ; RO = RO AND R2 /(2^R3)
```

### indirizzamento a registro
```asm
ADD R0, R1, R2 ; R0 = R1 + R2
SUB R5, R3, R2 ; R5 = R3 - R2
RSB R5, R2, R3 ; R5 - R2 - R3 (???)
```


> [!note] OSS.
> in base al tipo di indirizzamento utilizzato, cambia il modo in cui vengono usati i bit `11..0`


## istruzioni aritmetiche con saturazione
per la rappresentazione degli interi si usa il complemento.  ma certe operazioni vanno a modificare i bit del complemento quindi si sfancula tutto!

> esempio: `0x70000000 + 0x10000000 = 0x80000000`

quindi: con alcune operazioni si potrebbe eccedere il limite massimo o minimo di rappresentazione interpretando erroneamente il numero.

in arm quando un valore supera il limite massimo o minimo, viene ricondotto a quel limite e viene impostata la flag `Q` in `CPSR`.  queste istruzioni sono utili per l'elaborazione di segnali digitali dove il cambio di segno di un segnale in ambito audio/video sfancula tutto.

* `QADD`
* `QDADD`: addizione con doppia staturazione, $sat(\text{Rm} + sat(2 \cdot Rn))$
* `QSUB`:
* `QDSUB`: sottrazione co doppia saturazione, similmente a `QDADD`

la sintassi e':
```
<MNEM>{<PreCond>} <Rd>, <Rm>, <Rn>
```

dove per quanto riguarda la codifica dell'istruzione abbiamo dei bit designati come: `SBZ` (Should Be Zero).  

## istruzioni di confronto
non hanno un registro di destinazione perche' il loro risultato si trova nel registro di stato.  
sintassi:
```
<MNEM>{<PreCond>} <Rn>, OP2
```

le istruzioni sono 4:
* `TST`: test bit (`AND`)
* `TEQ`: test equal (test bit a bit, `EXOR`, se faccio l'exor di due sequenze di bit uguali ottengo 0)
* `CMP`: comparazione (`Rn - OP2`), si comporta come `ADD`
* `CMN`: comparazione negativa (`Rn + OP2`), si comporta come `SUB`

esempi:
```arm
CMP R1, #256       ; compara R1 con 256, aggiorna flag
TST R2, #1         ; TST LSB di R2 e agg. flag (???)

TEQ R2, R3         ; R2=R3? agg. flag

CMN R3, R3, LSL #4 ; R3 + R3 * 16 e agg. flag
TEQ R0, R1, RRX    ; R0 XOR R1 AND RRX(R1) e agg. flag

TST R0, R1, LSL R3 ; RO = R1*(2^R3) e agg. flag
CMN R0, R1, LSR, R3 ; R0 + R1/(2^R3) e agg. flag
```

## istruzioni SIMD
con `armv6` posso considerare i registri come array i di 2 o 4 bit, quindi posso suddividere i bit in gruppi da 2 o 4 e eseguire operazioni tra 2 registri in questo senso.  

```asm
UADD8 R0, R2, R3  ; R0[0] = R2[0] + R3[0]
                  ; R0[1] = R2[1] + R3[1]
                  ; R0[2] = R2[2] + R3[2]
                  ; R0[3] = R2[3] + R3[3]
```

la sinstassi e' la seguente:
```
<MNEM>{<PreCond>} <Rd>, <Rn>, <Rm>
```

ogni `MNEM` puo' differire in base a:
* prefisso: `U=Unsigned`, `Q=saturazione`, `H=risultati dimezzati`, ecc...
* tipo di istruzione: `ADD8`, `SUB8`, `ADD16`, ecc...

utilizzano i bit `GE` (Greather than or Equal) in `CPSR` come flag per i byte e le halfword dei risultati.

per le `halfword`
* `GE(2 / 3)`: dipende dall'`halfword` in posizione 1 (`MSB`)
* `GE(0 / 1)`: dipende dall'`halfword` in posizione 0 (`LSB`)

se invece ho `4-byte`, ogni bit di `GE` corrisponde al risultato di una sequenza di byte.

`GE` viene impostato se si eccede il limite superiore/inferiore di `16/8 byte`. (dipende se uso o non uso il segno).  

il flag `GE` viene impostato se:
* viene il `byte/halfword` supera il limite di $2^8 -1$/$2^{16} - 1$. 
* sottrazioni **senza segno**, se il risultato e' $\geq 0$
* addizioni **con segno**, se il risultato e' $\geq 0$

## istruzione di selezione
sintassi:
```arm
SEL{<PreCond>} <Rd>, <Rn>, <Rm>
```
permette di scegliere quale byte copiare tra 2 operandi, nel registro di destinazione, nello specifico, il `byte` $B_{i}$ di $Rd$ = `byte` $B_{i}$ di:
* $Rn$ se `GE[i]=1`
* $Rm$ se `GE[i]=0

all'interno dell'istruzione troviamo dei bit `SBO` (Should Be One).

## moltiplicazione
### due operandi, risultato word
```asm
<MNEM>{<PreCond>}{<S>} <Rd>, <Rm>, <Rs>
```

| mnem     | semantica                        |
| -------- | -------------------------------- |
| `MUL`    | $Rd \leftarrow Rm \cdot Rs$      |
| `SMULxy` | $Rd \leftarrow Rm[x]\cdot Rs[y]$ |
| `SMULWx` | $Rd \leftarrow Rm \cdot Rs[y]$   |
| ...      | ...                              |
> `SMULxy` permette di selezionare tramite `x` e `y` l'halfword da usare per la moltiplicazione: e' una moltiplicazione `16x16`


### tre operandi, risultato word
```asm
<MNEM>{<PreCond>}{<S>} <Rd>, <Rm>, <Rs>, <Rn>
```

| mnem     | semantica                              |
| -------- | -------------------------------------- |
| `MLA`    | $Rd \leftarrow Rn + Rm \cdot Rs$       |
| `SMLAxy` | $Rd \leftarrow Rn + Rm[x] \cdot Rs[y]$ |
| `SMLAWy` | $Rd \leftarrow Rn+Rm \cdot Rs[y]$      |
| ...      | ...                                    |

### risultato doubleword (`64-bit`)
sintassi:
```asm
<MNEM>{<PreCond>}{<S>} <RdL>, <RdH>, <Rm>, <Rs>
```

| mnem    | semantica                              |
| ------- | -------------------------------------- |
| `UMLL`  | $[RdH \| RdL] \leftarrow Rm \cdot Rs$  |
| `UMLAL` | $Rd \leftarrow Rn + Rm[x] \cdot Rs[y]$ |
| `UMAAL` | $Rd \leftarrow Rn+Rm \cdot Rs[y]$      |
| ...     | ...                                    |

## trasferimento dati
```asm
<MNEM>{<PreCond>}{<S>} <Rd>, OP2
```

`MNEM` puo' essere:
* `MOV`: Move (carica `OP2` in `<Rd>`)
* `MVN`: Move Not (carica l'inverso di `OP2` in `<Rd>`)

`<Rd>` e' il registro di destinazione.

## accesso ai registri di stato
### MRS
> Move to Register from Status register
```asm
<MNEM>{<PreCond>} <Rd>, {CPSR|SPSR}
```

### MSR
> Move to Status register from Register
```asm
<MNEM>{<PreCond>} {CPSR|SPSR}{_<ambiti>}, <Rm>
<MNEM>{<PreCond>} {CPSR|SPSR}{_<ambiti>}, #<valore>
```
ci sono 4 bit che servono a mascherare(o selezionare) 8 byte dei 32 del `CPSR|SPSR`:
* controllo: `0-7`
* estensione: `8-15`
* stato: `16-23`
* flag: `24-31`

```arm
MSR CPSR_c, R0
```

### branch
in modo diretto e' possibile saltare in avanti o indietro di `32MB`. utilizzando i registri si arriva fino a `4GB` (massimo disponibile).  grazie all'istruzione `BL` (Branch with Link) e' possibile attivare una `subroutine` e usa il registro `R14` per tenere in memoria `LR`, da caricare alla fine della `subroutine`.  
```asm
<MNEM>{<PreCond>} <Operando>
```

> l'operando puo' essere una label

usando una label come operatore posso effettuare un salto fino a `32MB` circa di distanza dall'istruzione corrente (offset).  

* `BX` (Branch Exchange), abilita thumb
* `BXJ` (Branche Exchange Jazelle), abilita jazelle
* `BLX` (Branch Link Exchange), branch link e abilita thumb

se devo fare una chiamata oltre i `32MB` uso `LDR` per caricare `PC` con l'indirizzo che mi serve

```arm
MOV LR, PC
LDR PC, =INDIRIZZO
```

esempi di branch
```arm
B   label ; salta 
BCC label ; salta se C=0
BEQ label ; salta se Z=0
```

## accesso in memoria
### load/store su word o unsigned byte
* `LDR`: load register
* `STR`: store register

```asm
<MNEM>{<PreCond>}{B}{T} <Rd>, {indirizzamento}
```
 * `{B}` indica se opero su un byte senza segno, il resto dei bit sono azzerati se si tratta di una load
 * `{T}` l'istruzione e' post-indicizzata

esempi:
```arm
LDR R0,[R1,#2] ; IMM: R0<-mem[R1,#2] 

STR R0,[R1,R2] ; OFF. A REG: R0->mem[R1,R2]

LDR R0,[R1,R2,LSL,#3] ; OFF. A REG. SCALATO: 
                      ; RO<-memoria[R1+R2*8]

STR R0,[R1,#2]!; PRE-INDICIZZ. IMM.
               ; R0->memoria[R1+2]
               ; R1=R1+2
...
```


| nome                   | esempio            |
| ---------------------- | ------------------ |
| a registro             | `MV R0, R3`        |
| assoluto               | `LDR R0, #memoria` |
| immediato              | `MOV $R0,#15`      |
| registro indiretto     | `LDR R0,[R3]`      |
| indiretto con offsett  | `LDR R0, [R4,#4]`  |
| offset pre-incremento  | ...                |
| offset post-incremento | ...                |
| registro indicizzato   | ...                |
|                        | ...                |

esistono inoltre vari tipi di istruzioni.  

### istruzioni aritmetiche e logiche

* sottrazione e addizione invertita
* and, or, xor. 

> pag 108 esercizi