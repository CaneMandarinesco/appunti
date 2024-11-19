* $n = p \cdot q \text{ con } p, q \text{ primi }$ 
* $e := \text{coprimo con } p \text{ e } q$
* $(M,n)=1$
* $d := 1\leq d < \phi(n) = (p-1)(q-1)$ con d inversa moltiplicativa di $e$ in modulo $\phi(n)$ 
--- 
per criptare il messaggio devo calcolare $M^e \text{ mod} \, n$
per decriptare il messaggio devo calcolare $M'^d \, \text{mod} \, n$
\
i valori che devono rimanere segreti sono: 
* $\phi(n)$
* $p$
* $q$
per quanto riguarda la creazione delle chiavi: una volta scelta la chiave pubblica determino quella privata (che e' inversa della pubblica)

#### perche' l'algoritmo funziona?
perche' 

### domande
* perche' e = 2 e' vietato?