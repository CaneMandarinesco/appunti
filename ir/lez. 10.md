**approccio probabilistico**: otterremo un sistema che performa bene tanto quanto l'approccio vettoriale utilizzando le probabilità.
* permette di ragionare su eventi incerti esprimendoli come probabilita.

**sorgenti di incertezza**:
* **utenti differenti** giudicano differentemente la rilevanza di $d$ rispetto a $q$
* un utente a **seconda del contesto** giudica differentemente $d$ rispetto a $q$.
* altro da slide...

**probabilita**: voglio studiare $P(\text{relevant} | d,q) = \text{probabilita che il documento e' buono dati } d,q$.
**modelli probabilistici**: *binary independence model e bestmatch25.*
**language model**: si applica anche all'information retrieval.

**formule varie**:
* $P(A,B) = P(A \cap B) = P(A|B)P(B) = P(B|A)P(A)$.
* $P(B) = P(A,B) \cdot P(\bar A, B)$.

**bayes e p. totali**:
$$
p(A|B) = \frac{P(B|A) P(A)}{P(B)} = \left[ \frac{P(B|A)}{\sum
} \right] P(A)
$$
* $P(A) = \text{priorita a priori}$
* $P(A|B) = \text{probabilita a posteriori}$

**odds**: $O(A) = \frac{P(A)}{P(\bar{A})} = \frac{P(A)}{1-P(A)}$
* **ordinamento**: permette di ordinare i risultati, e' indicatore della rilevanza di un documento (??? gemini spiega meglio)
* se $O(A) > 1$ allora e' più probabile che il documento sia rilevante
* se $O(A) = 1$ allora non si sa
* se $O(A) <$ allora e' più probabile che il documento non rilevante. 

$$p(R|d,q) = \sum_{u\in U} p(R|d,q,u)p(u)$$
* $P(R|d,q) = \text{probabilita che il documento sia rilevante per il documento}$
* $p(R|d,q,u)= \text{probabilita che l'utente } u \text{ giudica } d \text{ rilevante alla query } q$

**utenti**: nella formula non servono, omettere $p(u)$ non cambia l'ordinamento dei documenti. **Perche' si**.

**ranking**: faccio il ranking dei documenti osservando dunque $O(R|d,q)$ (odds).

**bayes su** $p(R|d,q)$: mi serve per calcolare tutte cose... **vedi sulle slide le formule**

**probabilistic ranking**: devo ordinare in base a $p(R_{d,q}) = p(R|d,q)$ e devo stimare questi valori. Devo fare delle **assunzioni**.

**assunzione**:
* **indipendenza**: assumiamo $d$ e $q$ .
* **uniformita documenti**: $\forall d,d': p(d) = p(d')$ (questa assunzione non vale per altri contesti, es: pagerank)
* **ignorare**: $p(R|q)$ puo essere ignorato (gemini ricordame pk).
$$
p(R|d,q) = \frac{p(d|R,q)p(R|q)}{p(d|q)} = \frac{p(d|R,q)p(R|q)}{p(d)}
$$

**PRP (Probability Ranking Principle)**:  sta sulle slide.

**Voglio calcolare il rischio**: 
* sia $C(d,q) = \text{ costo quando }d \text{ e' rilevante rispetto a } q \text{ e non e' stato recuperato}$
* sia $C'(d,q) = \text{ costo quando }d \text{ non e' rilevante rispetto a } q \text{ ed e' stato recuperato}$
* il rischio e' $R(D(q)) = \sum_{d\in D} \dots + \sum_{d \not \in D} \dots$
* vedi le slide le formule!

**assunzioni sul rischio**:
* $D(q) = k$ e' n valore costante.
* vedi slide...

**Teorema**: assumendo 0/1 loss, allora **PRP** e' ottimo e minimizza gli errori, ossia il costo per gli errori.

**Binary Independence Model**: e' il modello piu' semplice. faccio le seguenti assunzioni
* **indipendenza**: le rilevanze dei documenti sono indipendenti l'une dalle altre.
* **rilevanza binaria**: il documento e' rilevante oppure no.
* **rappresentazione binaria del termine**: il termine sta oppure non sta nel documento.
$$
p(R|f_{1},\dots,f_n,q) = \frac{p(f_{1},\dots,fn| R,q)p(R|q)}{p(f_{1},\dots,f_n|q)} 
$$
* **vettore**: il documento $d$ e' il vettore $(f_{1},\dots,f_{n})$
* vedi slide

**in BIM, ogni feture**:
1. e' associata ad un termine
2. e' binario
3. il documento e' $v_{d}= (x_{1},\dots,x_{m})$
4. vedi ste slide
5. vedi slide

tante formule per dire che:
$$
O(R|v_{d},v_{q}) = \prod_{t_{i}:x_{i}=y_{i}} \frac{p_{i}(1-u_{i})}{u_{i}(1-p_{i})}
$$
**RSV: passiamo ai logaritmi**:
1. $c_{i} = \log \frac{p_{i}(1-u_{i})}{u_{i}(1-p_{i})} = \log \frac{p_{i}}{1-p_{i}} - \log \frac{u_{i}}{1-u_{i}}$
2. $RSV_{d} = \sum_{t_{i}:x_{i}=y_{i}} \log_{} c_{i}$

**peso**: $c_{i}$ si comporta come un peso.

**dati da usare**:
* $N$ = numero di documenti totali
* $R$ = numero di documenti rilevanti segnalati da un'utente
* $r_{i}$ numero di documenti rilevanti in cui compare $t_{i}$
* $df_{t_{i}}$ numero di documenti che contengono $t_{i}$

altre formule...

> [!error] guarda esercizio su slide per esame

>[!warning] per martedi
> me se rompe la stampante un giorno. come faccio a dire la probabilita che si rompe di nuovo al giorno $x$? che distribuzione si usa? poisson


