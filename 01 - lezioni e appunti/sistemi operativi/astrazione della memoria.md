La necessita di astrarre la memoria nasce dal fatto che  in un computer ci sono diversi tipi di memorie: dalle piu lente, alle piu veloci. Bisogna trovare un modo per gestirle!
* di tanti TB ma lente

> astrarre la memoria : i processi pensano che la memoria sia fatta in un modo, fisicamente e' organizzata in maniera diversa. I processi hanno l'illusione di avere la memoria dedicata solo per loro.

### Modello Senza astrazione: Monoprogrammazione

tre modi per caricare il sistema operativo:
* sistema operativo caricato in **ram**
* sistema operativo caricato in **rom**
* **misto** es: MS-DOS. I driver sono in rom, il sistema operativo in ram.

Ci sono problemi dal momento in cui c'e' bisogno di condividere la memoria. Gli indirizzi assoluti possono essere interpretati in maniera errata, per esempio: eseguo una jump all'indirizzo `$0020`, ma il programma si trova a partire dall'indirizzo `$1000`.

Soluzione al problema: ogni programma ha uno spazio degli indirizzi che e' unico, memoria astratta.

### Implementazione con registri base
Questa soluzione consiste nell'equipaggiare le cpu con
* registro **base**: contiene l'indirizzo dove inizia il programma 
* registro **limite**: lunghezza del programma

All'indirizzo che il programma richiede viene sempre aggiunto un indirizzo base:
`BASE + addr`. Questo approccio implica che ogni volta io debba calcolare l'indirizzo corretto e devo controllare che questo sia contenuto tra **base e limite**, operazione fatta in automatico dal processore. Non e' conveniente questo approccio perché la somma e' un'operazione lenta.

Come gestisco il sovraccarico di memoria:
* **swapping**: sposto un processo sulla memoria non volatile. Blocca ulteriormente il computer: richiedo il disco costantemente e uso la CPU.
* **memoria virtuale**: uso il processo anche se si trova parzialmente in memoria.

### Partizionamento della memoria
Le operazioni per la gestione del sovraccarico di memoria (o comunque spostamento dei processi nella memoria), possono creare dei buchi dentro la memoria. In un programma ci sono anche aree di memoria che possono crescere dinamicamente: come soluzione posso dare al processo più memoria per tenere **Stack e Heap**. Lo Stack cresce verso l'altro mentre l'Heap verso il basso.

Se finisco la memoria disponibile (serve piu memoria gli indirizzi adiacenti sono occupati):
* swap
* trasferisco il processo in un'altra memoria
* uccido il processo

### Come gestisco la memoria libera?
Come faccio a tenere traccia delle aree libere per posizionarci dei processi?
Devo minimizzare il fenomeno della frammentazione della memoria.

> matrice sparsa: i valori non sono tutti inizializzati. 
> matrice densa: i valori sono tutti inizializzati.

Metodi principali:
* **Bitmap**: ogni bit indica se l'area di memoria e' occupata o se e' libera
* **Linked List**: uso delle liste collegate. I puntatori sono ordinati per indirizzi crescenti.

### Schemi per allocazione di memoria
Ho bisogno di usare uno tra questi algoritmi per scegliere le aree di memoria da usare per i processi che vengono dall'area di swap o che vengono creati.
* **First Fit**: Trovo il primo spazio che mi va bene.
* **Next Fit**: Trovo il secondo spazio che mi va bene. Leggermente peggiore di First Fit.
* **Best Fit**: Tende a frammentare la memoria dato che il processo si insedia nel buco di memoria piu piccolo. Questo potrebbe lasciare pochi byte liberi che statisticamente non verrano usati da nesusno.
* **Worst Fit**: Mi metto nell'area piu grande di memoria disponibile, al contrario di Worst  Fit vado a generare sempre buchi di memoria molto grandi che facilmente verranno richiesti dagli altri processi
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
Negli anni 60 si usava la suddivisione del programma in **overlay**: carico solo l'**overlay** 0 in memoria, cosi evito di caricare l'intero programma in memoria. Successivamente grazie a delle chiamate e' possibile caricare gli altri overlay: o nelle locazioni di memoria successive o al posto dell'overlay 0. Dall'overlay si passa alla Memoria Virtuale, implementata grazie alla **paginazione**.

Ogni pagina e' un'insieme di indirizzi contigui che vengono mappati sulla memoria fisica.

Alcune delle pagine assegnate vengono mappate dalla **MMU**: es pagina 8 corrisponde alla **frame 0** della **MMU**.

Un processo ha informazioni riguardo alla **tabella delle pagine**. Tiene lui in memoria le pagine di cui dispone. Esiste una cache fisica sul processore che memorizza le mappature usate.

* **MMU**: ogni volta che viene chiesto un indirizzo virtuale viene, tradotto in indirizzo fisico. Prevede una cache che permette di risparmiare i tempi di accesso alla memoria fisica.
* **Tabelle delle pagine**: suddivisione dello spazio degli indirizzi, vengono mappate a indirizzi fisici. Ogni processo ha la sua.

Se un processo prova a fare un'accesso ad una pagina che non e' stata mappata viene generato un **page fault**. Generalmente una pagina può essere marcata come presente in memoria grazie da un bit.

L'indirizzo che il programma fornisce, i primi bit piu significativi rappresentano la pagina a cui voglio accedere. Posso avere dei bit che indicano lo stato della pagina (es: 0 non usata, 1 usata).

Ipotizzando di avere una macchina a 16-bit, i primi 4 potrebbero essere usati come indice nella tabella delle pagine. Gli altri 12 sono l'**offset della pagina**, rimangono invariati e vengono usati dall'hardware insieme ai 4 bit in ouput dalla MMU.

