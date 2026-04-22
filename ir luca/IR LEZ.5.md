### RANKED RETRIEVAL
Nel modello Booleano classico, un documento o soddisfa la query oppure no. Questo però crea un problema pratico: spesso otteniamo o troppi risultati oppure nessuno. Questo fenomeno è chiamato _feast or famine_: con AND restringiamo troppo, con OR allarghiamo troppo.
Per risolvere questo limite si introduce il **ranked retrieval**, dove invece di filtrare i documenti, li **ordiniamo per rilevanza**. L’idea è quindi assegnare uno **score** a ogni coppia (query, documento), che misura quanto il documento è rilevante rispetto alla query, e poi restituire solo i migliori (top-k).
In questo modo otteniamo una sorta di **soft AND**: non richiedo che tutte le parole siano presenti, ma preferisco i documenti che matchano meglio la query.
##### Primo tentativo: confronto come insiemi
Un primo modo per misurare la similarità tra query e documento è trattarli come **insiemi di parole** e usare la **Jaccard similarity**:
$\frac{|A \cap B|}{|A \cup B|}$
- $A$ = **documento**  
- $B$ = **query**
Questa misura cattura quanto i due insiemi si sovrappongono. Tuttavia ha due limiti importanti:
- non considera quante volte compare una parola
- non distingue tra parole frequenti e parole rare
Quindi è troppo semplice per un sistema di retrieval efficace.

#### VECTOR SPACE MODEL
Per superare questi limiti si introduce il **Vector Space Model**. L’idea è *rappresentare* sia i *documenti* sia le *query* come **vettori in uno spazio ad alta dimensionalità**, dove ogni dimensione corrisponde a un termine del vocabolario. A questo punto non ragiono più solo in termini di “parola presente o assente”, ma posso associare a ogni termine un certo peso, che dipende dalla sua importanza nel documento e nella collezione.
###### Come rappresentiamo i documenti in questo modello
Per migliorare il modello, dobbiamo rappresentare i documenti in modo più informativo.
Nel caso più semplice usiamo una **incidence matrix**, dove ogni documento è rappresentato da un vettore binario:
- 1 se la parola è presente
- 0 se non è presente
e dove:
- righe → termini
- colonne → documenti
Questo però è ancora limitato, perché non tiene conto della frequenza.
Per questo passiamo alla **count matrix**, dove ogni documento è rappresentato da un vettore di conteggi:
- ogni componente indica *quante volte compare un termine*
Questa rappresentazione è più ricca, perché introduce l’idea che:

> più una parola compare in un documento, più quel documento è rilevante per quella parola

NON VENGONO USATE DAVVERO POI NEL CONCRETO SI USANO LE INCIDENCE MATRIX PER AVERE FREQUENZA O ALTRO

![[Pasted image 20260329183548.png|400]]
##### Bag of words
In tutto questo modello assumiamo il **bag of words**:
- *ignoriamo* completamente *l’ordine delle parole*
- consideriamo solo quali parole compaiono e quante volte
Questo semplifica molto il problema, anche se perdiamo informazione sul contesto
- il seguente modello funziona quindi per $tf$ (term frequency)
- $tf_{t,d​}$=numero di volte che il termine t appare nel documento d
###### Problema della frequenza
A questo punto potremmo pensare di usare direttamente il numero di occorrenze ($tf$), ma c’è un problema:
se una *parola compare 10 volte* in un documento, *non significa* che quel documento *sia 10 volte più rilevante* rispetto a uno in cui compare una sola volta.
Quindi la *crescita della rilevanza* rispetto alla frequenza **non è lineare**.
*non è linearmente proporzionale*
###### Soluzione: logaritmo
Per gestire questo problema si usa una trasformazione logaritmica:
Per sistemare questo problema introduco un **peso**:

