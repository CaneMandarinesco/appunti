Calcolare cammini minimi su grafi pesati. Il costo di un cammino $\pi = <v_{0}, v_{1}, \dots, v_{k}>$ e':
$$
w(\pi) = \sum_{i=0}^{k} w(v_{i-1}, v_{i})
$$

Il cammino minimo non e' necessariamente **unico**. Il problema funziona anche per **costi negativi**.

>[!note] def.
> $d_{G}(u,v)$ e' il costo di un qualsiasi cammino minimo da $u$ a $v$.

Esiste sempre un cammino minimo fra due nodi? Dipende, non esiste:
* se non c'e no: $d_{G}(u,v)=+\infty$ 
* se un **ciclo** ha costo negativo: $d_{G}(u,v)=-\infty$
* se $G$ non ha cicli di lunghezza negativa allora esistono **cammini semplici**: senza ripetere nodi.

### sotto-cammino
ogni sotto-cammino di un cammino minimo e' un cammino minimo. Si dimostrazione per `cut&paste`.

### disuguaglianza triangolare
$$
d(u,v) \leq d(u,x) + d(x,v)
$$
grazie al cazzo.

### cammini minimi a sorgente singolare
Due varianti, una volta che ha risolto uno so fare l'altro:
* Dato $G=(V,E,w)$, $s\in V$, calcola le distanze di tutti i nodi da $s$.
* Dato $//$ calcola l'albero dei cammini minimi radicato in $s$.

### Albero dei cammini minimi (SPT)
$T$ e' un albero dei cammini minimi con sorgete $s$ di un grafo se:
* $T$ e' un albero radicato in $s$.
* per ogni $v \in V$, vale: $d_{T}(s,v) = d_{G}(s,v)$

Che e' equivalente ad un albero `BFS` radicato in $s$. Ora come si risolve questo problemaaaaa?????

### Dijkstra
Assunzione: tutti gli archi hanno peso non negativo, ovvero ogni arco $(u,v)$ ha peso $\geq 0$.

