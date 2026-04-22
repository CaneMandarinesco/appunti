#### SPELL CORRECTION
- fino ad ora: match esatto dei termini
- problema: utenti **sbagliano a scrivere / non sanno la parola precisa**
quindi introduciamo:
- wildcard queries
- spelling correction
- obiettivo:
	- recuperare documenti **anche con errori o varianti**
##### WILD CARD
Una wildcard è un simbolo (come `*`) che rappresenta una sequenza arbitraria di caratteri, permettendo di cercare termini anche senza conoscerne esattamente la forma.
- `mon*` → tutte le parole che iniziano con _mon_
	- scendo nel tree con `m → o → n`
- `*mon` → tutte le parole che finiscono con _mon_
	- salvo le parole al contrario e le cerco come prima
- `pro*cent` → parole che iniziano con _pro_ e finiscono con _cent_
	- per risolvere questa query dovrei fare `pro* AND *cent` 
	- **estremamente costoso**
###### Permuterm index
- è una *struttura dati* che permette di gestire **wildcard query generali**
- idea: trasformare una wildcard query in una **ricerca per prefisso**
per ogni termine:
- aggiungo un simbolo speciale `$` alla fine
- genero **tutte le rotazioni** della parola
- ogni rotazione punta al termine originale, così da trovarlo facilmente nella fase di query
![[Pasted image 20260325103249.png|400]]
- `$` indica la **fine della parola**
- serve per trovare parole che iniziano con una lettera e finiscono con un'altra
	- ci saranno rotazioni dove appare prima del `$` la fine della parola e dopo il `$` l'inizio 
esempio di applicazione della query vera e propria: 
![[Pasted image 20260325103830.png|500]]

- con una query tipo `X*Y*Z`
	- riscontro dei problemi
	- faccio prima  `X*Z`
	- poi filtro controllando se ognuna di quelle parole ha `Y` in mezzo
###### Bigram (k-gram) indexes
- Il k-gram index indicizza sotto-stringhe dei termini e permette di trovare candidati che condividono parti della query, ma richiede un post-filtering per eliminare i falsi positivi.
- con $k=2$ creiamo una lista del seguente tipo  
`hello → $h, he, el, ll, lo, o$`
- `$h` → parola che **inizia con h**
- `he` → contiene `"he"`
- `el` → contiene `"el"`
- `ll` → contiene `"ll"`
- `lo` → contiene `"lo"`
- `o$` → parola che **finisce con `o`**
- per cercare ad esempio `mon*`
- → `$m AND mo AND on`
	- interseco le liste dei k-gram
**FALSI POSITIVI**
- alcuni termini passano il filtro ma non matchano davvero
	- `mon*` → trova anche `moon`
*EFFETTUO POST FILTERING*
- controllo finale sulla stringa reale
#### SPELLING CORRECTION TASK
- si divide in:
	- **spelling error detection**
	    - capire se una parola è sbagliata
	- **spelling error correction**
	    - generare la parola corretta
##### TIPI DI ERRORI DI SPELLING
- **non-word**
    - la parola non esiste nel dizionario
    - es: `teh`, `graffe`
    - facile da individuare (non è nel dizionario)
- **real-word**
    - la parola esiste ma è sbagliata nel contesto
    - es: `form → from`, `there → three`
    - difficile, serve il **contesto**
- **cognitive**
    - errori dovuti a pronuncia/suono simile (homophones)
    - es: `peace → piece`, `two → too`
    - legati a come si pronuncia la parola
##### Risolvere non word spelling
- situazione:
    - l’utente scrive una parola che **non esiste nel dizionario**
	    - es: `teh`, `acress`
- costruisco un sistema che:
	-  **genera candidati**
	    - parole reali simili all'errore
	-  **sceglie il migliore**
	    - tra i candidati
