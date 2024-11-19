# istruzioni
## LDR (indirizzamento immediato)
esempio:
```
LDR R0, [R1]
```

prende un indirizzo, ed ne estrae il valore 
se voglio caricare un valore come se usassi una `mov` devo usare la sintassi `=valore`, proprio perche' l'istruzione estrae il valore da un indirizzo!  

analizzando il codice disponibile sulla documentazione di arm:
```c
if ConditionPassed() then
    EncodingSpecificOperations();
    // calcola l'offset, utilizzando imm32(=valore preso per indirizzamento immediato)
    offset_addr = if add then (R[n] + imm32) else (R[n] - imm32); 
    
	// prendi l'indirizzo da un registro
    address = if index then offset_addr else R[n];

	// prendi i dati dall'indirizzo (registro o memoria)
    data = MemU[address,4];

	// se wback
    if wback then R[n] = offset_addr;

	// sto scrivendo il program counter?
    if t == 15 then
        if address<1:0> == '00' then LoadWritePC(data);  else UNPREDICTABLE;
    else if UnalignedSupport() || address<1:0> == '00' then
        R[t] = data;
    
    else // Can only apply before ARMv7
        R[t] = ROR(data, 8*UInt(address<1:0>));

```
`LDR` e' in grado di prendere un valore e di scriverlo dentro un registro (anche il `PC`, ovvero il registro `15`).  
> #chiedere perche' devo controllare i primi due bit dell'indirizzo? `address<1:0> == '00'`
## MOV 
copia il valore di `Operand2` in `Rd`. a differenza di `LDR` che prende il valore nell'indirizzo di `Operand2` (nel caso di indirizzamento immediato)

## MUL
fa una moltiplicazione a 32-bit: se una moltiplicazione va oltre i 32-bit ne considera i 32 meno significanti (**LSB**).

# osservazioni
## individuare le condizioni
durante l'analisi di un codice conviene sempre ricerca qual'ora ci sia un loop, **qual'e la variabile che controlla questo loop**. per esempio (moltiplicazione, pag. 182 simonetta):
```
.global _start
_start:
	MOV R0, #0x2
	MOV R1, #0x4
	MOV R2, #0x0
	MOV R3, #0x0
while: 
		CMP R1, R2
		BLS end
		ADDGT R3, R3, R0
		ADDGT R2, R2, #1
		BGT while
end: NOP
```

la variable che controlla il loop e' il registro `R2` dato che:
* viene comparato ad ogni ciclo con `R1`: `CMP R1, R2`
* viene incrementato per raggiungere il valore di `R1`: `ADDGT R2, R2, #1`
* subito dopo `CMP R1, R2` trovo `BLS end` che se soddisfatta la condizione (`LS` -> <=) fa uscire l'esecuzione dal loop

## conversione salti condizionali
e' possibile convertire i salti condizionali in codice senza salti.  
```asm
CMP RX, 0xYY
BYY label

// istruzioni da eseguire
...
```

nota: 
* `RX` e' un registro
* `0xYY` e' un valore esadecimale
* `BYY` e' un istruzione di branch condizionale (es. `BEQ`, `BLT`)
* `label` e' l'indirizzo a cui saltare

> si possono convertire `spostando` (opportunamente!) la condizione nelle *precondizioni* delle operazioni da eseguire!


# esempi simo
## somma di elementi in un array
andiamo a definire il nostro array, uno dei modi per farlo e' il seguente:
```asm
.data
V: .word 3,2,5,6
```

andiamo a creare una sezione, che andra' posizionata alla fine del codice: `.data`.

all'interno di questa sezione andiamo a definire una sequenza di word (parole a 32-bit) con `.word x,y,z,...,w` dove gli elementi `x,y,z,...,w` verranno immagazzinati in una porzione continua di memoria.  

per conoscere gli indirizzi di memoria di ogni elemento abbiamo bisogno di conoscere almeno l'indirizzo del primo elemento, ovvero `3`. per farlo usiamo la label `V`.  possiamo ricavare l'indirizzo degli altri elementi *sommando all'indirizzo `V` un multiplo di `4` dato che:
* gli elementi si trovano in una porzione **contigua** di memoria
* l'indirizzo di una word dista di `4` numeri rispetto alla successiva word nella memoria.  

> nota: a differenza di una word (che e' costituita da 4 byte!) ogni byte dista di `1` numero rispetto all'elemento successivo

> [!note] esempio
> se la word di valore `3` si trova in `0xC000` allora:
> - la word di valore `2` si trova in `0xC004`
> - la word di valore `5` si trova in `0xC008`
> - la word di valore `6` si trova in `0xC00C`


> nota: gli indirizzi sono assegnati in modo arbitrario dal compilatore!

ora andiamo a scrivere il codice che somma gli elementi di questo array:

```
.global _start
_start:
		LDR R0, =V		
		LDR R2, [R0]
		MOV R4, #0

		CMP R2, #0
		BLE fine
```

leggendo istruzione per istruzione:
* carico in `R0` il valore contenuto in `V`, ovvero l'indirizzo dell'elemento `3`
* carico in `R2` il valore dell'indirizzo in `R0` (ovvero `V`)
* scrivo `0` in `R4`

osservazione:
> non avrei potuto sostituire le prime due righe con l'istruzione: `LDR R2, [V]`? no, perché in `LDR` il secondo operando deve essere un registro!  

proseguendo:
* comparo `R2` con `0`
* se `R2 <= 0` allora salto alla label `fine`

questo perche' il primo elemento dell'array (come richiesto dall'esercizio) indica il numero di elementi nell'array, e questo valore non puo' essere `0`. inoltre se questo numero e' negativo non posso interpretare il dato come valido.  

fino a questo punto per riassumere:
* ho messo in `R2` il numero di elementi dell'array
* ho inizializzato `R4` (portandolo a 0) per prepararlo a contenere la somma degli elementi
* controllato se `R2` e' un valore corretto

ora andiamo a calcolare quanti `byte` devo scorrere

```asm
then:
		LSL R3, R2, #2
		MOV R1, #4
```

ricordando che fare `LSL` di un registro equivale a moltiplicarlo per una potenza di `2`:
* moltiplico il numero di elementi (`R3`) per `2^2=4`
* scrivo `4` in `R1`

perche' ho moltiplicato `R3` per `4`? come osservato prima in arm ogni byte corrisponde ad un indirizzo:
* `0xC000`: byte `n`
* `0xC001`: byte `n+1`
* ecc...

e dato abbiamo immagazzinato i nostri dati come word (composte da 4 byte), se vogliamo conoscere il numero di byte da scorrere ci basta moltiplicare il numero di elementi per `4`!