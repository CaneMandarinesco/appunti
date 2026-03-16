**Costruzione index**: ordino i termini in $O(n\log n)$
**Problema**: troppa memoria e tempo dato che i dati sono tanti.

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

## BSBI (Block sort-based indexing)
hei gemini di cosa facciamo il sort?

**obiettivo**: sorting con pochi accessi sul disco
**coppie**: $(\text{termID}, \text{docID})$ di `8-byte`

Voglio fare il sort di `100MB` di dati:
* **blocco**: `10MB` (in modo che entra in RAM)
* **per ogni blocco**: faccio il sort e lo salvo
* **merge**: *una volta che li ho sul disco mi basta fare il merge* di questi, tanto sono tutti ordinati.


**AVL**: se i blocchi sono alberi bilanciati, allora il merge di ogni AVL costa $O(\log N)$.

**merge dei blocchi**: faccio il merge dei blocchi unendo gli AVL che rappresentano ogni blocco.

**merge delle posting list**: per ogni termine in ogni blocco, il merge viene fatto concatenando le posting list.

**documenti per blocco**:
* **b1**: per ogni termine ha un range da 1-10 doc per esempio
* **b2**: per ogni termine ha un range da 11-20..
* ecc...
* **dunque**: mi basta fare la concatenzione!

## SPIMI
* **idea 1**: genera dizionari separati per blocco per ogni termine
* **idea 2**: non fare il sort. non ce n'e' bisogno
gemini aiutami te

**lettura e scrittura su disco**: in questi contesti faccio tantissime scritture e letture. Devo cercare di ridurre

**compressione**: se riesco a comprimere i termini e i postings riesco a mettere piu roba in memoria.

## distributed indexing
* **single point of failure**: non deve esserci
* gemini che vantaggi ha rispetto a singola macchina? perche' e necessario in information retrieval?

**error prone**: tendenzialmente, con 1000 server con SLA pari a $99.9$. 63 giorni l'anno ho un guasto e devo chiamare un cristiano a ripararlo.

**master machine**: decide che deve fare lo slave. assegna compiti alle macchine in idle da una pool


