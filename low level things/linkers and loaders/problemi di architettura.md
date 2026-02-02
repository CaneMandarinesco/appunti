Linker e loader sono dipendenti dall'architettura su cui si lavora, principalmente il modo in cui avviene l'indirizzamento e il formato delle istruzioni puo' essere problematico.

> **Nota**: quando modifico l'indirizzo di un'istruzione... lo devo fare correttamente!


Ogni sistema operativo adotta una ABI (Application Binary Interface) per i programmi che vengono eseguiti sotto quel sistema operativo.

> **Nota**: `ABI` include chiamate di sistema e tecniche per invocarle e regole per indirizzi di memoria e regole per l'uso dei registri della cpu.

### Memoria
Ogni computer ha una memoria che puo' essere vista come un'array di celle, indirizzabili che partono da `0` dove ognuna di queste e' fatta di `8 bit`, dunque tipi di dati piu grandi sono implementati usando piu celle vicine.

Ci sono due modi per leggere piu' byte da interpretare come un'unico dato: 
* `big-endian`: `0x0123` diventa `0x01` e poi `0x23`
* `little-endian`: `0x0123` diventa `0x23` e poi `0x01`.

Spesso le cpu riescono ad operare in entrambi in modi.

> [!note] aligment
> Dati `multibyte` devono essere allineati ad un `boundary`: di `2byte, 4byte, ecc...`, altrimenti le istruzioni sono piu lente, o possono causare un program fault.

> **Nota**: un sistema che fisicamente ha 32 bit per l'indirizzamento, allora ha registri a 32 bit! E similmente per 64...


> [!note] istruzioni
> L'istruzioni in un'architettura ha: `opcode`, un operando che puo' essere immediato (si ricava poiche scritto in memoria subito dopo l'opcode) o in memoria, dunque deve essere prelevato!

Si parla di **direct addressing** quando l'operando e' contenuto nel'istruzione, oppure si parla di addressing **register indirect** se il valore da usare e' contenuto in un registro o aggiungendo un'offset a questo.

L'indirizzamento puo' essere `based` nel caso di quello `register indirect` se il valore immediato rappresenta un offset da applicare all'area di memoria specifica dal registro. Se i ruoli sono al contrario, ossia il registro fa da `offset`, allora si parla di **indexed addressing**,

> **Nota**: Alcune architettura usano istruzioni a lunghezza fissa, oppure variabile! Per esempio in `x86` le istruzioni possono essere lunghe fino a `14 byte`, che e' tanto.

> **Nota**: ABI definisce una sequenza di istruzione per fare una chiamata. Una `call` a livello hardware deve ricordare l'indirizzo a cui ritornare e fare il salto verso la procedura.

Le call sono implementate con lo **stack** in `x86`: *il valore a cui ritornare e' messo nello stack*. In altre architetture il valore viene messo in un registro.

> **Nota**: Con `push` inserisco l'indirizzo quando faccio la `call`, mentre quando eseguo `ret` faccio `pop` per ottenere il vecchio indirizzo.

Nel contesto di una procedura ci sono 4 categorie di variabili:
* gli argomenti **passati** dal chiamante.
* **variabili locali**: create dalla procedura e che devono essere liberate al termine
* **dati statici locali**: si trovano in una locazione fissa in memoria, l'accesso e' privato alla procedura.
* **global static data**: si trovano in una locazione fissa in memoria, usabili da tutti.


> [!note] stack frame
> Parte di stack che contiene informazioni per una singola procedura.

> **Nota**: uno dei registri viene di solito usato come stack pointer, ossia l'indirizzo base 

Assumendo che lo stack cresca dall'alto verso il basso, gli argomenti si trovano ad un offset positivo rispetto allo stack pointer, mentre le variabili locali ad un offset negativo.

Il compilatore puo' generare una tabella di puntatori che fanno riferimento a oggetti `global static` e `static ` usati da una procedura.

L'indirizzo che punta alla tabella viene messo in un registro.

**Per esempio**: con `Rf` il frame pointer e `Rt` il table pointer e `Rx` un registro temporaneo, nel contesto di una chiamata ad una procedura:
* il chiamante salva il suo table pointer nello stack frame
* chiama la procedura e carica il table pointer della procedura
* la procedura ha il table pointer caricato e conosce gli indirizzi delle sue variabili
* poi viene ripristinato il puntatore

> **Nota**: nel contesto di una chiamata, gli indirizzi per le variabili da allocare sono dinamiche e **non statiche**. Dunque per accedere alle variabili locali si applica un offset allo stack pointer.

Per chiarire meglio:
> [!note] tabella dei simboli
> E' un struttura usata **dal compilatore** e tiene traccia di: nome variabile, tipo, visibilità e offset rispetto a freme pointer.


> [!note] Record di attivazione
> **Durante la compilazione**, vengono calcolati gli offset di variabili e parametri. Dunque viene allocata abbastanza memoria nello stack per tenere in memoria tutte le procedure diocane.

### data instructions references: `x86`
`x86` ha istruzioni asimmetriche e addressing segmentado.

* `EAX, EBX, ECX, EDX, ESI, EDI` sono registri general purpose a 32-bit, e `EBP` ed `ESP` per addressing a `32-bit`.
* `CS, DS, ES, FS, GS, SS` sono general purpose a `16-bit`.
* `AX, BX, CX, DX, SI, DI, BP, SP` si riferiscono a porzioni a `16-bit` dei registri a `32-bit`.
* `AL, AH, BL, BH, CL, CH, DL, DH` sono a `8-bit`. 

> **Nota**: alcune istruzioni su vecchi processori richiedono registri specifici, ma poi questa limitazione e' stata tolta.

> **Nota**: `ESP` e' l'hardware stack pointer e contiene sempre l'indirizzo dello stack

> **Nota**: `EBP` punta sempre al frame register che punta alla base dello stack frame corrente.

`x86` **esegue in tre modalità**:
* **real mode**: imita l'`8086` a 16-bit
* `16 bit` protected mode
* `32 bit` protected mode

Le istruzioni che indirizzano puntatori a dati in memoria usano un formato comune. Questi indirizzi sono calcolati aggiungendo uno shift (???) di `1,2,4` byte specificato nell'istruzione, un registro di base e un registro di index opzionale.
L'index puo' essere shiftato di `0,1,2,3` bit per valori multi-byte.



