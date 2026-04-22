#### VALUTAZIONE DEI SISTEMI
- obiettivo: restituire all’utente qualcosa che lo soddisfi
    - problema: la soddisfazione dell’utente è **qualcosa di soggettivo**
        - dipende dalla persona, dal contesto, dall’interfaccia, ecc.
    - quindi nasce una domanda:
        - _come posso misurare in modo oggettivo qualcosa che è soggettivo?_
        - evitando bias e interpretazioni personali
- soluzione: introdurre un metodo **standard e replicabile**
    - si usa un **benchmark**, cioè un ambiente controllato di valutazione
    - composto da:
        - una **collezione di documenti**
            - definisce il perimetro su cui lavoriamo
        - un insieme di **query**
            - rappresentano bisogni informativi realistici
            - devono essere riutilizzabili per confronti futuri
        - un insieme di **giudizi di rilevanza**
            - per ogni coppia (query, documento)
            - si stabilisce se il documento è rilevante oppure no
        - tutto questo serve a rendere la valutazione **riproducibile**
- idea chiave:
    - non possiamo misurare direttamente la “felicità” dell’utente
    - allora usiamo una **proxy**, cioè una misura indiretta che la approssima
        - in questo caso: la **rilevanza dei documenti**
    - quindi assumiamo:
        - se i documenti restituiti sono rilevanti → l’utente sarà soddisfatto
        - non è perfetto, ma è una buona approssimazione nella pratica
- cambio di paradigma:
    - si passa da valutazioni soggettive a una **valutazione empirica**
    - grazie ai benchmark possiamo:
        - confrontare sistemi diversi in modo equo
        - capire quale funziona meglio
        - migliorare i modelli nel tempo in modo sistematico
- osservazione importante:
    - è difficile isolare e confrontare singole componenti (ranking, pesi, stemming, ecc.)
    - quindi nella pratica si valuta:
        - **l’intero sistema di IR**
        - guardando l’output finale rispetto al bisogno informativo dell’utente
##### Processo Gold Standard
- non è il benchmark in sé
    - ma il **processo di costruzione dei dati etichettati**
    - cioè il dataset che poi useremo per valutare i sistemi
- nomi equivalenti:
    - **gold standard**
    - **dataset oracolo**
    - **dataset annotato**
    - → rappresenta la “verità di riferimento” (ground truth)
- idea:
    - voglio costruire un dataset dove so già, per ogni query:
        - quali documenti sono rilevanti
        - quali non lo sono
    - questo dataset servirà poi per valutare qualsiasi sistema IR
- Step 1: preparare delle **query rappresentative**
    - devono riflettere information need reali
    - non query casuali
- Step 2: recuperare i **documenti candidati**
    - uso un sistema IR “di base”
    - obiettivo: **massimizzare la recall**
        - quindi prendere _tutti_ i possibili documenti rilevanti
    - accetto il fatto che:
        - ci saranno molti **falsi positivi**
    - → questo step definisce il **perimetro di analisi**
- Step 3: **annotazione umana**
    - per ogni query, degli esperti valutano i documenti
    - decidono se sono:
        - rilevanti / non rilevanti
    - spesso:
        - ci sono **più annotatori**
        - per ridurre errori e soggettività
- osservazione:
    - la rilevanza non è oggettiva al 100%
    - quindi avere più giudici aumenta l’affidabilità
- risultato finale:
    - otteniamo una **collezione annotata**
        - documenti + query + giudizi di rilevanza
    - questo è ciò che useremo come **benchmark**
#### Precision & Recall
- idea di base:
    - confronto:
        - i documenti **rilevanti nel gold standard**
        - con quelli **restituiti dal sistema IR**
    - non basta dire “giusto/sbagliato”
        - perché il sistema restituisce **insiemi di documenti**, non singole risposte
    - quindi servono metriche più adatte → **precision** e **recall**
- rappresentazione:
    - ci sono 4 configurazioni possibili:
        - **True Positive (TP)** → documento rilevante e restituito
        - **False Positive (FP)** → documento non rilevante ma restituito
        - **False Negative (FN)** → documento rilevante ma NON restituito
        - **True Negative (TN)** → documento non rilevante e NON restituito