$w_{t,d} = \begin{cases} 1 + \log(tf_{t,d}) & \text{se } tf_{t,d} > 0 \\ 0 & \text{altrimenti} \end{cases}$
- questo sistema studiato sopra non funziona benissimo
- la funzione di ordinamento sarà proporzionale al contributo delle singole parole nel documento come esse sono riportate all'interno della query
Il modello basato solo su $tf$ (anche con il log) non è ancora sufficiente.
Infatti:
- parole molto frequenti (es. _the, is, and_) compaiono in quasi tutti i documenti
- quindi hanno un $tf$ alto, ma **non sono informative**
👉 problema:
- il modello darebbe troppo peso a parole inutili
- problema: parole troppo frequenti prenderebbero ogni volta un risultato
### Introduzione dell’idf (inverse document frequency)
Per risolvere questo problema introduciamo una misura della **rarità del termine nella collezione**.
- definiamo quindi:
$idf_t = \log \frac{N}{df_t}$
dove:
- $N$ = numero totale di documenti
- $df_t$=numero di documenti che contengono il termine t
![[Pasted image 20260329185228.png|400]]
### Intuizione dell’idf
- se un termine compare in **pochi documenti**:
    - $df_t$​ piccolo → $idf_t$​ alto  
        → termine molto informativo
- se un termine compare in **tutti i documenti**:
    - $df_t \approx N$ → $idf_t \approx 0$
        → termine poco utile
- parole rare → **peso alto**
- parole frequenti → **peso basso**
### tf-idf: peso finale del termine
A questo punto combiniamo:
- frequenza nel documento ($tf$)
- rarità nella collezione ($idf$)
$tf-idf\ w_{t,d} = (1 + \log tf_{t,d}) \cdot \log \frac{N}{df_t}$
Un termine ha peso alto se:
- compare spesso nel documento
- ma compare poco nella collezione
- è **rappresentativo di quel documento**
##### Collegamento con il ranking
A questo punto:
- ogni documento è rappresentato come vettore di pesi $w_{t,d}$
- ogni query viene rappresentata allo stesso modo
👉 il ranking dipenderà da:
- quanto i pesi dei termini della query sono presenti nel documento
##### Nota sulle stop words
- parole molto frequenti (the, is, ecc.)
    - hanno $idf \approx 0$
    - quindi contribuiscono pochissimo
per questo:
- spesso vengono direttamente rimosse (stop words)
##### Query come vettori
Dopo aver calcolato i pesi **tf-idf** per ogni termine in un documento, possiamo rappresentare sia i documenti che le query come **vettori** in uno spazio ad alta dimensionalità.
* **Vettori Documento:** Ogni documento $d$ è un vettore di pesi tf-idf $\vec{d} \in \mathbb{R}^{|V|}$, dove $|V|$ è la dimensione del vocabolario (numero di termini distinti).
* **Assi dello Spazio:** I termini del vocabolario fungono da assi in questo spazio.
* **Vettori Sparsi:** La maggior parte delle componenti di questi vettori è zero, poiché un documento contiene solo una piccola frazione di tutti i termini possibili.
##### Rappresentazione Vettoriale delle Query
L'idea chiave è applicare la stessa logica anche alle query:
* **Query come Vettori:** Anche le query possono essere rappresentate come vettori nello stesso spazio ad alta dimensionalità dei documenti, utilizzando gli stessi pesi tf-idf.
Il vettore della query è costruito così:
$$q = [w_{1,q}, w_{2,q}, ..., w_{M,q}]$$
dove ogni componente è:
$$w_{t,q} = tf_{t,q} \cdot idf_t$$

- **$tf_{t,q}$** = numero di volte che il termine compare **nella query**
- **$idf_t$​** = importanza del termine calcolata **sul corpus dei documenti**
* **Ranking per Prossimità:** L'obiettivo è ordinare i documenti in base alla loro "prossimità" o "somiglianza" alla query.
    * **Prossimità ≈ Similarità ≈ Distanza Negativa.**
    * Vogliamo quindi ordinare i documenti in ordine inverso rispetto alla distanza del loro vettore dal vettore della query.
