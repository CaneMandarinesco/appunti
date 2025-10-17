> [!note] Riduzione Polinomiale: $X \leq_P Y$
> Vuol dire che il problema $X$ può essere risolto facendo un numero polinomiale di chiamate alla black-box che risolve $Y$, e quindi che $X$ e' almeno difficile quanto $Y$. Se so risolvere $Y$ so risolvere anche $X$.

Siano $X,Y$ tali che: $X \leq_P Y$, e:
* se $X$ risolvibile in tempo polinomiale allora anche $Y$ lo e'.
* se $Y$ non risolvibile in tempo polinomiale allora anche $X$ non lo e'.
* se $Y \leq_P X$, allora $X \equiv_P Y$, ossia **equivalenti**.

E' possibile dimostrare che due problemi sono equivalenti trasformando l'istanza $X$, in un'istanza per $Y$ in tempo polinomiale, e si esegue poi l'algoritmo che risolve $Y$.

# `Independent Set` e `Vertex Cover`
>[!note] Independent Set
> **Input**: grafo $G$ e intero $k \geq 0$.
> **Output**: sottoinsieme di nodi di $G$ di grandezza **almeno** $k$ tale per cui questi non condividono nodi.

>[!note] Vertex Cover
>**Input**: grafo $G$ e intero $k \geq 0$.
>**Output**: sottoinsieme di nodi di $G$ di grandezza al **massimo** $k$ tali che questi tocchino tutti gli archi di $G$.

## $\text{INDEPENDENT-SET} \equiv_P \text{VERTEX-COVER}$

Dimostreremo che: $S \subseteq V$ e' Independent Set $\iff$ $V \setminus S$ e' Independent Set.
**Punto 1**: Se $S$ e' Independent Set allora ci sono due tipi di archi in $G$: $(v,u) \text{ t.c. } v \notin S \text{ o } u \notin S$ (o eventualmente entrambe), allora $v \in V \setminus S \text{ o } u \in V \setminus S$.
**Punto 2**: Se $V \setminus S$ e' Vertex Cover analogamente a prima: $(v,u) \text { t.c. } v \in V \setminus S \text{ o } u \in V \setminus S$, allora $S$ e' Independent Set!

---
# $\text{VERTEX-COVER} \leq_P \text{SET-COVER}$
> [!note] Set Cover
> **Input**: collezione di sottoinsiemi di un insieme universo $U$ e $k$ intero.
> **Output**: selezionare almeno $k$ sottoinsiemi di $U$ tali che la loro unione sia $U$.

Data un'istanza di Vertex Cover posso costruire $U = {e \in E}$, e definire i sottoinsiemi di $U$ come $U_i = \{ v_i \in E \text{ t.c. } \forall u \in V (v_i,u) \in E  \}$. Se eseguo la black box per Set Cover su questa istanza allora  sto trovando i nodi (in set cover $U_i$) che uniti sono toccati da tutti gli archi (in set cover gli elementi negli $U_i$).

> Nota: con Set Cover vogliamo trovare insieme il più diversi possibili in modo da coprire tutto $U$, come in Vertex Cover

## $\text{3SAT} \leq_P \text{VERTEX-COVER}$
> [!note] SAT
> **Input**: G = (V, E) contains a vertex cover of size k iff (U, S, k) contains
a set cover of size k.t: una formula `CNF` ossia fatta cosi: $(x_1 \lor x_2 \lor x_3) \land (...) \land (x_1 \lor x_2 \lor x_3)$ composta solo dai literali $x_1, x_2, x_3$ che possono anche comparire negati.
> **Output**: verificare se la formula e' **soddisfacibile**.

Per ridurre `3SAT` a Vertex Cover 