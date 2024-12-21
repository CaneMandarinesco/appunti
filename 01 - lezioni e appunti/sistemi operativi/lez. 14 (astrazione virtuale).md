* **MMU**: ogni volta che viene chiesto un indirizzo virtuale viene, tradotto in indirizzo fisico. Prevede una cache che permette di risparmiare i tempi di accesso alla memoria fisica.
* **Tabelle delle pagine**: suddivisione dello spazio degli indirizzi, vengono mappate a indirizzi fisici.

L'indirizzo che il programma fornisce, i primi bit piu significativi rappresentano la pagina a cui voglio accedere. Posso avere dei bit che indicano lo stato della pagina (es: 0 non usata, 1 usata).

In una macchina a 64-bit, 48 sono usati per gli indirizzi (248TB di spazio). Gli altri sono usati per altre informazioni, tra questi abbiamo i bit **M** e **R**:
* M (Modificato): si attiva quando scrivo, mi permette di evitare la scrittura sul disco se non ho modificato dati. (diminuisce l'usura degli **SSD**)
* R (Riferimento): si attiva quando accesso

Il **PTBR** (Page Table Base Register) indica la pagina del processo.

### Problema della paginazione
Problemi della paginazione, devo mappare velocemente la tabella, dove la metto? in Ram o dentro dei registri hardware? Viene introdotta la **TLB**, dispositivo hardware che mappa indirizzi virtuali in fisici senza passare dalla tabella. Riduce gli accessi alla memoria durante la paginazione.

Il TLB si basa sul fatto che i programmi tendono ad accedere ad un piccolo numero di pagine. I record nella TLB associano ad ogni pagina virtuale la corrispettiva pagina hardware e altre informazioni come i bit M e R, e i livelli di protezione.

La MMU si interfaccia con il TLB:
* TLB `miss`: la MMU non trova l'indirizzo e devo quindi accedere alla tabella delle pagine. Ma potrei anche non avere la pagina in memoria: **page fault**. Questo mi crea molti rallentamenti, anche se velocizzo l'accesso in caso di `hit`.

### Page table sizes
Introduco le tabelle delle pagine **gerarchiche**: il nostro indirizzo virtuale e' composto da piu pagine innestate:
* 10 bit che sono ridondanti: rappresentano un gruppo di pagine
* 10 bit, che rappresentano altre pagine puntate dai bit sopra

Nelle macchine a 64 bit ho **4 pagine concatenate**!

Posso ora parlare dei **TLB miss**:
* **soft** miss: ho la pagina in memoria ma non e' nel TLB
* **hard** miss: la pagina non e' nemmeno in memoria

## Algoritmi sostituzione delle pagine
Devo gestire i **page fault**: devo sfrattare qualche pagina. Ce ne so tanti.

> nota: `malloc` alloca spazio nell'heap.

### Algoritmo di sostituzione delle pagine ottimale
* scegliere la pagina con il riferimento più distante nel futuro da rimuovere
* idealmente: si rimuove la pagina che verrà usata meno volte
* impossibile prevedere queste cose

#### NRU (not recently used)
Elimino le pagine meno accedute recentemente. Usiamo i bit R (accesso) e M (modificato), con questi posso identificare 4 classi:
* 0: non referenziata, non modificata
* 1: non referenziata, modificata (interessante lol)
* 2: referenziata, non modificata
* 3: referenziata, modificata

NRU rimuove **randomicamente** la classe piu bassa. Una pagina di classe 1 e' una pagina che e' stata modificata molto tempo fa, ma nessuno ha fatto l'accesso. L'algoritmo prevede che R venga resettato periodicamente.

#### FIFO
Elimina la pagina piu vecchia (anche se potrebbe essere la piu utilizzata). Non e' la best choice.

#### FIFO-Second Chance
* Se `R=0`, la pagina e' vecchia e non usata di recente. 
* Se `R=1`, azzero il bit e inserisco in fondo la pagina, te do una seconda chance prima de esse inculato.

#### Clock
Ad ogni ciclo di clock ...

#### LRU (Least Recently Used)
Le pagine non usate di recente le buttiamo via, quelle meno usate si trovano in coda. Devo essere in grado di aggiornare periodicamente la mia struttura dati.
Uso un contatore 

#### NFU (Not Frequently Used)

Problema: ho una pagina che ho usato tantissimo, ma poi smetto di usarla. Come faccio a capire che devo eliminare la pagina in caso di page fault?


Aggiunge un meccanismo di **aging**