![[Pasted image 20260410173204.png|300]]
![[Pasted image 20260410173225.png|300]]
- formule:
    - **Precision**
        - tra i documenti che ho preso, quanti sono davvero rilevanti?
        - $$P = \frac{TP}{TP + FP}$$
    - **Recall**
        - tra tutti i documenti rilevanti, quanti sono riuscito a trovare?
        - $$R = \frac{TP}{TP + FN}$$
>[!info]- quando usare precision vs recall:
>    - **alta precision**
>        - quando voglio pochi risultati ma buoni
>        - es: web search (prima pagina)
>    - **alta recall**
>        - quando non voglio perdere nulla
>        - es: ambito legale, medico, intelligence

- trade-off:
    - precision e recall sono in competizione:
        - se prendo pochi documenti → alta precision, bassa recall
        - se prendo tutto → alta recall, bassa precision
    - non posso massimizzare entrambe contemporaneamente
- caso ideale:
    - precision = 1 e recall = 1
        - → ho preso **tutti e solo** i documenti rilevanti
    - altri casi:
        - precision = 1, recall = 0 → non ho preso nulla
        - recall = 1, precision = 0 → ho preso tutto (anche spazzatura)
![[Pasted image 20260410173621.png|400]]
- combinare precision e recall:
    - non uso la media aritmetica
        - perché può essere ingannevole (basta avere precision a 1 e recall a 0 per dare 0.5)
- come misura potrei anche usare:
	- **accuracy**
	    - misura: quante classificazioni sono corrette sul totale
        - $$accuracy = \frac{TP + TN}{TP + FP + FN + TN}$$
    - **perché NON si usa in IR**
        - nei sistemi di retrieval:
            - i documenti non rilevanti (TN) sono **enormemente di più** dei rilevanti
        - quindi:
            - anche un sistema stupido che non restituisce nulla avrebbe accuracy altissima
- **error rate**
    - è semplicemente:
        - $$error = 1 - accuracy$$
voglio una singola misura che tenga conto di entrambe
- soluzione:
    - uso la **media armonica**
        - cioè una media che dà più peso ai valori piccoli
        - quindi:
            - se precision o recall è bassa → il risultato finale scende molto
		- **F-Measure**
		    - combina precision e recall
	        - **F1-measure**
	            - $$F1 = \frac{2PR}{P + R}$$
			- i sistemi non scrivono `F-measure` ma F1-measure 
				- perché applicano dei pesi alla precision e recall con F1 la potenza sta a 1
		    - F1 è alta solo se:
		        - precision è alta
		        - recall è alta
			    - quindi misura il **bilanciamento tra le due**
![[Pasted image 20260410174904.png|500]]
- problema importante:
    - precision e recall **globali** non raccontano tutto
    - perché:
        - nel ranking l’ordine conta!
	    - esempio in foto:
		    - sistema A mette documenti rilevanti subito
		    - sistema B li mette in fondo
		    - → stessi P e R, ma qualità percepita diversa
- quindi:
    - valutare solo su tutto il set è limitante
    - introduciamo misure basate su ranking
#### Rank based measures
servono metriche che tengano conto della **posizione**
##### A Rilevanza binaria
- una risposta può essere rilevante oppure no
	- 1 o 0
###### Precision@K
- idea:
    - guardo solo i **primi K risultati**
    - calcolo quanti sono rilevanti
- formula:
    - $Precision@K = \frac{\text{relevant nei primi K}}{K}$
- interpretazione:
    - misura la qualità dei risultati **che l’utente vede davvero**
    - molto importante per:
        - web search
	- calcolo le rilevanze dei primi K
	- Prec@3 2/3
	- Prec@4 2/4
- Esiste anche $Recall@K$
![[Pasted image 20260410175036.png|400]]
- i quadratini sono i risultati della singola query
- anche qui precision e recall cambiano al variare di K
- quindi voglio capire:
    - come evolve il sistema al variare della soglia
- costruisco un grafico:
	- ogni punto corrisponde a un valore di K
		- ![[Pasted image 20260410175412.png|300]]
