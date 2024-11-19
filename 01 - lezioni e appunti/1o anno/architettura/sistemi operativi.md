## cos'e' un sistema operativo?
e' un programma che nasce spontaneamente per gestire le macchine. non nasce con una definizione precisa.

> e' uno strato software che avvolge l'hardware in modo da semplificarne l'utilizzo

il fondamentale concetto del sistema operativo e' l'**astrazione**: *nascondere i dettagli implementativi e le sue difficoltà all'utilizzatore*(interrupt, errori, ...)

> la **GUI** non e' parte essenziale del sistema operativo.

il sistema operativo ha due modalità: 
* **user**: per far girare i programmi in modalità utente senza dover preoccuparci della gestione delle risorse.
* **kernel mode**: se abbiamo problemi in user mode e dobbiamo chiedere delle risorse abbiamo bisogno delle chiamate di sistema che vengono gestite in questo spazio.  

## hardware
* Program Status Word: in ARM corrisponde al **CPSR**

### astrazione
* protezione
* processi
* i/o
* shell
* spazio indirizzi
* file

## shell
e' uno dei modi per interagire col sistema operativo. la shell realizza l'interfaccia utente.
la shell non fa parte del sistema operativo, come tutti i software di base (compilatori, assemblatori, gui)

### API e chiamate di sistema
non sono la stessa cosa:
* **API**: offrono servizi del kernel
* **chiamate di sistema**: lavorano nel kernel

> le API sfruttano le chiamate di sistema.  

> esempio comando `man`: posso selezionare due pagine una per il comando e una per le chiamate di sistema. ***vedi capitolo aggiuntivo***

## linguaggio C
i sistemi operativi moderni sono scritti in C, perche':
* fornisce la possibilita di utilizzare puntatori, e strutture dati potenti ma delicate.  
* si puo' gestire la memoria con `malloc` e rilasciarla con `free`

> i linguaggi di alto livello hanno un meccanismo di prevenzione di errori di memoria: garbage collector.

> in c se dopo aver allocato memoria non la libero, faccio un casino!

i sistemi real-time vengono usati nei sistemi critici come centralini per elicotteri o arei, e in questi tipi di sistemi il tempo di esecuzione e' cruciale, se un programma deve essere eseguito in $50ms$ e ne impiega $51ms$ e' un problema (il motivo per cui sugli aerei non ci devono essere interferenze telefoniche).

# processi
i processi e' l'astrazione che ci permette di seguire l'evoluzione di un programma durante la sua esecuzione.  
grazie al processo abbiamo l'illusione di far girare piu programmi su una CPU.  

> l'istruzione che una CPU esegue e' una, ed una soltanto.

come e' possibile avere piu processi insieme? in un processore **multiprogrammato** la CPU cambia contesto: cambia processo in $10ms$.  (**pseudoparallelo**, non lo percepiamo).

ogni processo ha un suo:
* program counter
* stack
* registri
* variabili

un processo ha 3 **stati**:
* esecuzione
* bloccato: se c'e' qualcos'altro da fare di piu importante
* pronto: inizializzato e può essere schedulato (**scheduler**) oppure se non deve fare niente

### segnali
come comunico coi processi?  
```man
kill - send a signal to a process
```
> e' una chiamata di sistema

i processi hanno un'interfaccia di comunicazione per i segnali che sono vari.  
esempio l'esecuzione di un istruzione illegale (errori), e' gestita con un segnale, viene stimolato il processo.  

process control block, mantiene lo stato del processo, e' gestito dal sistema operativo:
* program counter
* stack pointer
* allocazione memoria
* file aperti dal processo
* informazioni su gestione e scheduling


## modelli per la multiprogrammazione
la multiprogrammazione migliora l'uso della CPU.
se un processo usa il 20% della potenza per fare calcoli, allora con 5 processi che usano il 20% abbiamo il 100% teoricamente ma esiste un problema: I/O. 

> un processo puo' essere bloccato se aspetta l'I/O

con un'analisi statistica possiamo dire che la probabilità complementare e':
$$
1-p^n
$$
ovvero la probabilità che un programma sia bloccato

### esercizi
abbiamo 512 MB di cui:
* 128 per il sistema operativo
* altri 128 per 3 processi
se l'attesa i/o e' dell'80% allora l'utilizzo e' :
$$
1- 0.8^3 = 49 \%
$$
se aggiungiamo 512 MB miglioriamo del:
$$
79 \% = 1 - 0.8^7
$$

