### DEFINIZIONE DI IR
**Information Retrieval (IR)** racchiude il processo di trovare materiale (di solito documenti), di natura non strutturata (generalmente testo), che soddisfa un **information need** all’interno di grandi collezioni di dati, solitamente memorizzate su computer.
- NON SONO FORMULE MA SONO UNA **catena di elaborazione end-to-end** che porta a un output.
##### Assunzioni
- la **collezione di dati è statica**
- cerchiamo **documenti rilevanti** rispetto ad un certo **information need** 
#### Tipico modello di ricerca
![[Pasted image 20260306165440.png]]

- *User task*
	- È il **problema reale dell’utente nel mondo reale**.
- *Information need*
	- È il **bisogno informativo reale** dell’utente.
	- È importante per misurare l'efficacia del modello.
- *Query*
	- La **query** è ciò che l’utente scrive nel motore di ricerca. 
- *Search Engine*
	- Il motore di ricerca prende la **query** e cerca nei documenti.
	- l’**information need è nascosto** per il search engine.
- *Collection*
	- È la **collezione di documenti** usata per cercare la risposta alla query

>[!tip] **Information need vs Query**
> - **Information need**: il bisogno informativo dell’utente, cioè il tema o l’argomento su cui vuole ottenere informazioni.
> - **Query**: la formulazione esplicita che l’utente inserisce nel sistema per comunicare il proprio bisogno informativo.
### COME SI MISURA UN sistema IR
Spesso si misura **l’intera pipeline end-to-end**.
- non le varie fasi
#### 2 metriche specifiche utilizzate in modo end-to-end (di solito)
##### Precision
- La *precision* misura, tra tutti i *documenti recuperati* dal sistema, *quanti* sono effettivamente *rilevanti*.
	- misura i **falsi positivi**
Formula concettuale:
```scss
Precision = documenti rilevanti recuperati / documenti recuperati
```
##### Recall
- La *recall* misura, tra tutti i *documenti rilevanti* presenti nella collezione, quanti sono stati *recuperati dal sistema*.
	- misura i **falsi negativi**
Formula concettuale:
```scss
Recall = documenti rilevanti recuperati / documenti rilevanti totali
```

![[Pasted image 20260401115048.png|400]]
###### Domanda d’esame: differenza tra notazione sparsa e densa
Attenzione: la differenza può riferirsi a **due cose diverse**:
- **Natura intrinseca del fenomeno:** 
	- indica **come il fenomeno è realmente** (es. i dati sono naturalmente sparsi perché molti valori sono zero).
- **Modo in cui lo rappresentiamo:** 
	- indica **come scegliamo di memorizzare o modellare quei dati nel sistema** (es. rappresentazione sparsa o densa in memoria).
- *Rappresentazione sparsa*
	- si memorizzano **solo gli elementi diversi da zero**
	- si evitano i bit inutili a 0
- *Rappresentazione densa*
	- si memorizzano **tutti i valori**, anche quelli inutili (0)

Nel caso dell’IR le matrici sono **molto sparse**, perché la maggior parte dei termini **non appare nella maggior parte dei documenti**.
##### Struttura dati utilizzata: inverted index
Idea iniziale:
- per ogni **termine** memorizziamo la **lista dei documenti che lo contengono**
- i documenti sono identificati da un **docID**

Questa struttura si chiama **inverted index**.
- Un **inverted index** è una struttura dati che associa ad ogni termine la lista dei documenti in cui quel termine appare.
###### Struttura dell’inverted index
![[Pasted image 20260306175858.png|600]]
È composto da due parti principali:
- **Dictionary**  
    insieme dei termini indicizzati
- **Postings list**  
    lista dei documenti che contengono il termine
- **Posting**  
    singolo elemento della lista che indica che un termine appare in un documento.
- **Postings list**  
    lista di tutti i documenti che contengono un determinato termine.
### Implementazione
Per rappresentare questa struttura useremo:
- **linked list** chiamate **posting list**
- ogni posizione della lista è detta **posting**