* **Definire la Distanza:** Il problema diventa definire una metrica di distanza (o similarità) tra questi vettori di termini.
###### Perché la Distanza Euclidea non è Adatta
Un primo approccio potrebbe essere usare la distanza euclidea tra i punti finali dei vettori. 
- distanza tra gli endpoint dei vettori
Tuttavia, questa metrica ha dei limiti significativi:
* **Sensibilità alla Lunghezza:** La distanza euclidea è pesantemente influenzata dalla lunghezza dei vettori. Un documento lungo (che contiene molte più occorrenze di termini) avrà un vettore di lunghezza maggiore, anche se semanticamente simile a un documento più corto.
    * **Esempio:** Se un documento $d'$ è identico a un documento $d$ ma ripetuto due volte (quindi due volte più lungo), semanticamente sono identici. L'angolo tra i loro vettori sarà 0, indicando massima similarità. Tuttavia, la distanza euclidea tra $d$ e $d'$ risulterà grande, suggerendo bassa similarità.
    * Questo è un problema perché si darebbe un peso eccessivo a documenti semplicemente più lunghi, anche se non più rilevanti. Un classico esempio sarebbe un **dizionario** che, contenendo un'enorme quantità di parole, risulterebbe sempre tra i primi risultati se non si applicasse una normalizzazione per la lunghezza 
![[Pasted image 20260408115417.png]]
##### Similarità tramite Angoli: La Funzione Coseno
Un approccio migliore è ordinare i documenti in base all'angolo che il loro vettore forma con il vettore della query.
* **Angolo vs Distanza:**
    * Quando due vettori sono **coincidenti** (stessa direzione, massima similarità), l'angolo è **0 gradi**.
    * Quando due vettori sono **ortogonali** (perpendicolari, nessuna similarità), l'angolo è **90 gradi**.
*   **Funzione Coseno:** La funzione coseno è **monotonamente decrescente** nell'intervallo $[0, \pi]$ (o $[0, 180°]$), il che significa che:
    *   $\cos(0°) = 1$ (massima similarità)
    *   $\cos(90°) = 0$ (nessuna similarità)
    *   $\cos(180°) = -1$ (massima dissimilarità, ma non si usa in IR con pesi positivi).
    * se il coseno è grande → angolo piccolo  
    * se il coseno è piccolo → angolo grande
* **Equivalenza:** Rankare i documenti secondo l'angolo decrescente è equivalente a rankarli secondo il valore del coseno crescente.
* **Vettori TF-IDF:** I vettori di query e documento sono formati dai pesi tf-idf 
![[Pasted image 20260408115356.png]]

##### Normalizzazione della Lunghezza del Vettore
Per rendere l'angolo una misura efficace di similarità, è essenziale che la lunghezza dei vettori non influenzi il risultato.
* **Normalizzazione L2 (Euclidea):** Un vettore può essere normalizzato dividendo ogni sua componente per la sua lunghezza (o norma euclidea).$$||\vec{x}||_2 = \sqrt{\sum_i x_i^2}$$
    Dopo la normalizzazione, i vettori avranno lunghezza 1 e saranno proiettati su una sfera unitaria.
	- la norma euclidea di un vettore si ottiene facendo la radice quadrata della somma dei quadrati delle sue componenti
* **Benefici:** Documenti più lunghi e più corti avranno pesi dello stesso ordine di grandezza dopo la normalizzazione. Questo risolve il problema dei documenti lunghi che dominerebbero il ranking senza normalizzazione. Il documento $d'$ (doppio di $d$) dopo la normalizzazione avrà lo stesso vettore di $d$.
##### Similarità del Coseno (Cosine Similarity)
Per vettori normalizzati, la similarità del coseno è equivalente al **prodotto scalare (dot product)** tra i vettori.

* **Formula:** La similarità del coseno tra una query $\vec{q}$ e un documento $\vec{d}$ è definita come: $$\text{cos}(\vec{q}, \vec{d}) = \text{SIM}(\vec{q}, \vec{d}) = \frac{\vec{q} \cdot \vec{d}}{||\vec{q}|| \cdot ||\vec{d}||} = \frac{\sum_{i=1}^{|V|} q_i d_i}{\sqrt{\sum_{i=1}^{|V|} q_i^2} \sqrt{\sum_{i=1}^{|V|} d_i^2}}$$
    dove:
    *   $q_i$ è il peso tf-idf del termine $i$ nella query.
    *   $d_i$ è il peso tf-idf del termine $i$ nel documento.
    *   $||\vec{q}||$ e $||\vec{d}||$ sono le lunghezze euclidee dei vettori.
