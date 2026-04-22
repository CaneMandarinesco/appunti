#### INTRODUZIONE
In questa lezione introduciamo i **modelli distribuzionali della semantica lessicale** (_Distributional Lexical Semantics_), cioè un modo per rappresentare il significato delle parole **in modo automatico a partire dai testi**.
L’idea di base è la **Distributional Hypothesis**:

> parole con significati simili tendono ad apparire in contesti simili

Quindi, invece di definire il significato di una parola in modo esplicito (come in un dizionario), lo **ricaviamo osservando con quali parole compare**.

- per capire il significato di una parola guardiamo i **contesti in cui appare**
- il contesto è dato dalle parole che stanno:
    - a sinistra (sx)
    - a destra (dx)
- su larga scala sinonimi avranno distribuzione simile
	- se due parole compaiono **negli stessi contesti**, allora probabilmente hanno **significato simile**
##### Distributional Hypothesis
Questa idea si formalizza nella:
> **Distributional Hypothesis**: parole con significati simili tendono ad apparire in contesti simili

Se nel corpus trovi spesso:
- “il cane abbaia”
- “il gatto miagola”
e le parole:
- cane, gatto  
    compaiono in contesti simili (articoli, verbi simili, struttura simile)
allora il modello capisce che:  
**cane ≈ gatto (semanticamente simili)**
- il significato diventa qualcosa di **statistico**

##### TIPI DI RELAZIONI TRA PAROLE
Nei modelli distribuzionali non tutte le relazioni tra parole sono uguali.  
Possiamo distinguere **3 tipi principali**:
###### Relazioni sintagmatiche (co-occorrenza)
- riguardano parole che **compaiono insieme nel testo**
	- relazione **in presenza**
	- dipende dalla **vicinanza e dall’ordine nella frase**
	- tipicamente catturata guardando finestre di contesto (sx e dx)
`il lupo è affamato`
- “lupo” e “affamato” sono in relazione sintagmatica
- perché **co-occorrono nella stessa frase**
###### Relazioni paradigmatiche (sostituzione)
- riguardano parole che possono essere **scambiate nello stesso contesto**
	- relazione **in assenza** (non compaiono insieme)
	- condividono lo **stesso contesto linguistico**
`il lupo è [affamato | assetato]`
- “affamato” e “assetato”:
    - non compaiono insieme
    - ma possono occupare la stessa posizione
- è ciò che ci interessa per:
    - sinonimi
    - parole semanticamente vicine
###### Relazioni topiche
-  riguardano parole legate allo **stesso argomento**
	- non serve:
	    - né co-occorrenza diretta
	    - né sostituibilità
	- basta che compaiano nello stesso **topic/documento**
- “calcio, stadio, squadra”
- cattura il **tema generale**
##### WORD SPACES (SPAZI VETTORIALI)
- rappresentare ogni parola come un **vettore numerico**  
- costruito a partire dai **contesti in cui appare**
	- parole simili → vettori simili
	- possiamo misurare la similarità con **distanze nello spazio**
###### TOPIC SPACE (document-based)
-  contesto = **intero documento**
	- se due parole compaiono negli **stessi documenti**  
	    → sono semanticamente simili (a livello di tema)
	- cattura **relazioni topiche**
	- usa tipicamente **TF-IDF**
###### CO-OCCURRENCE WORD SPACE (window-based)
contesto = **parole vicine (sx e dx)**
- guardo una finestra di lunghezza $n$
- considero le parole:
    - a sinistra
    - a destra
- costruito con **co-occorrenze**
- ogni parola → vettore di parole vicine
- cattura:
    - relazioni sintagmatiche (co-occorrenza)
    - ma anche paradigmatiche (se contesti simili)
 è il modello più importante
###### CO-OCCURRENCE SYNTAX-BASED SPACE
 contesto = **relazioni sintattiche**
- non guardo solo parole vicine
- ma **ruolo grammaticale**
- soggetto di un verbo
- oggetto di un verbo
- cattura meglio relazioni **paradigmatiche** essendo più preciso del window based

##### COSTRUZIONE DELLA MATRICE DI CO-OCCORRENZA
Per trasformare le parole in vettori, costruiamo una **matrice M**.
La matrice è fatta così:
- **righe → parole target** (le parole che voglio rappresentare)
- **colonne → parole di contesto**
- **celle → numero di co-occorrenze**
quanto spesso una parola appare vicino a un’altra
- esempio
	- parola target: **lexicon**
	- parole di contesto:
	    - verb
	    - available
	    - online
	- Allora il vettore sarà tipo:
		- `lexicon → [verb:2, available:1, online:1, ...]`
			- “lexicon” appare 2 volte vicino a “verb”
##### costruzione della matrice M
1. scegli le **parole target**
    - es: quelle più frequenti nel corpus
2. scegli le **parole di contesto**
    - es: le più frequenti
3. scorri il testo
    - per ogni parola:
        - guarda la finestra (sx/dx)
        - aggiorna i conteggi

La finestra di contesto è un parametro chiave
- window = 2 → guardo 2 parole a sx e 2 a dx
- window grande → più contesto semantico
- window piccola → più relazione sintagmatica

- Ogni parola diventa:
	- un **vettore sparso (sparse)**
	- es:
		- `[0, 0, 2, 0, 1, 0, 0, ...]`
		- tantissimi zeri
		- poche co-occorrenze reali

#### POINTWISE MUTUAL INFORMATION (PMI)
PMI misura:

> “quanto è sorprendente vedere x e y insieme rispetto a vederli separatamente”

- non ci interessa:
	- quante volte due parole compaiono insieme
- ci interessa:
	- **quanto è significativa questa co-occorrenza**
##### Formula
$$P(x)P(y)PMI(x,y) = \log \frac{P(x,y)}{P(x)P(y)}$$
- $P(x)$ → probabilità di vedere x
- $P(y)$ → probabilità di vedere y
- $P(x,y)$ → probabilità che compaiano insieme
	- parole molto frequenti → penalizzate
	- parole rare ma significative → valorizzate
##### LATENT SEMANTIC ANALYSIS (LSA)
Dopo aver costruito la matrice (anche con PMI), abbiamo ancora problemi:
- matrice **molto grande**
- matrice **sparsa**
- presenza di **rumore**
- difficile catturare relazioni profonde
soluzione: **ridurre e “ripulire” lo spazio**
LSA cerca di:
- ridurre la dimensionalità
- mantenere solo le informazioni più importanti
- far emergere **relazioni latenti (nascoste)**
Si applica una decomposizione della matrice:
$M = U S V^T$
- e poi si approssima:
- $M \approx U_k S_k V_k^T$
- manteniamo solo le **k componenti principali**

Questa rappresentazione vettoriale permette:
- Unsupervised
	- clustering di parole simili
-  Supervised
	- classificazione (es. verbi in classi)
- Generale
	- misura di similarità tra parole