- il **dizionario punta all’inizio della posting list**
- per ora **non ordiniamo le posting list**
#### Costruzione di questa struttura dati
![[Pasted image 20260306165514.png]]
La costruzione dell’indice è divisa in tre fasi principali.
###### Tokenizzazione
- La **tokenizzazione** consiste nel trasformare il testo di un documento in una sequenza di **token**, cioè unità elementari (di solito parole)
Esempio:
```
Friends, Romans, countrymen
↓
Friends Romans countrymen
```
- il risultato viene detto:
	- *token stream*
###### Normalizzazione linguistica
- Si applicano **moduli linguistici** che modificano i token.
Obiettivo:
- rendere i termini **più uniformi**
- ridurre le varianti linguistiche
Esempio:
```
signor rossi
colore rossi
```
problema: a quale posting list associare "rossi"?
- viene affrontato dalla normalizzazione linguistica
- Stemming
- Lemmatizzazione
	- Processo che fa parte della normalizzazione linguistica e porta una parola alla sua **forma base (lemma)**
Esempio:
```
studentessa → studente
correndo → correre
```
Serve per ridurre le varianti grammaticali
###### Indicizzazione
- Ultima fase:
	- i token normalizzati vengono inseriti nell’**indice inverso**
Quindi:
```
token normalizzati
↓
inverted index
↓
dictionary + postings lists
```


### COME FUNZIONA LA FASE DI INDEX
- innanzitutto serve per costruire la nostra struttura dati inverted index
1. vengono raccolti tutti i documenti e ogni testo viene tokenizzato
	- il testo viene diviso in **token**, cioè unità lessicali (tipicamente parole)
2. il testo viene *normalizzato*
3. si generano *coppie* *(termine,docID)*, 
	- senza accorparle ancora in posting list
4. le coppie vengono ordinate in base a term e docID
	- questo permette che **uno stesso termine venga associato a più documenti**, costruendo successivamente la **posting list**.
- fase di ordinamento
![[Pasted image 20260311100247.png|300]]
- fase di posting list
	- i termini uguali vengono **raggruppati**
	- si costruisce quindi la **posting list** per ogni termine
- da notare che è stata aggiunta anche la frequenza della parola
	- **document frequency (df)**  
		- numero di documenti che contengono il termine.
		- tipo se esce the 3000k volte, posso pure ignorarlo nella fase di filtering
![[Pasted image 20260311100324.png|400]]

#### STRUTTURA FINALE
- quindi alla fine avremo
	- `term → df → posting list(docID)`
![[Pasted image 20260311100541.png|500]]

### QUERY PROCESSING
- descrive il **processo di esecuzione di una query**
- grazie all’**inverted index** una query può essere interpretata come una **Boolean Query**
esempio:
``Brutus AND Caesar``
Procedura:
1. Cercare **Brutus** nel dictionary
2. Recuperare la postings list
3. Cercare **Caesar**
4. Recuperare la postings list
5. Intersecare le liste ( con MERGE)
![[Pasted image 20260311101126.png|400]]
#### ALGORITMO DI MERGE
PSEUDOCODICE MOLTO UTILE
![[Pasted image 20260311101142.png|400]]

##### Ottimizzazione delle query processing
- se ho più condizioni logiche tipo 
`(Brutus OR Caesar) AND NOT(Antony OR Cleopatra)`
- possiamo ottimizzarla?
	- si prendendo i termini in ordine crescente di frequenza
![[Pasted image 20260311103414.png|400]]

### BOOLEAN RETRIEVAL MODEL
- *definizione e idea*
	- modello di Information Retrieval basato su **query booleane**
	- i documenti vengono rappresentati come **insiemi di parole**
	- una query è una **espressione logica**
	- le query vengono risolte usando operazioni di **merge sulle postings list** dell’inverted index
	- **operatori**
	    - **AND** → intersezione delle postings list
	    - **OR** → unione delle postings list
	    - **NOT** → esclusione dei documenti che contengono un termine
	- **idea di base**
	    - si recuperano le **postings list dei termini**
	    - si applicano **operazioni logiche tra le liste**
	    - il risultato è l’insieme dei documenti che soddisfano la query