- problema:
    - la curva precision-recall reale è **irregolare (a zig-zag)**
        - perché:
            - quando aggiungo un documento non rilevante → precision scende
            - quando aggiungo uno rilevante → precision sale
    - quindi:
        - è difficile confrontare due sistemi direttamente
- vado a **interpolare i dati del grafico**
	- cioè costruisco una nuova curva più regolare
		- $P(R) = \max \{ P' : R' \geq R \ \land \ (R', P') \in S \}$
			- per un certo valore di recall $R$:
		    - considero tutti i punti della curva con recall $R' \geq R$
		    - tra questi prendo la **precision massima**
				- quindi:
				    - non uso il valore “locale”
				    - ma il **miglior valore ottenibile da quel punto in poi**
	- è **monotona decrescente**
		- la curva interpolata rappresenta una sorta di:
	        - **impronta digitale del sistema**
	    - descrive come il sistema si comporta al variare del recall
	        - in modo più stabile e meno rumoroso
    - dopo l’interpolazione:
        - posso confrontare correttamente sistemi diversi
        - (es: Bing vs Google)
		- ![[Pasted image 20260410180214.png|400]]
	        - confronto curve “lisce” e non influenzate da oscillazioni casuali
	- posso confrontarli su un breakeven point
	    - **break-even point**
	        - punto in cui:
	            - precision = recall
		    - è solo un riassunto
##### Average Precision (AP) e Mean Average Precision (MAP)
- sono comunque binarie
- *Average Precision (AP)*
	- è la metrica per **una singola query**
	- idea:
	    - non guardo un singolo K
	    - ma considero:
	        - **tutte le posizioni in cui trovo un documento rilevante**
	- procedimento:
	    - ogni volta che trovo un documento rilevante:
	        - calcolo la **Precision@K** in quella posizione
	    - poi faccio la **media** di questi valori
	-  non guardo un singolo K
	    - ma considero:
	        - **tutte le posizioni in cui trovo un documento rilevante**
	    - ogni volta che trovo un documento rilevante:
	        - calcolo la **Precision@K** in quella posizione
    - poi faccio la media di questi valori
![[Pasted image 20260410180804.png|500]]
- *Mean Average Precision (MAP)*
	- estensione della AP a **più query**
	    - è la **media delle AP** su tutte le query
- $MAP = \frac{1}{|Q|} \sum_{q \in Q} AP(q)$
    - prima calcolo $AP$ per ogni query
    - poi faccio la media
	- MAP è una media di tipo macro
	- micro vs macro
		- micro: considera tutte le decisioni insieme, query con più documenti pesano di più
		- macro: ogni query pesa allo stesso modo indipendentemente da quanti documenti rilevanti ha
- MAP utilizzata ancora tutt'oggi
##### A più livelli di rilevanza
- introduciamo una **scala di rilevanza**
	- es: 0, 1, 2, 3
- i più rilevanti devono stare **più in alto nel ranking**
###### Discounted Cumulative Gain
- è previsto che il guadagno informativo scenda in modo proporzionale all'ordine di apparizione dei risultati
- voglio misurare il **guadagno informativo totale**
- tenendo conto di:
    1. **quanto è rilevante un documento (gain)**
    2. **dove si trova nel ranking (discount)**
- si divide in:
	- *GAIN*
		- è la “qualità” del documento
		- dipende dal livello di rilevanza:
		    - più è alto → più contribuisce
	    - uso il valore di rilevanza $r_i$
	- *DISCOUNT*
		- tiene conto della posizione nel ranking
		    - un documento in alto vale di più
		    - uno in basso vale meno
		    - $\frac{1}{\log_2(i)}$
	- partiamo da un range di giudizio di rilevanza $[0,r]$ con $r>2$
	- il cumulative gain è la somma dei range di rilevanza
$$CG = r_1+r_2+..r_n$$
- il Discount cumulative Gain
	- misura i contributi dei documenti in ordine di apparizione
		- quelli più in fondo pesano meno