- cosa useremo
	- **query word (x)**
	    - parola sbagliata osservata
	    - es: `acress`
	- **candidate word (w)**
	    - possibile parola corretta
	    - es: `actress`, `across`, `access`
	- **candidate set**
	    - insieme di tutte le possibili correzioni
##### Noisy Channel Model
- utilizzo di noisy channel intuition
	- il **noisy channel model** _(versione base)_ viene usato per stimare la correzione di una **non-word error**
	- volevo scrivere una parola **corretta `w`**
	- ma ho scritto **`x` per errore**
	- dato `x`, trovare la parola corretta `w`
$$\hat{w}=argmax \ P(w | x)$$
- inverto la probabilità applicando bayes:
	- la frazione si toglie perché è una costante
	$$\hat{w}=argmax​ \ P(x∣w)⋅P(w)$$
- la probabilità di errore dipende da:
	- errori comuni
	- vicinanza dei tasti
	- typo frequenti
- **P(x∣w)** → *CHANNEL MODEL PROBABILITY*
	- probabilità di fare errore
	- se volevo scrivere `w`, quanto è probabile scrivere `x`?
	- viene approfondita sotto dopo la confusion matrix!
- **P(w)** → *UNIGRAM PRIOR PROBABILITY*
    - quanto è comune la parola `w`
    - stimata da:
$$P(w) = C(w) / T$$
- dove:  
	- `C(w)` = numero di occorrenze della parola  
	- `T` = numero totale di parole nel corpus
	- più una parola è frequente → più è probabile
###### Computing error probability Confusion matrix
- una matrice che contiene quanto è probabile ogni tipo di errore, sotto una certa trasformazione di quelle viste con la edit distance
	- es:`sub[r,c]` → quante volte `r` diventa `c`
![[Pasted image 20260325112927.png|400]]
###### Channel model
- definizione operativa del channel model
	- modello che descrive **come una parola corretta $w$** viene trasformata nella parola osservata $x$ a causa di errori (typo)
	- fa parte del complesso del noisy channel model, questo approfondisce solo la formula $P(x∣w)$
- vedere nella formula del noisy channel model $P(x∣w)$
![[Pasted image 20260325113523.png]]

- esempio con sub:
	- `sub[c,r]` = quante volte scrivo `c` invece di `r`
		- Hai un dataset di errori, quante volte succede che quella parola viene inserita al posto di un altra
	- `count[r]` = quante volte compare `r`
		- count sta per quante volte compare quella determinata occorrenza
###### uso di edit distance generare candidati
- la **edit distance**
	- algoritmo che ci consente di stimare quali $w$ sono da considerare
	- è il **numero minimo di operazioni** necessarie per trasformare una parola in un’altra	
	- con operazioni semplici come
	- **insertion**
		- inserisco un carattere
	- **deletion**
		- elimino un carattere
	- **substitution**
		- sostituisco un carattere con un altro
	- **transposition**
	    - scambio due caratteri adiacenti
si usa per trovare parole del dizionario **vicine** alla parola sbagliata
- la edit distance si calcola di solito con una **tabella di programmazione dinamica**
- nella tabella:
    - righe = prefissi della prima parola
    - colonne = prefissi della seconda parola
- ogni cella contiene il **costo minimo** per trasformare un prefisso nell’altro
- tabella incompleta ma rende l'idea $\downarrow$ 
![[Pasted image 20260408102725.png]]

![[Pasted image 20260325111724.png]]
- circa **l’80% degli errori** sta entro **edit distance 1**
- quasi tutti gli errori reali stanno entro **edit distance 2**
###### Come generare i candidati
- obiettivo:
    - dato un errore `x`, trovare parole simili nel dizionario
1. **brute force**
    - confronto `x` con tutte le parole del dizionario (edit distance)  
        ❌ troppo costoso
2. **genero tutte le parole con edit distance ≤ k**
    - poi le confronto con il dizionario  
        ❌ ancora costoso se il vocabolario è grande
