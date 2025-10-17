Dato $U$, mantenere in memoria $S \subseteq U$.
* `make-dict`
* `look-up`
* `insert`
* `delete`

Con Hash Table: $O(|S|)$ per memoria e $O(1)$ ammortizzato in totale.
funzione hash: $h: U \to {1, ..., m-1}$.
**Collisione**: $h(u) = h(v) \land u \neq v$.
Idea: **Randomizzazione**, funzioni deterministiche potrebbero far corrispondere a piu' oggetti lo stesso hash. Pero' dovrei ricordare il valore.

> $Pr _{h()} \leq 1/m$ -> allora Halef e' insieme di funzioni universale.

Come creo Halef?
* $n \leq m \leq 2n$.
* Identifico ogni elemento in base $m$.
* $h_{a}(x) = \sum\limits_{i=1}^{r} a_{i}x_{i} mod \; m$ dove $a \in U$ e $x \in U$