- *svantaggi* (lettura delle parole non in ordine):
	- **assenza di ranking**
		- i documenti sono solo:
		    - rilevanti
		    - non rilevanti
	- **perdita della struttura del linguaggio**
		- il modello considera solo la **presenza dei termini**
		- ignora **ordine e contesto**
		- `persona mangia gelato = gelato mangia persona`
- *utilizzo*
	- spotlight
	- email
	- library catalog
### SCRIVERE ESERCIZI!!!
[[ESERCIZI CROCS]]
#### PHRASE QUERIES
Le **phrase queries** sono query in cui si richiede che i termini compaiano **in una specifica sequenza e posizione** all’interno del documento.
#### PHRASE QUERIES con bi grammi
Un **bi-gramma (bigram)** è una **sequenza di due token consecutivi**.
- token = parola o unità lessicale ottenuta dopo la **tokenizzazione**    
- bi-gramma = **due token di seguito**

serve per gestire le **phrase queries**, cioè query in cui l’ordine delle parole è importante.
- es: `stanford university` ma non `university stanford`

##### soluzione stupida *Biword indexing*:
- salvare direttamente i **bi-grammi come termini dell’indice**
	- occuperebbe tantissimo spazio
- ad esempio per:
	- `stanford university palo alto`
	- la query diventerebbe:
		- `stanford university  AND university palo  AND palo alto`
		- ma potremmo trovare documenti che contengono queste coppie **senza avere la frase completa**.
		- rischio di falsi positivi
	- quindi:
		- la **precision** peggiora
		- soluzione poco pratica
- bi word, svantaggi(confondere la statistica)
	- aumento anche di falsi positivi
	- aumento anche di memoria
#### Soluzione con POSITIONAL INDEXES
- salvo la posizione in cui appare il termine
	- `term → docID → positions`
	- così poi posso fare `stanford AND university` con il controllo delle posizioni uno dopo l'altro
		- `pos(university) = pos(stanford) + 1`
- problema: tanta memoria dovuto alla posizione del testo ma comunque fattibile
##### Memoria occupata dal positional index
>[!tip]- prima di parlare dei numeri che riguardano il positional index vorrei aprire una parentesi su cosa viene fatto in information retrieval come stima di occupazione di memoria di un documento
> In molti esempi di Information Retrieval si assume che **un documento medio contenga circa 1000 parole**. 
> - Una parola nel testo occupa circa **6 byte**, includendo lettere, spazi e punteggiatura. 
> - Poiché **1 byte = 8 bit**, una parola occupa quindi circa **48 bit**. 
> - Se un documento contiene 1000 parole, la dimensione approssimativa del documento è **1000 × 6 byte = 6000 byte**, cioè circa **6 KB**.

- per quanto riguarda il positional index non salviamo in memoria tutto il testo parola per parola bensì salviamo per ogni termine nuovo solo il docID e la sua posizione e anche la frequenza
### posizioni nel positional index
nel positional index non salviamo il testo completo ma:
- termine
- docID
- **posizione della parola**
- utile anche per proximity queries
dato che un documento ha circa **1000 parole**, le posizioni possibili sono:
$$1 → 1000$$
per rappresentare N valori servono circa:
$$log₂(N) bit$$
quindi:
$$log₂(1000) ≈ 10$$
dato che
$$2¹⁰ = 1024$$
questo significa che **circa 10 bit sono sufficienti per rappresentare la posizione di una parola nel documento**.
#### ESEMPIO
![[Pasted image 20260311105821.png|400]]
- qui abbiamo la parola be, la sua frequenza nei documenti
- e il documento con le rispettive docID e le posizioni in cui appare
![[Pasted image 20260311110845.png|450]]
##### NUMERI SU NUMERI SPIEGAZIONE
in alcuni casi si **pre-indicizzano frasi molto comuni**, ad esempio:
`Michael Jackson`
per ridurre il costo delle query frequenti **senza usare sempre il positional index**.
##### MISURA DEI TREND
- si possono usare **probabilità condizionate**
- per misurare **quanto spesso certe parole compaiono insieme**
questo è utile per:
- scoprire **pattern frequenti**
- identificare **frasi comuni da indicizzare direttamente**.
>[!info]- probabilità
>![[Pasted image 20260311113718.png]]

