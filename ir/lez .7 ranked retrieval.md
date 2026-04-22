**feast or famine**: gli operatori `AND` ed `OR` restringono o allargano troppo lo spazio dei risultati.

**soluzione a feast or famine**: ranked retrieval. Invece di filtrare i documenti vengono ordinati per rilevanza.

**score**: ogni coppia $\text{(query, documento)}$. misura quanto $\text{documento}$ e' rilevante rispetto alla $\text{query}$. restituisco solo i migliori $k$.

**jaccard similarity**: confronto come insiemi. Ma non funziona per il retrieval, non considera:
* quante volte compare una parola.
* quali parole sono frequenti ed altre rare.

## Vector space model
**Vector Space Model**: rappresentare **documenti** e **query** come vettori in uno spazio ad alta dimensionalità.

**cos'e' una dimensione**? ad ogni dimensione corrisponde un termine del vocabolario, ogni termine ha un certo peso, che dipende dalla sua importanza nel documento e nella collezione.

## modelli
**incidence matrix**: ogni documento e' rappresentato da un vettore binario. ma non tiene conto della frequenza.

**count matrix**: indica quante volte compare il termine in ogni documento.

**idea**: piu il termine compare nel documento e piu' il documento e' rilevante per quella parola.

**gemini spiega**: NON VENGONO USATE DAVVERO POI NEL CONCRETO SI USANO LE INCIDENCE MATRIX PER AVERE FREQUENZA O ALTRO

**bag of words**: ignoro l'ordine delle parole. considero quali compaiono e quante volte.

**perdita di contesto**: con bag of words perdiamo l'ordine delle parole ma semplifico il problema.

## term frequency
**term frequency**: $\text{tf}_{t,d} = \text{quante volte } t \text{ occorre in } d$.
**problema**: se un termine compare 10 volte in un documento, allora non e' necessariamente piu rilevante di un documento in cui compare una sola volta.

**soluzione**: introduco il **peso**, non voglio che la crescita dell'importanza del documento si lineare in $\text{tf}_{t,d}$ $$w_{t,d} = \begin{cases} 1 + \log_{10}(tf_{t,d}) & \text{se } tf_{t,d} > 0 \\ 0 & \text{altrimenti} \end{cases}$$

$\text{tg-matching-score}(q,d)=\sum_{t\in q\cap d}(1 + \log \text{tf}_{t,d})$

**problema di** $\text{tf}$:: parole come `the`, `is`, `and` compaiono spesso, ma sono poco informative.

**gemini**: dimmi le conseguenze di sta roba e perche' introduco `idf`.

## idf
**frequenza nella collezione**: considero quanto un termine e' frequente nell'intero set di documenti prima di decidere quanto e' importante un termine.

**regola**: pesi alti per termini rari come `Phenethylamine`

**pesi dei termini frequenti**: hanno comunque pesi positivi ma più leggeri dei termini rari.

**misura della quantità di informazione di un termine**: $\text{idf}_{t} = \log_{10} \frac{N}{\text{df}_{t}}$
* $\text{df}_{t}$: e' il numero di documenti in cui $t$ occorre.
* **ammorbidire** l'effetto di idf: $\frac {\log N}{\text{df}_{t}}$ invece di $\frac{N}{\text{df}_{t}}$

**relazione tra $\text{df}_{t}$ e $\text{idf}_{t}$**: $\text{df}_{t}$ **piccolo** implica un $\text{idf}_{t}$ **alto** e viceversa.


## tf-idf
**peso** $\text{tf-idf}$: e' il **prodotto** dei pesi $\text{tf}$ e $idf$.
* $w_{t,d} = (1 + \log \text{tf}_{t,d}) \log \frac{N}{\text{df}_{t}}$
* all'aumentare di $w_{t,d}$ aumenta il numero di occorrenze nel documento e la rarità nella collezione.

**documento come vettore**: rappresento un documento come un vettore di pesi $\text{tf-idf}$ in $\mathbb R^{|V|}$. 
* **dimensioni**: $|V|$ che e' grande.
* **vettore**: molto sparso.

key










