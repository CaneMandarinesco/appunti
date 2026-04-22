
**analisi parametrizzata**: alcuni problemi sono difficili solo in certi casi.

**istanze difficili**: osserviamo che in molti problemi i casi difficili sono rari.

## clustering
**input**: dati $\mathcal X = \{ x_{1},\dots,x_{n} \}$ su **spazio metrico** $(\mathcal M, d)$ con $d$ **funzione di distanza**.
**output**: raggruppamento dei dati in clusters

## $k\text{-median clustering}$
**input**: dati $\mathcal X = \{ x_{1}, \dots, x_{n}\}$ ed $(\mathcal M, d)$
**Goal**: $S \subseteq \mathcal X$ di $k$ centri che minimizzano la distanza tra i punti dal centro più vicino.

**va bene come formulazione**?
* funziona solo quando c'e' un pattern da seguire
* quando i dati sono mischiati fa schifo, ma in questo caso non ci interessa fare il clustering.

**hardness**: $k\text{-media}$ clustering e' **NP-hard**. Ma posso trovare l'ottimo in *tempo polinomiale* se le istanze sono "semplici".

**perturbation stability**: $\gamma \geq 1$, una $\gamma \text{-perturbazione}$ di $(\mathcal M, d)$ e' $(\mathcal M, d')$ tale che:
$$
d(x,y) \leq d'(x,y) \leq \gamma d(x,y) \;\;\; \forall x,y \in \mathcal M
$$

* **un'istanza** e' $\gamma \text{-stabile}$ se esiste una soluzione $S^*=\{ c_{1}^*, \dots, c_{k}^* \}$ tale che per ogni $\gamma \text{-perturbazione}$, allora $S^*$ e' l'unica soluzione ottima.
* **intuitivamente**: vuol dire che per qualsiasi $\gamma$ i cluster sono ben separati. 

## Single-Link
* **step 1**: grafo completo. Vedi l'input come un grafo completo dove il peso degli archi e' dato da $d$.
* **step 2**: kruskal. Fai $k$ iterazioni di kruskal.

**hierarchical cluster tree**:
* **significato**: *ogni nodo interno del **HCT** rappresenta un'arco aggiunto da kruskal.*

$k\text{-pruning}$: voglio togliere da $H$ albero, $k$ nodi a partire dalla radice, in mod che se $x \in S$, allora $parent(x) \in S$.

## SingleLink++

**single-link++**: 
1. calcola hierarchical cluster tree di $G$ in input
2. calcola l'ottimo $k\text{-pruning}$ di $H$. Si fa con Programmazione Dinamica.
3. ritorna il cluster rappresentato da $H$.

**Teorema**: funziona per ogni $\gamma \text{-stable}$ $k\text{-median}$ instance con $\gamma \geq 3$

**Lemma**: per ogni $\epsilon > 0$, esiste un istanza $(3-\epsilon)\text{-stable}$ $k\text{-median}$   tale che $\text{SL++}$ non trova l'ottimo.

Sia $C_{1}^*, \dots, C_{k}^*$:
1. Pero ogni $i$, allora $C_{i}^*$ e' una componente connessa in qualche iterazione di kruskal.
2. Per ogni $i$, allora per ogni $A \subset C^*_{i}$,  se prendo un nodo in $\mathcal X \setminus A$, che minimizza la distanza da $A$, questo $x^*$ si trova in $C^*_{i}$

**Se 2 vera, allora vera 1**: difficile

**La 2 e' vera**: facile


## altre definizioni di stabilita
**approx stability**: un'istanza di $k\text{-median}$ e' $(c,\epsilon)\text{-approzimate}$ se  ogni $c$ **approssimazione** e' $\epsilon$ **accurata** se ...