$$DCG_p = rel_1 + \sum_{i=2}^{p} \frac{rel_i}{\log_2 i}$$

###### NDCG (Normalized Discounted Cumulative Gain)
- il DCG che abbiamo visto prima misura quanto è buono un ranking tenendo conto sia della rilevanza dei documenti sia della loro posizione.  
    Però ha un problema importante: **il valore del DCG da solo non è facilmente confrontabile tra query diverse**.  
    Questo succede perché:
    - ogni query può avere un numero diverso di documenti rilevanti
    - e distribuzioni di rilevanza diverse
- per risolvere questo problema si introduce l’**NDCG**, che è semplicemente una **versione normalizzata del DCG**.  
    - prendo il mio risultato reale
    - lo confronto con il **miglior risultato possibile**(ranking ideale)
$$NDCG = \frac{DCG}{IDCG}$$

#### Cosa succede se i risultati non sono in una lista?
- ci sono casi in cui:
    - **non ci interessa una lista di documenti**
    - ma trovare **un solo risultato corretto**
        - il famoso _ago in un pagliaio_
    - tipo **fact retrieval**
        - es: “capitale del Giappone”
- in questi casi:
    - metriche come **precision, recall, MAP**
        - **non sono adatte**
    - perché:
        - non mi interessa _quanti_ documenti rilevanti trovo
        - ma **quanto velocemente trovo il primo giusto**
##### Mean Reciprocal Rank (MRR)
- guardo **la posizione del primo documento rilevante**
    - se il primo documento rilevante è in posizione $K$:
        - il punteggio è:
            $RR = \frac{1}{K}$
    - più il documento è **in alto**  
        → più il punteggio è alto
- **Mean Reciprocal Rank (MRR)**:
    - estensione a più query
    - faccio la media dei reciprocal rank:
        $MRR = \frac{1}{|Q|} \sum_{q \in Q} \frac{1}{rank_q}$
#### PROBLEMA: gli human judgments sono costosi
- i **giudizi umani di rilevanza**:
    - sono **costosi**
    - richiedono tempo ed esperti
    - sono anche:
        - **inconsistenti** (tra annotatori diversi)
        - **variabili nel tempo**
#### Soluzione: usare il comportamento degli utenti (user behavior)
- sfruttare i **click degli utenti**
- come segnale implicito di rilevanza
    - se un documento viene cliccato spesso → probabilmente è rilevante
    - dati **abbondanti**
    - raccolti automaticamente
    - ma i click **non sono perfetti**
###### Click Position Bias
- gli utenti: tendono a cliccare di più i risultati **in alto**
    - un documento può ricevere molti click:
        - **non perché è rilevante**
        - ma perché è **in prima posizione**
    - i click sono:
        - **informativi**
        - ma anche **biased** (distorti)
![[Pasted image 20260418083556.png|300]]
- anche invertendo i risultati, quelli in alto ricevono comunque più attenzione
#### Valutazioni relative (pairwise)
- invece di definire la rilevanza del documento in modo binario “questo documento è rilevante o no”
    - si effettua una valutazione relativa dove si dice
	    - **Doc A è meglio di Doc B**
    - mediante i click per inferire **preferenze relative**
    - più robusto rispetto al bias assoluto
#### Confronto tra ranking: Interleaving + click
- ho due ranking:
    - ranking A
    - ranking B
- costruisco un **ranking interleaved**:
    - mescolo i risultati dei due
    - rimuovo duplicati
- mostro la lista all’utente
- conto i click:
    - quanti su A
    - quanti su B
- conclusione:
    - il ranking con più click è **migliore (in media)**
- foto sotto: ho due liste di risultati, le intervallo e le metto in una unica lista, tolgo i duplicati e conto quali vengono presi di più
![[Pasted image 20260418082919.png|500]]
![[Pasted image 20260418082931.png|500]]
#### A/B Testing
- è diverso dall’interleaving
- idea:
    - divido gli utenti in due gruppi:
        - gruppo A → sistema vecchio
        - gruppo B → sistema nuovo
- confronto metriche:
    - click
    - tempo
    - conversioni
- serve per:
    - testare **una modifica reale in produzione**
