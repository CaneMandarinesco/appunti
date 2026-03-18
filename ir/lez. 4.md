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
* indice ausiliario in ROM $I_{2}$: viene creato facendo il merge di 2 indici $Z_{1}$ con $I_{1}$

**vantaggio di log merge**: faccio molti merge ma su collezioni piccole. vado a toccare quelle grandi poche volte.

**in memoria ram ho**: $Z_{0}$, il piu piccolo e si trova in RAM.
**in memoria fissa ho**: $I_{i}$

**Costo**:
* $T = \text{ numero di postings}$

...
# early bird su twitter
**funzione di recupero e ordinamento a costo 0 su hashtag**: prendo l'ultimo tweet in coda dalla posting lists. costa 0!!!!!

earlybird index organization: ...

**positional index**: 

# index compression
**decompressione**: deve esse fast.
