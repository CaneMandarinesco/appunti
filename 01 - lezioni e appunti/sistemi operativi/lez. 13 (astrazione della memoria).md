Bisogna gestire tipi diversi di memorie:
* di pochi MB e veloci
* di tanti TB ma lente

> astrarre la memoria : i processi pensano che la memoria sia fatta in un modo, fisicamente e' organizzata in maniera diversa. I processi hanno l'illusione di avere la memoria dedicata solo per loro.

### Modello Senza astrazione: Monoprogrammazione
* sistema operativo caricato in ram
* sistema operativo caricato in rom
* **misto** es: MS-DOS.

Ci sono problemi dal momento in cui c'e' bisogno di condividere la memoria:
* Gli indirizzi interpretati in maniera assoluta possono essere interpretati in maniera errata. (es: `jmp 28`, ma il programma si trova a partire dall'indirizzo `1000)

Soluzione al problema: ogni programma ha uno spazio degli indirizzi che e' unico, memoria astratta.

### Implementazione con registri base
* registro **base**: contiene l'indirizzo dove inizia il programma 
* registro **limite**: lunghezza del programma

All'indirizzo che il programma richiede viene sempre aggiunto un indirizzo base:
`BASE + addr`. Questo approccio implica che ogni volta io debba calcolare l'indirizzo corretto e devo controllare che questo sia contenuto tra **base e limite**.

Come gestisco il sovraccarico di memoria:
* **swapping**: sposto un processo sulla memoria non volatile. Blocca ulteriormente il computer: richiedo il disco costantemente e uso la CPU.
* **memoria virtuale**: uso il processo anche se si trova parzialmente in memoria.

### Partizionamento della memoria
Le operazioni per la gestione del sovraccarico di memoria (o comunque spostamento dei processi nella memoria), possono creare dei buchi dentro la memoria. Ci sono anche aree di memoria che possono crescere dinamicamente: come soluzione posso dare al processo piu memoria di quanta glie ne serve.

Se finisco la memoria disponibile:
* swap
* trasferisco il processo
* uccido il processo

### Come gestisco la memoria libera?
Come faccio a tenere traccia delle aree libere per posizionarci dei processi?
Devo minimizzare il fenomeno della frammentazione della memoria.

> matrice sparsa: i valori non sono tutti inizializzati. 
> matrice densa: i valori sono tutti inizializzati.

Metodi principali:
* **Bitmap**: 
* **Linked List**

### Schemi per allocazione di memoria
* **First Fit**: Trovo il primo spazio che mi va bene.
* **Next Fit**: Trovo il secondo spazio che mi va bene.
* **Best Fit**: Tende a frammentare la memoria
* **Worst Fit**: Mi metto nell'area piu grande di memoria disponibile
* **Quick Fit**: Ho linked list diversificate a seconda della memoria che richiedo. Non funziona in caso di **coealescenza**
* **Buddy Allocation**: Migliora la performance in coalescenza. La coalescenza nasce nel momento in cui ho lo schema OCCUPATO-LIBERO-OCCUPATO. Appena uno dei due occupati si libera, si fonde insieme al blocco libero. Usato in Linux! (Cap 10.4).

### Buddy Memory Allocation
La memoria inizialmente e' un unico blocco di memoria contiguo che poi viene suddiviso in potenze di due: per esempio un un blocco di 64 pagine. Se richiedo 8 blocchi, divido la memoria in potenze di due fino ad arrivare a 8.

Quando il processo da 8 termina, si riuniscono i due blocchi da 8, e ritorno a 16. Eventualmente posso tentare di tornare a 32 se l'altro blocco non e' usato.

### Frammentazione e slab allocator
> e' un porkaround

Uso un'area di memoria per tenere piccole aree di memoria richieste (es: `malloc`). E' un monnezzaro, usa sempre la tecnica `buddy mem alloc`. Quando l'oggetto viene liberato dalla memoria, viene lasciato in memoria e invalidata. Quando chiamo una `malloc` della stessa dimensione posso riutilizzarlo.

La memoria e' divisa in `slabs`, uno `slab` puo' essere:
* pieno
* parzialmente pieno
* vuoto

## Memoria Virtuale
(anni '60) Carico solo l'**overlay** in memoria: aree piccole, la grandezza e' piu piccola della memoria che il processo richiede. Dall'overlay si passa alla memoria virtuale: ogni programma ha uno spazio degli indirizzi diviso in **pagine** (di solito di grandezza fissa).

* Un processo ha l'illusione di poter accedere ad uno spazio di indirizzo di 48 bit.

Alcune delle pagine assegnate vengono mappate sulla MMU: es pagina 8 corrisponde alla pagina 0 della MMU.

Il page fault viene gestito da una `trap` nel sistema operativo.

Un processo ha informazioni riguardo alla **tabella delle pagine**. Tiene lui in memoria le pagine di cui dispone. Esiste una cache fisica sul processore che memorizza le mappature usate.