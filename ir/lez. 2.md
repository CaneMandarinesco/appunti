# indexer steps
**matrice di incidenza**: per ogni termine salvo se compare o meno in ogni documento
**vettore di incidenza**: i valori per ogni termine costituiscono un vettore
![[Pasted image 20260311234311.png]]

**sort**: faccio **sort delle righe** per termini e poi per $\text{docID}$
**merge**: faccio merge dei termini che compaiono in più documenti con la posting lists.
**puntatori**: i termini hanno puntatori alle loro posting lists.

**Strutture dati**. Devo ricordare:
* **per ogni termine** il numero di occorrenze e puntatore alla posting lists
* **per ogni termine** la posting lists che descrive i documenti in cui compare.

**Storage**: paghiamo in storage per il dizionario e per la posting list. **Cosa puo' stare in memoria e cosa deve stare sul disco?**
* **posting lists**: occupano molto spazio, sul disco.
* **dizionario**: puo' stare se entra in memoria.

**Che struttura dati uso per le postings list in memoria**?
* **fixed length array**: spreca spazio. I termini poco frequenti non usano tutto lo spazio allocato.
* **single linked list**: inserimento di termini e' facile.
* **variable length array**: evito l'overhead dei puntatori nelle linked list, ed usano uno spazio contiguo di memoria, dunque una porzione dei dati che esamino entra in cache, permettendo una lettura velocissima.
* **ibrido**: linked list di variable length array.

**Mettere le postings lists sul disco**: sono blocchi continui di dati, senza puntatori (poiche voglio minimizzare lo spazio usato su disco). Voglio minimizzare il numero di `seek` che deve fare il disco e i blocchi da leggere.
## West Law
**boolean search**: *West Law usava boolean search per esprimere le query usando espressioni booleane. Il sistema doveva ritornare tutte le sentenze che contenevano esattamente il contenuto informativo richiesto senza perdere dati.*
## processare le query
**boolean retrieval model**: dato che mi trovo in un modello booleano, voglio comporre le query usando `AND, OR` e `NOT` per fare le query.

**AND**: voglio i documenti che contengono due termini.
* **come**: faccio il merge delle posting lists.
* **costo**: $O(x+y)$, devo scorrere entrambe le posting lists.
* **ottimizzazione** a 3 operatori: *prima la lista più corta*.

![[Pasted image 20260312000053.png]]

**Per ottimizzare le query**: devo conoscere la frequenza delle 
parole per ogni documento, in modo da ottimizzare le query.

> [!error] Per Esame
> Fai gli esercizi sull'ottimizzazione delle query

Ottimizzare: $(\text{madding OR crowd}) \text{ AND } (\text{ignore OR strife}) \text{ AND } (\text{killed OR slain})$
* **stimare grandezza**: faccio la somma delle clausole.
* **processo** in ordine di grandezza stimata.

**Come processo query arbitrarie**?
* tengo in memoria uno stato
* identifico l'ordine con cui fare le operazioni
* vai!

## tokenizzazione (approfondimento)
**tokenizzazione**: prendo una sequenza di caratteri e la taglio in vari punti, ottenendo i tokens.

**token**: una sequenza di caratteri raggruppati ed e' un'unita semantica.

**type**: tutti i token con la stessa sequenza di caratteri
**term**: sinonimo di type. E' un type incluso nel dizionario del sistema IR.

**come faccio tokenizzazione**? sicuramente devo togliere **spazi** e **punteggiatura**, ma...
* come divido in token parole che contengono un'apostrofo? es: `O'Neill` ed `aren't`

## stop words (approfondimento)
**stop words**: sono quei termini che compaiono troppo spesso e che non hanno valore informativo.

**identificare le stop words**: tolgo i 10 termini piu frequenti in tutta le collezione dei documenti, da ciascun documento.

**problema**: come cerco nomi propri, canzoni, testo dove invece quelle stop words erano rilevanti se le tolgo?

## normalizzazione (approfondimento)
**token normalizzazione**: voglio canonizzare i token in modo che differenze superficiali tra 2 token producano lo stesso termine.
**cosa vuol dire canonizzare**: voglio che `anti-discriminatory` sia mappato a `antidiscriminatory`, `car` mappato a `automobile`, ecc...

**come**? creo liste di sinonimi oppure quando costruisco il dizionario faccio la normalizzazione del termine.

## stemming (approfondimento)
**stemming**: voglio tagliare gli ultimi caratteri di un termine, a cazzo di cane. 

**lemmatization**: a differenza dello stemming, taglio la radice, devo quindi analizzare per bene la parola.

**esistono algoritmi che fanno lemmatization**

## phrase queries
**Come faccio** a rispondere a query come *"stanford university"* (deve contenere stanford e *subito dopo* university)?

**Biword Index**: indicizza le coppie di parole adiacenti
* **complessita**: il numero di coppie e' più di $n$ ma non e' in $O(n^2)$
* **query lunghe**: *posso fare l'AND di piu biword*
* **and di piu biword**: ***Introduce falsi positivi** $\equiv$ **penalizzo la precision**

**phrase index**: indicizza un numero arbitrario di parole adiacenti.
* **nota**: *un phrase index di 3 termini introduce meno falsi positivi di un biword*!

**Sol. al biword index**: memorizza la **posizione** per ogni termine, in ogni documento, una posting lists delle posizioni in cui compare.

**positional index**: dunque non si usa il biword index.
* **posting**: ha la forma $\text{docID: } <\text{pos1,pos2,...}>$.
* una query richiede $\Theta(T)$ con $T = \# \text{ di termini nella collezione}$

**Problema di spazio**: memorizzare la posizione di ogni termine, per ogni documento, vuol dire praticamente copiare il dataset originale.

**Entita nominali**: conviene in fase di processazione del testo riconoscere come **biword** entita come `Michael Jackson`, tuttavia tengo in memoria anche `Michael` e `Jackson`.
* **query piu richieste**: sfrutto il fatto che certe combinazioni sono piu richieste, **per queste creo un biword index**.
* *devo trovare **tradeoff** tra spazio e tempo quando costruisco questi indici*






