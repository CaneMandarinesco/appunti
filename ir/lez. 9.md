**approccio probabilistico**: otterremo un sistema che performa bene tanto quanto l'approccio vettoriale utilizzando le probabilità.
* permette di ragionare su eventi incerti esprimendoli come probabilita.

**domanda da studiare**: quanto e' rilevante $d$ rispetto alla query $q$ fatta da un utente $u$?

**sorgenti di incertezza**:
* **utenti differenti** giudicano differentemente la rilevanza di $d$ rispetto a $q$
* **seconda del contesto**: un utente giudica differentemente $d$ rispetto a $q$ a seconda del contesto.
* **rappresentazione di** $d$: un utente giudica in base a come $d$ e' rappresentato. 
* **approssimazione del sistema ir**: per stimare la rilevanza di $d$ si fanno errori di approssimazione.

**probabilita**: voglio studiare $P(\text{relevant} | d,q) = \text{probabilita che il documento e' buono dati } d,q$.

**come lo calcolo la rilevanza**?
* **modelli probabilistici**: *binary independence model e bestmatch25.*
* **language model**: che si applica anche all'information retrieval.

**oss. su vector space model**: in questo modello, il documento piu simile potrebbe essere totalmente irrilevante per l'utente. (gemini ha senso???)

**formule varie**:
* $P(A,B) = P(A \cap B) = P(A|B)P(B) = P(B|A)P(A)$.
* $P(B) = P(A,B) \cdot P(\bar A, B)$.

**bayes e p. totali**:
$$
p(A|B) = \frac{p(B|A) p(A)}{p(B)} = \left[ \frac{p(B|A)}{p(B|A)p(A) + p(B| \bar A)p(\bar{ A})} \right] p(A)
$$
* **priori**: $P(A) = \text{priorita a priori}$
* **posteriori**: $P(A|B) = \text{probabilita a posteriori}$
* **ossia**: posso trovare la probabilità a posteriori di $A$ usando quella a priori.

**odds**: $O(A) = \frac{P(A)}{P(\bar{A})} = \frac{P(A)}{1-P(A)}$
* **ordinamento**: permette di ordinare i risultati, e' indicatore della rilevanza di un documento (??? gemini spiega meglio)
* se $O(A) > 1$ allora e' più probabile che il documento sia rilevante
* se $O(A) = 1$ allora non si sa
* se $O(A) < 1$ allora e' più probabile che il documento non rilevante. 

**rilevanza di un documento**: sia $p(R_{d,q})=p(R|d,q)$, usiamo **p. totali** per calcolare...
$$p(R|d,q) = \sum_{u\in U} p(R|d,q,u)p(u)$$
* $p(R|d,q) = \text{probabilita che il documento sia rilevante per il documento}$ dove $R_{d,q}$ e' una v.a. aleatoria.
	* $R_{d,q} = 1$ se il documento $d$ e' rilevante per $q$
	* $R_{d,q} = 0$ altrimenti
* $p(R|d,q,u)= \text{probabilita che l'utente } u \text{ giudica } d \text{ rilevante alla query } q$
* $p(u) = \text{ probabilita che l'utente } u \text{ e' chiamato a giudicare la rilevanza}$

**utenti**: nella formula non servono, omettere $p(u)$ non cambia l'ordinamento dei documenti. **Perche' si**. 
* gemini contestualizza questa cosa che ha detto er proff

**odds sulla rilevanza**. un documento e' rilevante se $p(R|d,q) > p(\bar{R}|d,q)$, ossia:
$$ O(R|d,q) = \frac{p(R|d,q)}{p(\bar{ R} | d,q)} > 1$$

**decomporre $p(R|d,q)$ usando bayes:** ci sono due modi
1. $p(R|d,q) = \frac{p(d|R,q)p(R|q)}{p(d|q)}$, ed $p(\bar{ R}| d,q)$ si ricava similmente
2. $p(R|d,q) = \frac{p(q|R,d)p(R|d)}{p(q|d)}$, ed $p(\bar{R}|d,q)$ si ricava similmente

**ranking**: faccio il ranking dei documenti osservando dunque $O(R|d,q)$ (odds).

**probabilistic ranking**: devo ordinare in base a $p(R_{d,q}) = p(R|d,q)$ e devo stimare questi valori. Calcolarlo e' complicato, devo fare delle **assunzioni**.

**assunzione**:
* **indipendenza**: assumiamo $d$ e $q$  indipendenti $\forall d,q$
* **uniformita documenti**: $\forall d,d': p(d) = p(d')$ (questa assunzione non vale per altri contesti, es: pagerank)
* **ignorare**: $p(R|q)$ puo essere ignorato. e' un valore costante per tutti i documenti.
$$
p(R|d,q) = \frac{p(d|R,q)p(R|q)}{p(d|q)} = \frac{p(d|R,q)p(R|q)}{p(d)}
$$
**definizioni**:
* $p(d|R,q) = \text{p. che } d \text{ venga scelto u.a.r dai documenti rilevanti per } q$.
* $p(R|q) = \text{p. che un documento scelto dalla collezione sia rilevante per } q$
* $p(d|q) = p(d) = \text{la probabilita che } d \text{ sia pescato dalla collezione}$, si assumono $d,q$  indipendenti $\forall d,q$.

## PRP e calcolo del rischio
**PRP (Probability Ranking Principle)**:  
> [!note] PRP (Probability Ranking Principle)
>  *If the retrieved documents (w.r.t a query) are ranked decreasingly on their probability of relevance, then the effectiveness of the system will be the best that is obtainable.*
* **come dicevano ad oxford**: *se i documenti sono ordinati in ordine decrscente in base alla probabilita di rilevanza, allora abbiamo ottenuto il miglior sistema di ir possibile*. **e grazie al cazzo**.
* gemini: spiega che vuol dire bene

**PRP esteso**:
>[!note] PRP esteso
> If *the IR* system’s response to each *query* is a ranking of the documents ... in order of decreasing probability of relevance to the *query*, where the probabilities are estimated as accurately as possible on the basis of whatever data have been made available to the system for this purpose, the overall effectiveness of the system to its user will be the best that is obtainable on the basis of those data

**Voglio calcolare il rischio**. siano:
* $C(d,q) = \text{ costo quando }d \text{ e' rilevante rispetto a } q \text{ e non e' stato recuperato}$
* $C'(d,q) = \text{ costo quando }d \text{ non e' rilevante rispetto a } q \text{ ed e' stato recuperato}$
* $D = D(q)$ insieme di documenti ritornati dal sistema.
* **allora il rischio** e' $R(D(q)) = \sum_{d\in D} C'(d,q)p(\bar{R}|d,q) + \sum_{d \not \in D} C(d,q) p(R|d,q)$
* **ossia**: $R(D(q)) = \sum_{d \in D} C'(d,q)(1-p(R|d,q)) + \sum_{d \not \in D} C(d,q) p(R|d,q)$

**A che serve il rischio**? si puo' usare come misura inversa rispetto alla qualita di un risultato.

**assunzioni sul rischio**:
* $D(q) = k$ e' n valore costante.
* $C,C'$ sono variabili binarie.

**calcolare il rischio**:
$$
R(D(q)) = \sum_{d \in D(q)}(1-p(R|d,q)) + \sum_{d \notin D(q) }p(R|d,q) = |D(q)| + \sum_{d \notin D(q)} p(R|d,q) - \sum_{d \in D(q)} p(R|d,q)
$$
* $\forall d\in D$ voglio ottenere la probabilita che $d$ **non venga restituito** come parte del risultato
* $\forall d\notin D$ voglio ottenere la probabilita che $d$ **venga restituito** come parte del risultato

**minimizzare** $R(D(q))$: vuol dire che $D(q)$ e' tale che da **massimizzare** 
$$
\star:\sum_{d \in D(q)} p(R|d,q) - \sum_{d \notin D(q)} p(R|d,q)
$$
**massimizzare** $\star$: vuol dire che $D(q)$ contiene i $k$ documenti con $p(R|d,q)$ piu alta.
* assumendo che $|D(q)| = k$

**Teorema**: assumendo 0/1 loss, allora **PRP** e' ottimo e minimizza gli errori, ossia il costo per gli errori.
* gemini vuol dire che con $C,C'$ variabili binarie allora prp e' ottimo?

## fare effettivamente i calcoli con BIM
**Binary Independence Model**: e' il modello piu' semplice. faccio le seguenti assunzioni
* **indipendenza**: le rilevanze dei documenti sono indipendenti l'une dalle altre.
* **rilevanza binaria**: il documento e' rilevante oppure no.
* il documento e' un **set di features** che costituiscono la **rappresentazione**: $d$ e' il vettore $(f_{1},\dots,f_{n})$ di features. allora consideriamo la probabilita che le feature $f_{1},\dots,f_{n}$ siano rilevanti rispetto a $q$:
$$
p(R|f_{1},\dots,f_n,q) = \frac{p(f_{1},\dots,fn| R,q)p(R|q)}{p(f_{1},\dots,f_n|q)} =_{rank} p(f_{1},\dots,f_{n}|R,q)
$$
* $=_{rank}$: gemini che vuol dire?
* **indipendenza feature**: $p(f_{1},\dots,f_{n}|R,q) = \prod_{i} p(f_{i}|R,q)$, dunque le features sono indipendenti tra loro $\forall \in [1,n]$.

**in BIM, ogni feture di un documento**:
1. e' associata ad un termine
2. e' binario: vale 1 se occorre nel documento, 0 altrimenti.
3. **il documento e'** $v_{d}= (x_{1},\dots,x_{m})$ con $x_{i} = 1$ se $t_{i} \in d$.
4. **la query e** '$v_{q} = (y_{1},\dots,y_{m})$ dove $y_{i} = 1$ se $t_{i} \in q$.
5. **collisioni**: *query e documenti differenti potrebbero essere rappresentati dallo stesso vettore*.

**rimodelliamo** $p(R|d,q)$ **usando i vettori**:
* $p(R|v_{d},v_{q}) = \frac{p(v_{d}|R,v_{q})p(R|v_{q})}{p(v_{d}|v_{q})} =_{rank} p(v_{d}|R,v_{q})$
* **similmente**: $p(\bar{R}|v_{d},v_{q}) = p(v_{d}| \bar{R}, v_{q})$
* $p(v_{d}|R,v_{q})$ e' la probabilita che se un documento rilevante e' stato selezionato, allora $v_{d}$ e' la sua rappresentazione.

**fare il ranking in BIM**: data la query $q$ faccio il ranking guardando $p(R|v_{d},v_{q})$ per ogni documento rispetto alla query. ossia:![[Pasted image 20260427174833.png]]
$$
O(R|v_{d},v_{q}) =_{rank} \prod_{t_{i}:x_{i}=y_{i}} \frac{p_{i}(1-u_{i})}{u_{i}(1-p_{i})}
$$

**split dei termini**: dato che $x_{i}$ puo' essere 0 o 1, allora:
$$\
O(R|v_{d},v_{q}) =_{rank} \prod_{t_{i}:x_{i}=1} \frac{p(x_{i}=1|R,v_{q})}{p(x_{i}=1| \bar{R}, v_{q})} \cdot \prod_{t_{i}:x_{i}=0} \frac{p(x_{i}=0|R,v_{q})}{p(x_{i}=0| \bar{R}, v_{q})}
$$

**RSV: passiamo ai logaritmi**:
1. $c_{i} = \log \frac{p_{i}(1-u_{i})}{u_{i}(1-p_{i})} = \log \frac{p_{i}}{1-p_{i}} - \log \frac{u_{i}}{1-u_{i}}$
2. $RSV_{d} = \sum_{t_{i}:x_{i}=y_{i}} \log_{} c_{i}$

**peso**: $c_{i}$ si comporta come un peso.

**dati da usare**:k* $N$ = numero di documenti totali
* $R$ = numero di documenti rilevanti segnalati da un'utente
* $r_{i}$ numero di documenti rilevanti in cui compare $t_{i}$
* $df_{t_{i}}$ numero di documenti che contengono $t_{i}$

altre formule...

> [!error] guarda esercizio su slide per esame

>[!warning] per martedi
> me se rompe la stampante un giorno. come faccio a dire la probabilita che si rompe di nuovo al giorno $x$? che distribuzione si usa? poisson


