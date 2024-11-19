> si implementa pensandolo come un problema di programmazione. ogni istruzione isa e' una funzione chiamata dal programma principale.  

troviamo delle variabili:
* `PC`
* `stato`

segue il modello di esecuzione `fetch-decode-execute`. 

#### note aggiuntive
`ijvm` sta per `Integer Java Virtual Machine` dato che implementeremo solo istruzioni per numeri interi. creare una JVM e' il modo piu efficiente per eseguire java, che sia implementata a livello software o a livello hardware (come ISA).   quindi la nostra architettura e' fatta per eseguire il `Bytecode IJVM`.  

### percorso dati

![[Pasted image 20240521105522.png]]

* bus C -> input per i registri
* bus B -> output per i registri

> gli input e output dei registri possono essere **abilitati o disabilitati**, per caricare correttamente i dati.

troviamo una alu che e' collegata nel seguente modo:
* input sinistro, legge dal registro `H` (di mantenimento, `holding`), che a sua volta puo' essere caricato dal bus `C`
* input destro, legge dal bus `B`
* output ALU: collegato allo `shifter`
* output shifter: collegato al bus `C`

> nota: ogni registro ha massimo due pin di controllo: output e input

> [!note] come carico `H`?
> carico un valore sul bus `B` nell'ALU, e imposto il datapath in modo tale che l'`ALU` e lo `shifter` non facciano niente. viene quindi propagato quindi il valore sul bus `C`, mi basta solo abilitare il registro `H` per propagare il risultato al suo interno.  

> [!question] come posso leggere e scrivere in una botta sola?
> le due operazioni vengono eseguite in momenti diversi del ciclo:
> - all'inizio l'output viene propagato in B, e l'ALU e lo shifter svolgono automaticamente le operazioni opportune.
> - alla fine del ciclo, l'output dello shifter si e' **stabilizzato** e propagato su `C` e con un'altro ciclo di clock posso scrivere il valore in un registro.

### temporizzazione
abbiamo 3 tempi in un solo ciclo:
* stabilizzazione dati di controllo
* stabilizzazione input: bus `B` e `H`
* stabilizzazione valori in output di alu e shifter
* stabilizzazione dell'output: bus  `C`

> quello piu importante e' l'ultimo, deve essere propagato correttamente!


### memoria
* `MAR`: Memory Address Register, `32-bit`
* `MDR`: Memory Data Register, `32-bit`
* `MBR`: indirizza a **byte**, letto dalla memoria durante il fetch

> se `MAR` contiene 2 allora leggero' i byte `8,9,10,11`.

> per leggere indirizzi di 4 bit in una volta mi basta non collegare le due linee meno significanti del bus indirizzi e collego i restanti

![[Pasted image 20240521110127.png]]
### microistr.
ci sono **29 segnali** per controllare il datapath e le sue porte logiche.  

esempio:
* ciclo 1: carico l'indirizzo in `MAR` (fine del ciclo)
* ciclo 2: inizio la lettura
* ciclo 3: ho i risultati

### MIC-1
![[Pasted image 20240521110745.png]]
ha una **memoria di controllo** (contiene gli indirizzi del microprogramma), pilotata da:
* `MPC` (MicroProgram Counter): indirizzo
* `MIR` (MicroInstruction Register): istruzione in corso, determina i bit di controllo.

tra i bit di `MIR` troviamo:
* i gruppi `NEXT_ADDR` e `J` per il controllo  della prossima istruzione da eseguire.  i controlli vengono fatto utilizzando anche i bit `N` e `Z` dell'alu che vengono salvati in un flip flop per la stabilizzazione

funzionamento:
1. la parola puntata da `MPC` viene messa nella **memoria di controllo** e poi in `MIR`
2. i valori in `MIR`, presi dalla **memoria di controllo**, opportunamente collegati, si propagano in tutto il **datapath**.
3. si aspetta che i valori della ALU diventino stabili
4. i risultati vengono caricati sul bus C e i flip-flop vengono caricati dall'ALU

> #chiedere`MPC` puo' essere caricato da `MBR`?

per implementare i salti condizionali vengono usati:
* `NEXT_ADDR`
* gruppo di bit `JAM`: seleziono se voglio OR o AND o tutte e due
	* `JAMZ`
	* `JAMN`
* flip-flop `Z` e `N`

```
F = (JAMZ AND Z) OR (JAMN AND N) OR ADDR[8] 
```

come faccio transitare un valore verso `Z`? 
* metto il valore in `TOS` (Top Of Stack)
* faccio l'assegnazione: `TOS=TOS` -> cosi il valore di `TOS` passa dentro l'ALU aggiornando la flag
## istruzioni ISA
### Stack
e' un area di memoria dove e' possibile accedere ai suoi valori usando `LV` e `SP`.
lo Stack viene diviso in blocchi, ognuno dedicato al metodo/oggetto che sta scrivendo.  