# esercizi simo
## pag 209
macchina da 2GB
* SO = 1GB
* I/O = 80% **= 0.8**
* processi = 256MB

devo trovare la cpu usage: $1-p^n$ -> $1-0.8^n$
ora trovo quanti processi posso eseguire:
$$
\frac{1024}{256} = 2^2
$$
quindi calcolo:
$$
1-0.8^4
$$

se aggiungo 512MB di ram allora:
$$
1-0.8^6
$$

# thread
sono dei miniprocessy.
????

## scheduling
coordina i processi: quando e come devono essere eseguiti!

tipi di scheduling:
* non preemptie: una volta mandato in esecuzione va finche' non siblocca
* preemptive

obiettivi degli algoritmi di scheduling:
* per progettare un algoritmo di scheduling ci sono obiettivi comuni e altri che dipendono dal tipo di ambiente
* in ogni caso e' importante l'equita
* forzare le politiche del sistema e' scorretto. in un centro d
* ...

tre tipi di ambienti
* batch: sistemi bancari
* interattivi: windows, linux, android
* real-time: aerei, celle lte, centralina dialisi

## algoritmi sistemi batch
* first-come, first-served (chi prima arriva meglio alloggia)
* shortest job first
* shortest remaining time next
### first come first served
il piu semplice non preemptive. il primo che arriva e' il primo a esse servito.  
la cpu e' assegnata ai processi in ordine di arrivo,

> ...


# gestione della memoria
* memoria principale: `RAM`
* concetto di gerarchia di memoria:
	* cache
	* memoria principale
	* storage
* **gestore della memoria**: programma del SO che gestisce la memoria

perche' tutti questi accorgimenti? la memoria e' una risorsa preziosa.  

### astrazione
il miglior modo e' usare la memoria **senza astrazione** (esempio senza considerare i settori), ma dipende dal caso, il metodo senza astrazione e' usato in device come frigoriferi, microonde, vecchie console, ... .  ovviamente se abbiamo più programmi non si devono sovrapporre infatti:

> un programma utente non dovrebbe poter sovrascrivere il sistema operativo, quindi bisogna gestire in qualche modo le richieste di memoria.

> rimane comunque complicato avere un'organizzazione rigida della memoria

## protezione e riposizionamento
* un processo non deve sovrascrivere i dati di un'altro (**protezione**)
* bisogna re allocare la memoria, quindi spostare i dati in modo dinamico (**riposizionamento**)

esiste quindi un'astrazione: **spazio degli indirizzi**.  

> [!note] DEF
> mutuo := immutabile, non cambia


uno spazio degli indirizzi e' uno spazio di memoria destinato ad un processo.  

...


### registro base e registro limite
* registro base: indirizzo iniziale di memoria
* registro limite: ultimo indirizzo di memoria

quindi la porzione di memoria disponibile e' statica.  


nota:
* i programmi sono caricati consecutivamente nella memoria, non ci sono spazi vuoti tra un programma e l'altro

# swapping
quando un processo e' in esecuzione viene caricato sul disco, e quindi prelevato dalla memoria, quando non deve fare niente viene tolto dalla memoria e messa nello: `swap` (vedi linux).  

### re allocazione della memoria
lo swapping crea buchi nella memoria, e' necessario che la cpu la riorganizzi.  due modi per tenere traccia della memoria:
* bitmap
* liste collegate

## bitmap
mappa di bit che designano gli indirizzi utilizzati (a `parola`/`B`/`MB`/...):
* 0 -> indirizzo non occupato
* 1 -> indirizzo occupato

> se e' troppo grande la bitmap occupa tanta memoria!

## liste collegate
una lista e' fatta di:
* P (proc id ??? #controllare)
* inizio
* fine
* puntatore alla prossima lista

### algoritmi
* first fit
* best fit
* worst fit

# memoria virtuale
si va a simulare una macchina che ha più memoria di quanta ne dispone effettivamente. questo metodo permette di caricare tutti i programmi nella memoria perche' *non e' detto che tutti i programmi necessitino di tutta la memoria contemporaneamente.*

### paging
una porzione di memoria mappata 


> [!note] attenzione
> devi sapere cosa e' un page fault


> anche la cache funziona con un modello simile

