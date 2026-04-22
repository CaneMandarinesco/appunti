**Costruzione index**: ordino i termini in $O(n\log n)$
**Problema**: troppa memoria e tempo. I dati sono tanti.

**indexer**: e' la macchina o il processo che costruisce l'indice.

**RCV1**: collezione usata fine anni 90, vecchia e piccola. Collezione di notizie di reuters.

**formattazione RCV1**: titolo, data e body.

**statistiche su RCV1**:
* $N = \text{numero documenti} = 800.000$
* $L = \text{numero medio di token} = 200$
* $M = \text{termini (= tipi di parole)} = 400.000$. Numero di parole differenti tra gli $N$ documenti.

```
cat file.txt | tr " " "\n" | sort | uniq | wc
```

**problema**: devo caricare tutti i documenti in memoria per farne il `sort`

**average bytes per term**:
* **token**: sono le parole che vedo nel testo
* **term**: sono le parole nel dizionario. Queste vengono trasformate!

**non-positional postings**: **100.000.000** contro i **160.000.000** di token stimati nei testi. 
* **perche?** molte parole  sono ripetute o stopword rimosse.

**memoria**: 100.000.000 postings sono tanti. 
* **problema**: troppa memoria
* **soluzione**: li butto sul disco fisso

**block size**: ogni disco scrive a blocchi da 8 fino a 256 KB.

## BSBI (Block sort-based indexing)
hei gemini di cosa facciamo il sort?

**termID**: unique serial number, invece di utilizzare le stringhe vere e proprie.

**obiettivo**: sorting con pochi accessi sul disco
**coppie**: $(\text{termID}, \text{docID})$ di `8-byte`

Voglio fare il sort di `100MB` di dati:
* **blocco**: `10MB`, la grandezza e' scelta in modo da stare in memoria ed serre veloce nel sort.
* **per ogni blocco**: ne calcolo l'inverted index, ossia faccio sorting e creo le posting list in $O(n \log n)$ con $n=10M$
* **merge**: *una volta che li ho sul disco mi basta fare il merge* di questi, tanto sono tutti ordinati.

**AVL**: se i blocchi sono alberi bilanciati, allora il merge di blocchi costa $O(\log N)$.

**merge dei blocchi**: faccio il merge dei blocchi unendo gli AVL che rappresentano ogni blocco.

**merge delle posting list**: per ogni termine in ogni blocco, il merge viene fatto concatenando le posting list.

**documenti per blocco**:
* **b1**: per ogni termine ha un range da 1-10 doc per esempio
* **b2**: per ogni termine ha un range da 11-20..
* ecc...
* **dunque**: mi basta fare la **concatenazione**!

**Multi-way merge**: 
* **apri i file simultaneamente**: mantiene per ognuno un piccolo buffer di memoria per leggere.
* **apri il file di output**: un buffer per scrivere.
* **coda priorita**: ottieni il termID piu' basso non processato, leggi e fai il merge.

![[Pasted image 20260318113735.png]]

**Problema**: assumo che io possa mantenere il dizionario che mappa termini (**stringhe**) a termID (**numeri**).

## SPIMI
* **idea 1**: genera dizionari separati per blocco per ogni termine
* **idea 2**: non fare il sort. non ce n'e' bisogno
* **token**: coppia $(\text{term}, \text{docID})$
![[Pasted image 20260318114401.png]]
* **riga 6**: aggiungi il nuovo termine al dizionario (implementato con hashing)
* **riga 7**: dal dizionario recupera la posting list
* **riga 9**: raddoppia lo spazio allocato **se** (**riga 8**) non basta.

**piu veloce di BSBI**:
* costruisco la posting list senza dover collezionare prima i token per poi farne il sort.
* ogni chiamata `SPIMI-Invert` scrive un blocco.
* L'algoritmo `SPIM` fa il merge del prodotto delle chiamate a `SPIMI-Invert`
* **compressione**: se so comprimere i dati, posso leggere e scrivere velocmenete.
* **complessita temporale**: $O(T)$

**lettura e scrittura su disco**: in questi contesti faccio tantissime scritture e letture. Devo cercare di ridurre

**compressione**: se riesco a comprimere i termini e i postings riesco a mettere piu roba in memoria.

## distributed indexing
* **single point of failure**: non deve esserci
* gemini che vantaggi ha rispetto a singola macchina? perche' e necessario in information retrieval?

**error prone**: tendenzialmente, con 1000 server con SLA pari a $99.9$. 63 giorni l'anno ho un guasto e devo chiamare un cristiano a ripararlo.

**master machine**: decide che deve fare lo slave. assegna indexing job alle macchine in idle da una pool


