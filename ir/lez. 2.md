# indexer steps
**matrice di incidenza**: per ogni termine salvo se compare o meno in ogni documento
**vettore di incidenza**: i valori per ogni termine costituiscono un vettore
![[Pasted image 20260311234311.png]]

**sort**: faccio **sort delle righe** per termini e poi per $\text{docID}$

**merge**: faccio merge dei termini che compaiono in più documenti con la posting lists.

**puntatori**: i termini hanno puntatori alle loro posting lists.

![[Pasted image 20260312000053.png]]

**Strutture dati**. Devo ricordare:
* **per ogni termine** il numero di occorrenze e puntatore alla posting lists
* **per ogni termine** la posting lists che descrive i documenti in cui compare.
## West Law
**boolean search**: *West Law usava boolean search per esprimere le query usando espressioni booleane. Il sistema doveva ritornare tutte le sentenze che contenevano esattamente il contenuto informativo richiesto senza perdere dati.*
## processare le query
**boolean retrieval model**: dato che mi trovo in un modello booleano, voglio comporre le query usando `AND, OR` e `NOT` per fare le query.

**AND**: voglio i documenti che contengono due termini.
* **come**: faccio il merge delle posting lists per fare l'intersezione dei documenti per i due termini.
* **costo**: $O(x+y)$, devo scorrere entrambe le posting lists.
* **ottimizzazione** a 3 operatori: *prima la lista più corta*.

**Per ottimizzare le query**: devo conoscere la frequenza delle 
parole per ogni documento, in modo da ottimizzare le query.

> [!error] Per Esame
> Fai gli esercizi sull'ottimizzazione delle query
## phrase queries
**Come faccio** a rispondere a query come *"stanford university"* (deve contenere stanford e *subito dopo* university)?

**Biword Index**: indicizza le coppie di parole adiacenti
* **complessita**: il numero di coppie e' più di $n$ ma non e' in $O(n^2)$
* **query lunghe**: *posso fare l'AND di piu biword*
* **and di piu biword**: ***Introduce falsi positivi** $\equiv$ **penalizzo la precision**

**Sol. al biword index**: memorizza la posizione per ogni termine, in ogni documento, una posting lists delle posizioni in cui compare.

**Problema di spazio**: memorizzare la posizione di ogni termine, per ogni documento, vuol dire praticamente copiare il dataset originale.

**Entita nominali**: conviene in fase di processazione del testo riconoscere come **biword** nomi come `Michael Jackson`, tuttavia tengo in memoria anche `Michael` e `Jackson`.
* **query piu richieste**: sfrutto il fatto che certe combinazioni sono piu richieste. 
* *devo trovare **tradeoff** tra spazio e tempo quando costruisco questi indici*






