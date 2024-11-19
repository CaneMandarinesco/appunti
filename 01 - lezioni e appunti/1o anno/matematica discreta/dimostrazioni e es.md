### es 1
Quante relazioni di equiv. ci sono su $a = [3] (= \{ 1,2,3 \})$?

Sia $R$ una relazione di equiv. su $[3]$, allora:
$$
R \subseteq [3]\times[3] = \{ (1,1), (1,2), (1,3), (2,1), (2,2),(2,3), (3,1), (3,2), (3,3)  \}
$$
> dato che $R$ e' di **equivalenza**, quindi deve essere: riflessiva, transitiva, e ogni elemento deve essere in relazione con se stesso

una prima  relazione di equivalenza e'
$$
\{(1,1), (2,2),(3,3)\} \subseteq R
$$
ora provo ad aggiungere altri elementi a questa relazione di equivalenza. dato che $\exists (c,d) \in R \text{ con } c \neq d$
per esempio considero:
$$
 \{ (1,1), (2,2), (3,3),(1,2),(2,1) \}\subsetneq R
$$
allora $\exists (c,d) \in R$ che e' diverso da quelli nell'insieme considerato.
sia per esempio: $(c,d)=(2,3)$ allora $(3,2) \in R$ e $(1,3) \in R$ e $(3,1) \in R$ allora:
$$
R = [3] \times [3]
$$
e' di equivalenza.

quindi ho trovato 3 relazioni di equivalenza in $A$:
1. $\{(1,1),(2,2),(3,3)\}$
2. $\{ (1,1), (2,2), (3,3),(1,2),(2,1) \}$
3. $\{ (1,1), (1,2), (1,3), (2,1), (2,2),(2,3), (3,1), (3,2), (3,3)  \}$

inoltre modificando la 2 ho che
1. $\{ (1,1), (2,2), (3,3),(2,3),(3,2) \}$
2. $\{ (1,1,), (2,2), (3,3), (1,3), (3,1) \}$
dove il $(3)$ l'ho ricavato dalla $(2)$

#nonhocapito
un'altro metodo più immediato e' considerare le partizioni di $A$:
* $\{ \{ 1,2,3 \} \}$
* $\{ \{ 1,2 \}, \{ 3 \} \}$
* $\{ \{ 1,3 \}, \{ 2 \} \}$
* $\{ \{ 1 \}, \{ 2,3 \} \}$ 
* $\{ \{ 1 \}, \{ 2 \}, \{ 3 \} \}$

quindi le relazioni di equivalenza su $A$ sono sicuramente $\leq 5$
dato che sono tutti sotto insiemi di $A \times A$ allora sono tutte 5 relazioni di eq


## dimostrazioni
* ragionamento diretto
* induzione
ba.