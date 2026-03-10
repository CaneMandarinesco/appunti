**IR**: trovare materiale di natura **non strutturata** che soddisfa un **bisogno informativo**.

**Dove si usa IR?** Web Search, ricerca file nei sistemi operativi, classificazione email, ecc...

**Assunzioni**:
* **Staticità**: la collezione non cresce, ne diminuisce nel tempo.

**Performance e Ricerca di un sistema IR**: data una query posso ritornare 10,20,100,... documenti. Ma quanti di questi sono giusti?

**Precision**: ossia la **qualita**, *frazione di documenti trovati* che rispettano la richiesta informativa. 
* **Risponde alla domanda**: *"Tra tutto quello che mi hai dato, quanto è effettivamente utile?"*
* **Massimizzare la precision**: gemini aiutami te.

**Recall**: frazione di documenti rilevanti nella collezione, che sono stati trovati.
* **Risponde alla domanda**: *"Tra tutto quello che esiste di utile nel database, quanto sei riuscito a trovarne?"*
* **Massimizzare la recall**: *vuol dire aumentare i falsi positivi*.

**Esempi**:
- **Alta Precision, Bassa Recall:** Il sistema è molto "timido". Restituisce solo 2 documenti, ma sono perfetti. Hai però saltato altri 100 documenti utili (es. un motore di ricerca legale che deve essere ultra-preciso).
- **Alta Recall, Bassa Precision:** Il sistema è "generoso". Ti restituisce 10.000 documenti; tra questi ci sono tutti quelli rilevanti, ma devi spulciare migliaia di risultati inutili (es. una ricerca esplorativa per una tesi di laurea).
![[Pasted image 20260306122808.png]]
**Esempio**: trovare documenti che contengono il termine $A$, $B$ e $\lnot C$.
* costruisco la matrice per righe i termini e per colonne i documenti
* e posso "facilmente" trovare i documenti che rispettano la richiesta.

**Domanda Esame**: differenza tra notazione sparsa e densa.
* **notazione densa**: **memorizzi ogni singolo valore** dell'insieme (vettore o matrice), inclusi gli zeri.
* **notazione sparsa**: in una notazione sparsa, memorizzi **solo i valori diversi da zero**, annotando la loro posizione (indice).

**Nota**: *non conviene usare notazione densa su un dato per sua natura sparsa*.

## Invertex Index
**Indice Inverso**: voglio vedere per ogni parola quale documento lo contiene, *ossia: per ogni termine $t$ voglio una lista di documenti che lo contiene*.
* `docID`: **numero seriale** corrispondente al documento.

**Array fisso Per Invertex Index**: *non va bene*. Alcuni termini la riempiono, altri no, enorme spreco al caso peggiore.

**Posting Lists Per Invertex Index**: e' una Linked List e va bene allo scopo. E' fatta di **posting**, **ordinata** per `docID`.

**Merge di una linked List**: avanzo su due Posting Lists in ordine, in modo da cercare i documenti che compaiono in entrambi.

![[Pasted image 20260306124936.png]]


**Costruzione di un Inverted Index**:
* **tokenizer**: per esempio rimuovo punteggiature ecc... Ottengo una **stream di token**, per separare le parole
* **linguistic modules**: faccio il lower  case per esempio, voglio **normalizzare** i token. Ma magari il cognome `Rossi` diventa `rossi`, ossia un colore! Greve! 
	* Come gestisco i token `U.S.A` e `USA`?
	* Posso **troncare la desinenza** in inglese
	* si fa **lemmatizzazione** in italiano (es: da `studentessa` a `studente`).
	* Devo capire bene il ruolo di una parola del testo e decidere come modificarla per gli step successivi.
	* **polisemia**: le parole hanno tanti significati.
* **rimozione stop words**: devo togliere `the`, `a`, `of`? Queste hanno una lista lunga lunga lunga... non e' discriminativa nella mia ricerca e dunque la tolgo. Ma la band `The Who` scomparirebbe nel database!
* **indexer**: mi crea le liste sul prodotto dei linguistic modules.