> [!note] oss.
> questo metodo e' ottimale per le funzioni ricorsive

per accedere ad un elemento nello stack, bisogna fornire l'offset rispetto a `LV`. 

se un metodo ne chiama un'altro questo vuol dire che lo stack sara' diviso in due blocchi: A e B, ciascuno con i suoi parametri.  

lo stack viene utilizzato per le operazioni aritmetiche:
* copio i due valori dell'operazione **in cima allo stack**
* faccio la somma, tolgo gli operandi singoli dallo stack e metto il risultato in cima
* ripristino la grandezza dello stack e sposto il risultato dentro l'**area di memoria originaria** (da LV a SP)

# IJVM
aree di memoria, `4GB` o `4x1GB`:
* porzione costante di memoria, dati costanti
* blocco delle variabili locali
* stack degli operandi
* area dei metodi: risiede il programma

`CPP`, `LV`, `SP`: puntano a parole.  

## istruzioni
* `IADD`, `ISUB`, `IAND`, `IOR`
* `GOTO`, e altri per salti condizionali
* `INVOKEVIRTUAL`, `IRETURN`. per chiamate metodi e terminare metodi.

le chiamate a metodi funzionano nel seguente modo:
* inserisco nello stack un **rif. al metodo chiamante**. (`OBJREF`: param. 0)
* inserisco poi i **parametri**

altri dati sono salvati nell' **area dei metodi**:
* numero di parametri (incluso `OBJREF`)
* **dimensione** dello stack

### `Main1`
* incrementa PC
* fetch
* assumo che il prossimo indirizzo sia gia in `MBR` e lo carico
#### `NOP`
* `goto Main1` -> incrementa solo il `PC`

#### `IADD`
* `iadd1`: `MAR = SP = SP-1`
* `iadd2`: `H=TOS`
* `iadd3`: `MDR= TOS = MDR + H`
### `INVOKEVIRTUAL`
quando chiamo un metodo con `INVOKEVIRTUAL`:
* calcolo `SP` e `LV`
* salvo i vecchi valori di `LV` e `PC` nello stack
* salvo i parametri e lascio lo spazio per le variabili del metodo

per terminare:
* devo reimpostare il `PC`, e aggiorno `LV` e calcolo `SP`

`### IRETURN`
* deallocazione dello spazio usato
* ripristina lo stack
* ripristina PC e `LV`

> [!note] OSS.
> non ci sono dispositivi I/O. non servono e questo rende la macchina sicura.

### Java e IJVM
quando faccio delle operazioni tipo somma o comprazione, i parametri e i risultati vengono inseriti nello stack e subito dopo vengono cancellati.  

### p. 274-279
sticazz.

### implementazione: MIC-1
nel microprogramma abbiamo 112 istruzioni che implementa la IJVM

>[!note]
> `CPP` -> puntatore porzione costante di memoria
> `OPC` -> registro per `appunti`

ad ogni iterazione dobbiamo assicuraci di:
* avere un'istruzione in `MBR` da caricare nella memoria del microprogramma
* `PC` aggiornato alla prossima istr.

*perche' nelle istruzioni e' contenuto un riferimento alla prossima istruzione?*

> -> non so quanti operandi ha un'istruzione se prima non la decodifico! cosi so gia dove devo saltare senza dover calcolare gli indirizzi. potrebbe anche non essere necessario alcune volte ma conviene sempre.  

> [!note]
> nella maggior parte delle microarchitetture *si preleva sempre il prossimo byte dalla memoria*, perche' o serve come **operatore** o e' un'**istruzione**

> [!note]
> il microprogramma e' fatto di micro istruzioni che eseguite compongono un'unica istruzione!
### p284 iadd isub e bo

#chiedere p283 
* `IADD`, `ISUB`, `IAND` sono simili
* `DUP`, `POP`, `SWAP`
* `ILOAD`: un byte senza segno identifica la parola nello *spazio di memoria delle variabili locali* da caricare nello stack
	* prendo `MBR` e `LV` e li sommo per ottenere lo slot nello stack in cui inserire il valore (???)
* `ISTORE` fa il contrario di `ILOAD`, toglie dallo stack per memorizzare.

#### iadd
* `MAR = S = SP-1`
* `H = TOS`
* `MDR = TOS = MDR + H`

> TOS = Top Of Stack

`ILOAD` e `ISTORE` sono limitate, possono indirizzare solo 256 parole nello stack, per risolvere questa limitazione abbiamo il `prefisso`: `WIDE` che permette di usare 16 bit al posto di 8.

> #chiedere quali sono le operazioni che si sovrappongono durante un unica istruzione?

## progettazione del livello di microarchitettura

> [!note] lunghezza del percorso
> numero di cicli di clock per eseguire un'operazione.  

per diminuire la **lunghezza del percorso** si possono usare pezzi hardware specializzati: esempio un sommatore per il PC, oppure parallelizzare il prelievo e' ancora piu efficiente.  