3. **k-gram index (metodo più importante)**
    - uso i k-gram per trovare parole simili 
        ✔ molto più efficiente
4. **finite state automata / transducer**
    - calcolo veloce della edit distance  
        ✔ più avanzato
5. **liste precomputate**
    - mapping errore → correzioni  
        ✔ veloce ma poco flessibile

![[Pasted image 20260325114032.png]]
## Concetto di smoothing di LaPlace
- problema:
    - usando la confusion matrix
    - alcune probabilità possono essere **0**
	- e questo **azzera tutto il prodotto**
$$P(x∣w)⋅P(w)=0$$
- aggiungo una piccola quantità a tutte le probabilità(tipo 1)  con add-1
- 👉 così:  nessun evento ha probabilità 0
![[Pasted image 20260408105658.png|400]]
#### CONTEXT-SENSITIVE SPELL CORRECTION
- contesto
	- prima abbiamo visto un modello in grado di correggere le *non word*
	- per **real world** errors è necessario un contesto
	- es: _form → from_
	- entrambe le parole sono corrette ma è possibile definire quale delle due è giusta solo in base al contesto
##### Noisy channel in base al contesto per spell correction
- il modello precedente si basava solo ed esclusivamente su una parola alla volta senza poter vedere tutta la frase
- ora (modello con contesto):
    - considero **intere frasi**
    - il modello usa **sequenze di parole**
		- quindi:
			- W = *frase* candidata  
			- X = *frase* osservata