In una macchina a 64-bit, 48 sono usati per gli indirizzi (248TB di spazio). Gli altri sono usati per altre informazioni, tra questi abbiamo i bit **M** e **R**:
* M (Modificato): si attiva quando scrivo, mi permette di evitare la scrittura sul disco se non ho modificato dati. (diminuisce l'usura degli **SSD**)
* R (Riferimento): si attiva quando faccio un'accesso

Il **PTBR** (Page Table Base Register) e' un registro di proprietà del processo che indica dove si trova la tabella in memoria.

### Problema della paginazione
Problemi della paginazione, **devo mappare velocemente la tabella**, dove la metto? in Ram o dentro dei registri hardware? Viene introdotta la **TLB**, dispositivo hardware che mappa indirizzi virtuali in fisici senza passare dalla tabella (che fisicamente si trova in RAM). Riduco gli accessi di memoria per la paginazione.

Il TLB si basa sul fatto che i programmi tendono ad accedere ad un piccolo numero di pagine. I record nella TLB associano ad ogni pagina virtuale la corrispettiva pagina hardware e altre informazioni come i bit M e R, e i livelli di protezione.

Quando richiedo una pagina alla TLB posso avere un TLB `miss`, ossia non posso usare la pagina nel TLB. Posso avere due tipi di `miss`:
* **soft** miss: ho la pagina in memoria ma non e' nel TLB, basta aggiornare il TLB. Quest'operazione richiede qualche nanosecondo, e accadono molto comunemente dato che le TLB hanno poche voci: raramente piu di 256.
* **hard** miss: la pagina non e' nemmeno in memoria, deve essere caricata dal disco. Richiede molti millisecondi.

Una **page table walk** e' il nome della ricerca nella gerarchia delle pagine.

Se provo ad accedere ad un indirizzo non valido ho un `segmentation fault`.

### Page table sizes
Introduco le tabelle delle pagine **gerarchiche**: il nostro indirizzo virtuale e' composto da piu pagine innestate:
* 10 bit che sono ridondanti: rappresentano un gruppo di pagine al livello piu alto
* 10 bit, rappresentano un gruppo di pagine al livello piu basso
* 12 bit, che rappresentano l'offset

Questo metodo di organizzare gli indirizzi e' più efficiente in termini di spazio.

## Algoritmi sostituzione delle pagine
Devo gestire i **page fault**, ossia quando devo sfrattare una pagina caricata in memoria. Ci sono tante pagine e devo scegliere quale mandare a fanculo.

> nota: `malloc` alloca spazio nell'heap.

### Algoritmo di sostituzione delle pagine ottimale
* scegliere la pagina con il **riferimento più distante nel futuro** da rimuovere
* **idealmente**: si rimuove la pagina che verrà usata meno volte
* ma e' impossibile prevedere queste cose

#### NRU (not recently used)
Elimino le pagine meno accedute recentemente. Usiamo i bit R (accesso) e M (modificato), con questi posso identificare 4 classi:
* 0: non referenziata, non modificata
* 1: non referenziata, modificata (interessante!)
* 2: referenziata, non modificata
* 3: referenziata, modificata

NRU rimuove **randomicamente** la classe piu bassa. Una pagina di classe 1 e' una pagina che e' stata modificata molto tempo fa, ma nessuno ha fatto l'accesso. L'algoritmo prevede che **R venga resettato periodicamente** dal Clock (questo rende possibile il caso 1, ovvero pagina non referenziata ma modificata).
#### FIFO
Simula una coda di pagine, le piu vecchie si trovano in testa. Quelle nuove si trovano in fondo. Rimuovo sempre la pagina in testa alla coda. Non e' ottimale a livello pratico perche' la pagina da eliminare potrebbe essere la piu utilizzata anche se vecchia.
#### FIFO-Second Chance
Procedo come FIFO ma faccio un controllo sul bit `R`
* Se `R=0`, la pagina e' vecchia e non usata di recente, posso rimuoverla.
* Se `R=1`, azzero il bit e **inserisco in fondo la pagina**, te do una seconda chance prima de esse inculato.
Se non trovo una pagio&*CX5mM^una, allora l'algoritmo diventa di tipo FIFO liscio.

#### Clock
Le pagine sono rappresentate con una struttura collegata a cerchio, una lancetta indica la pagina più vecchia. Come al solito controllo i bit, se `R=1` lo azzero, e poi mando avanti la lancetta alla prossima pagina da esaminare. se `R=0` mando a fanculo la pagina.

#### LRU (Least Recently Used)
Le pagine piu usate di recente, molto probabilmente verranno usate ancora in futuro.  Uso una struttura a coda dove in testa ho le pagine meno usate di recente. In pratica non e' un algoritmo efficiente, costa molte operazioni aggiornare la struttura dati.

#### NFU (Not Frequently Used)
Implementare LRU e' complicato. Si cerca di simulare un effetto simile usando altre strategie.

Ogni pagina ha un **contatore**, ad ogni clock **aggiungo al contatore il valore di** `R`. Quando devo sfrattare una pagina scelgo quella con il contatore più basso. Questo algoritmo non e' ottimale nel caso di scenari come il **boot del sistema** operativo o le compilazioni a piu passate: le pagine dei processi piu vecchi che non vengono più usati potrebbero essere erroneamente interpretate come pagine importanti.

L'efficienza dell'algoritmo in questi casi può essere migliorata grazie all'**aging**, ossia prima di aggiungere il valore di `R` al contatore, eseguo uno **shift di 1 verso sinistra.** 


### Working Set
E' l'insieme di pagine usate da un processo
