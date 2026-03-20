## Distributed Indexing
**master**: assegna task ad altre macchine. E' considerata **safe** ed e' un *single point of failure*.

**splits**: collezione di di docoumenti diviso in split.
**split**: sottoinsieme dei documenti, la grandezza deve essere tale da poter essere gestita da un nodo.

**parser**: lo slave costruisce un indice in locale, sul sottoinsieme di documenti affidati
1. master assegna uno split ad una macchina in idle
2. parser legge ed emette coppie $(\text{term}, \text{doc})$ in **chunk divisi per termini**.

**partizionare i termini**: partiziono i termini in chunk in modo che gli stessi termini si trovino nello stesso chunk.

**inverter**: slave che costruisce le posting lists a partire da un chunk di termini.

**replicazione**: le posting lists costruite dagli slave, devono essere replicate.

![[Pasted image 20260318121849.png]]
**document partitioned**: una macchina si occupa di collezionare termini di un sottoinsieme di documenti

**term partitioned**: una macchina si occupa di collezionare un sottoinsieme di termini.

**partition tollerance, consistency, availability**: non posso averne tutti e 3 insieme (*teorema*). Si sacrifica la consistency nell'IR.
## Dynamic Indexing
**assunzione**: i doc ID sono crescenti.
~~assunzione: la collezione e' statica~~

**eliminazione dei documenti**: uso un vettore di bit che indica i documenti eliminati.
**modifica dei documenti**: invalido e poi riassegno.

**approccio**: 
* **main index**:
* **small index**: per i documenti nuovi
* **merge**: faccio la ricerca su entrambi gli indici e faccio il merge dei risultati.
* **re-index**: periodicamente reindicizza dentro quello principale quando lo small index diventa troppo grande.

**problema**: faccio troppi accessi in memoria per reindicizzare.
**soluzione**: ogni posting list e' un file in memoria.
* **merge**: faccio l'append scrivendo nel file della posting list corrispondente.
* **problema della soluzione**: troppi file, inefficiente in un sistema operativo.

**assunzione**: lavoriamo assumendo che l'indice e' un file unico.

**obiettivo**: fondere strutture piccole, e toccare il meno possibile quelle grosse.

**schema**: sia $n=\text{grandezza indice ausiliario}$ e $T = \text{ numero di postings totale}$
* dovrei fare  $\frac{T}{n}$ merge guardando $T$ posting
* **complessita**: $\Theta(T^2 /n)$

**schema migliore: logarithmic merge**:
* uso $\log_{2}\left( \frac{T}{n} \right)$ indici chiamati $I_{0}, I_{1}, \dots$ di grandezza $2^0n, 2^1n, 2^2n$ ecc...
* $N$ elementi.
* **indice ausiliario in RAM**: $Z_{0}$ di grandezza $2^0 n$.
* **indice ausiliario in ROM** $I_{0}$: viene creato quando $|Z_{0}| > n$.
* **indice ausiliario in ROM** $I_{1}$: viene creato facendo il merge di 2 indici $Z_{0}$ con $I_{0}$
* **indice ausiliario in** ROM $I_{2}$: viene creato facendo il merge di 2 indici $Z_{1}$ con $I_{1}$

**vantaggio di log merge**: faccio molti merge ma su collezioni piccole. vado a toccare quelle grandi poche volte.

**Costo log merge**:
* **numero di merge**: faccio al piu $O\left( \log\left( \frac{T}{n} \right) \right)$ merge.
* **costo totale dei merge**: faccio al piu $O\left( T \log \left( \frac{T}{n} \right) \right)$ operazioni.

**correzione input utente**: per farlo devo sapere qual'era probabilmente la parola che voleva inserire l'utente, ossia quella piu frequente tra i documenti.

**calcolare frequenza parole**: con piu' indici e' difficile. Devo vedere quali documenti sono stati invalidati e ricalcolare la frequenza della parola. **E' un'operazione lenta.**

**soluzione**: uso solo l'indice principale per rispondere le query. in parallelo ricostruisco l'indice da 0, quando e' pronto lo sostituisco con quell principale.

# early bird su twitter
**obiettivo**: sono interessato ai tweet piu nuovi.

**multiple index segments**: segmenti piccoli che contengono fino a $2^{32}$ tweet, ogni posting e' una parola a 32 bit.

**parole a 32 bit**: perche' e' la dimensione di una word che un processore riesce a gestire con istruzioni native.

**posting**: 24 bit per il tweet id e 8 per la posizione nel tweet.

**aggiungo alla posting list**: appendo sempre alla fine
**traversal della posting list**: dalla fine, in modo da ottenere i tweet piu nuovi.

**indici read-only**: solo un indice puo' essere scritto e letto. Gli altri sono ottimizzati per essere read-only.
# index compression
**decompressione**: deve esse fast.
