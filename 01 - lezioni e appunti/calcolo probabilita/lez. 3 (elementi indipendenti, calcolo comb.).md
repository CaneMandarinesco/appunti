## elementi indipendenti
famiglia di elementi indipendenti:
* $P(A_{1} \cap A_{2}) = P(A_{1})P(A_{2})$ vale in generale per ogni $A_{k}$

### esempio
Se per esempio ho le carte: $1,2,3, 123$ e considero $A_{i} = \{  \text{ la carta estratta ha il numero } i \}$ allora $P(A_{1})=P(A_{2})=P(A_{3})=\frac{1}{2}$
1. $P(A_{1} \cap A_{2} \cap A_{3}) = \frac{1}{4} \neq \frac{1}{8 } = P(A_{1}) P(A_{2}) P(A_{3})$, ovvero $123$
2. $P(A_{1} \cap A_{2}) = P(\dots) = P(A_{2} \cap A_{3}) = \frac{1}{4}$ 

quindi sono eventi non indipendenti. ma lo sono 2 a 2.

### esempio
Si lanciano 3 monete eque e considero gli eventi:
* $A = \{  \text{ esce testa al 1o lancio} \}$
* $B = \{ \text{escono 2 teste consecutive} \}$
Allora l'insieme di riferimento e': $\Omega = \{  (T,T,T), (T,T,C), \dots \}$ che ha cardinalita' $2^3=8$ $\to$ ogni elemento di $\Omega$ ha probabilità $\frac{1}{8}$.
* $P(A) = \frac{1}{2}$
* $P(B) = \frac{1}{2}$
* $P(A \cap B) = \frac{1}{8}$ e $P(A)P(B)=\frac{1}{8}$ $\to$ quindi e' $\{ A,B \}$ e' famiglia di eventi indipendenti
## Calcolo combinatorio
>[!note] DISPOSIZIONI SEMPLICI
> sono semplici perché' non ci sono ripetizioni e considero  l'**ordine**
>$\{D_{n,k} = \{ i_{1},..,i_{n} \} \,\, \text{sequenze ordinate di elementi in } \{1,\dots,n\} \text{ senza ripetizioni, di lunghezza }k\}$ 

ovvero: 
$$
n(n-1)\dots(n-k+1) = \frac{n!}{(n-k)!}
$$
se $k=n$ allora le disposizioni semplici sono: $n!$

>[!note] oss
se $C_{n,0} = \text{insieme vuoto}$ o $C_{n,n} = \{ 1,\dots ,n \}$allora la sua cardinalita' e' $1$. 

### esempio
$n = 4, k=2$ allora ho che: $C_{n,k} = \{ \{ 1,2 \}, \{ 1,3 \},\dots, \{ 3,4 \} \}$, dove $\{ 1,2 \} = (1,2) = (2,1)$, $\{ 1,3 \} = (1,3) = (3,1)$ e cosi via...
Quindi per ogni elemento di $C_{n,k}$ ho 2 coppie quindi: 
$$k \cdot |C_{n,k}| = 2 \cdot 6 = 12 = \frac{4!}{(4-2)!}$$
### esempio
sia $\{ 1,\dots,n \}$, e $\{ i_{1},\dots, 1_{k} \} \in$ $C_{n,k}$, permutando gli elementi di $i_{1}, \dots, i_{k}$ trovo $k!$ elementi di $D_{n,k}$.

$$
D_{4,3} = \{ (1,2,3), (1,2,4), (1,3,4), (2,3,4) \}
$$
dove considero ogni permutazione di ogni elemento, per esempio $(1,2,3) = (1,3,2) = \text{ ecc...}$. e quindi $\#D_{4,3} = 24$ perche' ho 4 elementi e ognuno ha 6 permutazioni. 

### esempio: disposizioni con ripetizioni
ho $n_{1}$ oggetti di tipo 1 e $n_{2}$ oggetti di tipo 2. *Quanto vale la possibilità di pescare k oggetti di tipo 1?* ($n-k$ sono gli oggetti di tipo 2)

$P_{k} = 0 \text{ se } k>n_{1} \text{ o se } n-k>n_{2}$ altrimenti va calcolato! Generalmente ho $\binom{n_{1}+n_{2}} {n}$. Per $n_{1}$ ho $\binom {n_{1}}{k}$ e per $n_{2}$ ho $\binom{n_{2}} {n-k}$. 

$$
P_{k} = \frac{\binom {n_{1}}{k} \binom {n_{2}}{n-k}}{\binom {n_{1} + n_{2}}{n}}
$$

#### esempio
$n_{1}=3, n_{2}=2, n_{1}+n_{2} = 5$ e ho $\{ 1,2,3,4,5 \}$ con $\{ 1,2,3 \}$ di tipo 1 e $\{ 4,5 \}$ di tipo 2. 

$P_{k}$ con $k=1$? e $P(1 \text{ oggetto tipo } 1 \text{ e } 2 \text{ oggetti di tipo } 2)$?

$$P_{1} = \frac{\binom{3} {1}\binom{2} {2}}{ \binom {5}{3}} = \frac{3}{10}$$
in generale:
$$
P_{k_{1}, \dots, k_{r}} = \frac{\binom{n_{1}}{k_{1}} \dots \binom {n_{r}}{k_{r}}}{\binom {n_{1}+\dots+n_{r}}{n}}
$$

## Esercizi di riepilogo
### esercizio 1
Un'urna ha 3 palline bianche e 4 nere. si estraggono 2 palline a caso, una alla volta e con **reinserimento**.

1. calcolare la probabilità di estrarre due colori uguali
2. calcolare la probabilita' di estrarre almeno una pallina nera

**svolgimento**: $B_{k} = \{ \text{k-esima estratta bianca}  \}$ e $B_{k} = \{ \text{k-esima estratta nera}\}$, uno e' il complementare dell'altro.

1) la probabilita e' $P((B_{1} \cap B_{2}) \cup(N_{1} \cap N_{2})) = P(B_{1} \cap B_{2}) + P(N_{1} \cap N_{2}) = P(B_{1})P(B_{2}) + P(N_{1})P(N_{2})$. ovvero $\frac{25}{49}$. Dato che reinserisco, allora $B_{1} = B_{2}$ ecc...
2) $P(N_{1} \cup N_{2}) = P(N_{1}) + P(N_{2}) - P(N_{1} \cap N_{2}) = \frac{40}{49}$ oppure $=1-P(N_{1}^C \cap N_{2}^C) = 1-P(B_{1} \cap B_{2}) = \frac{40}{49}$