- data la frase X osservata composta da $x_1, x_2, x_3, ..., x_n$
	- ogni parola $x_i$ genera un insieme di parole candidate $w_i$ che potrebbero essere corrette
	- $Candidate(x_i) = { x_i, w_i, w_i', w_i'', ... }$
- *FORMULA IDEATA*
	- $\hat{W}=argmaxP(W∣X)=argmaxP(X∣W)⋅P(W)$
	- questa formula di per se è corretta ma è davvero difficile stimare se tutta la frase è più o meno corretta rispetto a tutte le combinazioni possibili $P(W)$ 
	- la parte con $P(X|W)$ viene trattata come indipendenza delle singole probabilità quindi $P(X|W) = \prod_i P(x_i | w_i)$
- di conseguenza per calcolare $P(W)$ si usa:
##### Bigram model
- per calcolare la $P(W)$ vedo quanto ogni parola è coerente con la precedente e non l'intera frase tutta insieme
	- vado a vedere la probabilità della parola $w_i$ in base alla parola precedente $w_{i-1}$ 
	- questo tipo di modello si chiama *modello markoviano*, i modelli markoviani in generale si basano su anche più parole precedenti ma i *bigram* si fermano a solo 1
	- $P(w_1…w_n) = P(w_1)P(w_2|w_1)…P(w_n|w_{n−1})$
###### Problema di smoothing
- alcuni bigram possono comparire **0 volte**
    - questo porta ad azzerare tutta la probabilità della sequenza  
        → nella produttoria (quella del bigram sopra) basta uno zero per annullare tutto
- si introduce quindi **smoothing** direttamente nella formula di $P(W)$ 
    - soluzione (interpolazione tra bigram e unigram):
    - $P(w_i \mid w_{i-1}) =  (1 - \lambda) \, P_{bigram}(w_i \mid w_{i-1}) + \lambda\, P_{unigram}(w_i)$
    - con 
	    - $P_{bi}(w_k|w_{k−1}) = C(w_{k−1}, w_k) / C(w_{k−1})$
		    - conta quante volte $w_k$ segue $w_{k-1}$
	- e invece $P_{uni}=C(w_i)/C(W)$
- idea:
    - se il bigram non è mai visto → uso l’unigram
    - se invece è affidabile → do più peso al bigram
		- se $\lambda = 1$
		    → uso solo unigram
		- se $\lambda = 0$
		    → uso solo bigram
		- se $\lambda = 0.7$
		    → 70% bigram, 30% unigram
- $λ$ (lambda) è un parametro che:
    - bilancia **contesto (bigram)** e **frequenza globale (unigram)**
		- valore:
		    - lo definisco costruendo un benchmark provando i vari valori di lambda e scegliendo quelli che portano a un risultato migliore 
###### RIDURRE UNDERFLOW CON IL LOGARITMO
- la formula per trovare $P(W)$
	- così com'è rischia di causare underflow ovvero, sottostima dei valori
	- al posto di moltiplicare le varie probabilità delle successioni delle varie parole
	- si va a fare la somma con il logaritmo per avere valori più facili da confrontare
	- $log P(w_1…w_n) = log \ P(w_1) + log \ P(w_2|w_1) + … + log \ P(w_n|w_{n−1})$
	- quindi per tutta la formula del noisy channel avrò
	- $\hat{W}=argmax[log \ P(X∣W)+log \ P(W)]$
- esempio
![[Pasted image 20260329174344.png]]
il modello sceglie:
- **actress** perché “suona giusto nella frase” ed ha più alta probabilità
### Hidden Markov Model (HMM)
- nasce per modellare *sequenze* di stati nel tempo / sequenze simboliche
- idea:
    - ho due livelli:
        - **osservazioni (X)** → ciò che vedo (query sbagliata)
        - **stati nascosti (W)** → ciò che voglio indovinare (frase corretta)
- struttura:
    - **transizioni**
        - $P(w_i \mid w_{i-1})$
        - → successioni di parole
    - **emissioni**
        - $P(x_i \mid w_i)$
        - → le parole possibili da scegliere
- questa struttura si chiama **trellis 
    - un grafo a livelli dove:
        - ogni livello = posizione nella frase
        - ogni nodo = possibile parola candidata
    - si vanno a moltiplicare le varie probabilità di emissione con quella di transizione
![[Pasted image 20260329174642.png]]
- se avessi tutte queste probabilità (transizioni + emissioni)  
    → potrei costruire tutta la struttura e risolvere esattamente il problema  
    → (se le vedo tutte capisco tutto)
- da qui nasce l’**algoritmo di Viterbi**
    - serve per:
        - trovare la sequenza $W$ più probabile
    - idea:
        - invece di provare tutte le combinazioni (esponenziale)
        - uso **programmazione dinamica**
        - tengo solo i percorsi migliori passo passo
    - in pratica:
        - trova il **cammino migliore nel trellis**
- nota:
    - è il metodo corretto “teoricamente”
    - ma qui **non viene usato davvero**
        - sarebbe troppo pesante / overkill per IR
- quindi si fa una **semplificazione pratica**
    - assumo:
        - **1 errore per frase** *One error per sentence*
    - invece di esplorare tutto:
        - genero alcune frasi candidate
        - confronto le probabilità
        - scelgo la migliore
- **rischio: la probabilità può “falsare tutto”**
    - il modello tende a favorire parole molto frequenti
    - anche se nel contesto sono sbagliate
    - esempio:
        - parola rara ma corretta → penalizzata
        - parola comune ma sbagliata → favorita
    - quindi si rischia di 
		- correggere **quando non serve**
		- o correggere **male**
###### Peter Norvig, le probabilità di tutti i cambiamenti
- introduco:
	- $P(w \mid w)$
- probabilità che la parola sia già corretta  
- idea:
    - assumo che:
        - l’utente **di solito scrive giusto**
    - quindi:
        - $P(w \mid w)$ è **alto** (es. 0.9–0.99)
- problema:
    - questa probabilità compete con le altre
    - se troppo alta:  
        → non correggo mai
    - se troppo bassa:  
        → correggo troppo
- **introduco un peso (β)**
$\arg\max P(X \mid W)\cdot P(W)^\beta$
- serve per:
    - ridurre l’effetto della frequenza $P(W)$
    - evitare che parole comuni dominino sempre