* **Range:** La similarità del coseno assume valori tra 0 e 1.
* **Svantaggio (Data Sparseness):** Il modello "Bag of Words" (usato con i vettori tf-idf) non cattura relazioni semantiche tra parole diverse. Ad esempio, "cane" e "rottweiler" potrebbero essere trattati come termini ortogonali (nessuna somiglianza diretta) se non si usano tecniche più avanzate. Tuttavia, la sua semplicità e efficacia ne giustificano l'uso.

L'esempio con i romanzi mostra come:
1.  Si parta dalle frequenze grezze dei termini (`affection`, `jealous`, `gossip`, `wuthering`).
2.  Si applica il **log-frequency weighting** per smorzare l'impatto delle alte frequenze.
3.  Si normalizzino i vettori (in questo esempio, senza idf per semplicità).
4.  Si calcolano le similarità del coseno, mostrando che `Sense and Sensibility` e `Pride and Prejudice` sono più simili tra loro di quanto lo siano con `Wuthering Heights`, riflettendo la vicinanza stilistica e tematica.

##### Calcolo del Punteggio Coseno (COSINESCORE)
L'algoritmo per calcolare i punteggi di similarità del coseno tra una query e l'intera collezione di documenti è il seguente:

![[Pasted image 20260407123204.png|400]]

* **Approccio "Term-at-a-time" (TAAT):** Questo algoritmo elabora un termine della query alla volta, scorrendo la sua posting list e aggiornando i punteggi dei documenti.
* **Ottimizzazioni:**
    * **Memorizzazione dei pesi:** Memorizzare $w_{t,d}$ (un numero floating-point) in ogni posting può essere costoso. È più efficiente memorizzare solo $\text{tf}_{t,d}$ nella posting e l'$\text{idf}_t$ nell'header della posting list (o nel dizionario), calcolando $w_{t,d}$ al momento.
    * **Pre-filtering:** Per evitare di scorrere tutte le posting list per ogni termine della query, si può applicare un filtro iniziale (ad esempio, un OR di tutti i termini della query) per selezionare un sottoinsieme più piccolo di documenti candidati da valutare in dettaglio.
    * **Estrazione Top K:** I $K$ elementi con i punteggi più alti possono essere estratti efficientemente usando una **priority queue** (ad esempio, un heap).

##### Varianti di Pesatura (tf e idf)
Esistono numerose varianti per le funzioni di pesatura tf e idf, ognuna con diverse assunzioni e scopi:
#### Varianti di tf Weighting
*   **Natural ($\text{TF}_{\text{total}}(t,d) = n(t,d)$):** Semplicemente il conteggio grezzo. Non ha normalizzazione per la lunghezza del documento, favorendo i documenti più lunghi.
*   **Boolean ($\text{TF}_{\text{bool}}(t,d)$):** 1 se il termine è presente, 0 altrimenti.
*   **Sum ($\text{TF}_{\text{sum}}(t,d) = n(t,d)/N(d)$):** Normalizza per la lunghezza del documento. Tutti i documenti con la stessa frazione di occorrenze avrebbero lo stesso punteggio.
*   **Max ($\text{TF}_{\text{max}}(t,d)$):** Normalizza per la frequenza massima del termine nel documento.
*   **Logarithmic ($\text{TF}_{\text{log}}(t,d) = \log(1+n(t,d))$):** Smorza la crescita della frequenza, come già visto.
*   **Fraction ($\text{TF}_{\text{frac}}(t,d; k) = n(t,d) / (n(t,d) + k)$):** Introduce un guadagno marginale decrescente. Mentre il logaritmo cresce all'infinito, queste funzioni tendono a saturare verso 1. Il parametro $k$ influisce sulla pendenza di questa saturazione (come mostrato nel grafico della slide).
*   **BM25:** Una delle funzioni più avanzate e popolari, basata su un **paradigma probabilistico**

##### Varianti di idf Weighting
Anche per l'idf esistono diverse formulazioni, che variano principalmente nel modo in cui gestiscono il logaritmo e le costanti aggiunte per lo smoothing, influenzando come si "smorzano" i termini rari e comuni.
*   **Conclusione sulle Varianti:** Non esiste un singolo schema di pesatura "migliore" in assoluto. La scelta dipende dall'ambiente specifico, dal tipo di collezione e dalle esigenze dell'applicazione.
* Ogni variante offre un trade-off tra complessità, accuratezza e performance.