> il sommatore e' la parte hardware piu studiata, puo diventare molto complessa, costosa, ma efficiente

> nota ogni istruzione termina andando a `Main1`

### riduzione della lunghezza del percorso di esecuzione

> durante l'esecuzione di Main1 il mic1 ha gia' caricato `MBR`, il codice da interpretare (letto dalla memoria centrale).

possiamo fare di meglio sovrapponendo le istruzioni, per esempio pop ha una microistruzione dove deve aspettare. si puo' usare per eseguire `Main1`!

sequenza pop *originale*:
* `pop1`: `MAR = SP = SP-1`
* `pop2`: `aspetta lettura`
* `pop3`: `TOS = MDR`
* `Main1`

sequenza pop **ottimizzata**:
* `pop1`: `MAR = SP = SP-1`
* `pop2`: `Main1`
* `pop3`: `TOS=MDR`

### architettura a 3 bus
inserendo il bus `A` e' possibile sommare due registri in una botta sola, senza dover caricare un registro in `H`.  

### unita di prelievo dell'istruzione: mic-2
* creare percorsi dati aggiuntivi per copiare i dati senza impiegare la ALU a vuoto.

oppure possiamo usare una **IFU** (Instruction Fetch Unit) che incrementa il `PC` e preleva gli operandi delle istruzioni, per farlo ha un registro a parole interno che viene incrementato (`IMAR`, finché l'operando si trovi immediatamente dopo l'istruzione in esecuzione). 

> nota: l'incrementatore di `IMAR` e' a parole!

inoltre deve monitorare il bus C per vedere quando il `PC` viene aggiornato.  

la IFU quindi carica gli operandi dentro `MBR1` e `MBR2`. 

> l'implementazione delle istruzioni e' completamente diversa ora, bisogna sfruttare IFU. vedi MIC2

> #chiedere perche' IMAR e' traslato di 2?

### architettura a pipeline: mic-3
inserisco dei **latch** che spezzano la mia architettura:
* latch `A`
* latch `B`
* latch `C`

posizionati in corrispondenza di input e output di alu.  
ora facendo cosi abbiamo:
* **ridotto** il ritardo: ***clock piu elevato!!!***
* **sequenzializzato** il datapath
* possiamo utilizzare la alu in tutti i cicli! (conseguenza della sequenzializzazione)

quindi il workflow e' diviso nel seguente modo:
* metto i dati nei latch `A` e `B`
* aspetto che l'alu calcoli
* metto i risultati nel latch `C`

abbiamo triplicato la capacita di calcolo del processore!

bisogna stare attenti pero alle situazioni di **dipendenza RAW** (read after write): sto leggendo un registro che non e' stato ancora scritto. rispetto alle architetture senza pipeline questo può accadere spesso e una possibile soluzione e' ritardare l'esecuzione nella pipeline.  nonostante questo ritardo pero' il sistema e' molto efficiente.  

> il **clock** e' determinato dal tempo di propagazione sul bus!


# esercizi (cap. 4)!
## es. 4
quando viene abilitato il campo `JMPC` viene calcolato l'or logico per formare l'indirizzo della microistruzione successiva ci sono situazioni in cui ha senso che `NEXT_ADDRESS=0x1FF` e uso `JMPC`?

dal testo: 
> se `JMPC` e' impostato ad 1 allora gli 8-bit meno significativi di `NEXT_ADDRESS` sono comparati in OR con `MBR`.  

di solito se uso `JMPC` mi aspetto che gli 8 bit meno significativi di `NEXT_ADDRESS` siano `0x00`.

sempre dal testo:
> **I 9 BIT** di `NEXT_ADDRESS` vengono **inizialmente** copiati in `MPC`. 

e quindi durante la copia viene ispezionato il campo `JAM` e' il bit piu significativo viene modificato di conseguenza.  

avere `NEXT_ADDRESS=0x1FF` ha senso, perché cosi posso indirizzare l'istruzione in `0x1FF` (servono) che non e' possibile indirizzare con `MBR` (dato che e' costituito da 8 bit), tutto questo senza dovere comparare `MBR` con i bit `N` e `Z`.  quindi si posso saltare in modo incondizionato all'istruzione in `0x1FF`.  

## es. 8
si perché `L1` e `L2` distano esattamente **256 parole**.  infatti: `0x40 + 0x100 = 0x140` dove `0x100 = 256 = 2^9 = (100000000)`

## es. 9
nel microprogramma `MDR` viene copiato dentro `H`, durante `if_icmpeq3` e successivamente, dopo qualche linea, viene sottratto da `TOS` per effettuare il test di uguaglianza. non sarebbe stato meglio avere semplicemente un'unica istruzione?
```MIC-1
if_cmpeq3 Z = TOS - MDR; rd
```

perché' dovrei scrivere contemporaneamente `TOS` e `MDR` sul bus `B`. infatti nell'implementazione uno dei due valori viene caricato nel registro `H`.  


