nella lez. 5 abbiamo definito $\Omega$ e $\mathcal A$. Ora ci occupiamo di $P$, per i due casi:
1. $n$ prove indipendenti con ognuna probabilità $p$.
2. $n$ estrazioni casuali di un oggetto alla volta, senza reinserimento, da un insieme di $n_{1}$ oggetti di tipo 1 (**successo**) e $n_{2}$ oggetti di tipo 2 (**fallimento**).

Richiamo lezione 5: per trovare la densita discreta di $X$:
$$
\forall k \in L_{X} = \{ 0,1,\dots,n \} \,\,\, P_{X}(k) = \sum_{w:X(w)=k} P(\{ w \})
$$
vedremo che, nei casi 1 e 2:
$$
X(w) = X(w') \to P(\{ w \}) = P(\{ w' \})
$$
E' vero che:
$$
P_{X}(k) = \sum_{w: X(w)=k}P(\{ w \}) = \sum_{w:X(w)=k} q_{k} = q_{k}+\dots+q_{k} = r_{n,k} \cdot q_{k}
$$

con: $r_{n,k} = \binom{n}{k}:= \#\{ w: X(w) = k \}$. Dato che abbiamo definito nella lez. 5 $X(w)=w_{0}+\dots+w_{n}$. $X(w)=k$ vuol dire che ho $k$ eventi verificati.


### Caso 1: distribuzione binomiale
v.a. che conta successi su $n$ prove indipendenti, con probabilità di successo $p$ in ogni prova.

consideriamo il caso $n=3$:
* $P(\{ 0,0,0 \}) = (1-p)^3$
* $P(\{ 1,0,0 \}) = p(1-p)(1-p)$
* $P(\{ 0,1,0 \}) = (1-p)p(1-p)$
* ...
* $P(\{ 0,1,1 \} = (1-p)\cdot p\cdot p$
* $P(\{ 1,1,1 \}) = p^3$

ne consegue che: 
* $X(w)=0 \to P(\{ w \}) = (1-p)^3$
* $X(w) = 1 \to P(\{ w \}) = p(1-p)^2$
* $X(w) = 2 \to P(\{ w \}) = p^2 \cdot (1-p)$
* $X(w)=3 \to P(\{ w \} = p^3)$

o in generale:
$$
P(\{ w \}) = p^{X(w)} (1-p)^{n-X(w)}
$$
e quindi per ogni $X(w)=k$:
$$
P(\{ w \}) = p^k (1-p)^{n-k}
$$
In riferimento alla formula di prima:

> [!note] Formula densità discreta della v.a. con distribuzione di Bernoulli
> $$
P_{X}(k) = \binom{n}{k} p^k (1-p)^{n-k}
$$

dove: 
* $n$ e' il **numero di prove indipendenti**
* $p$ e' la probabilità di successo di ogni prova.

oss:
* $\sum_{k=0}^n P_{X}(k)=1$
* per $p=\frac{1}{2}$ si ha $P_{X}(k)=\binom{n}{k} \left( \frac{1}{2} \right)^n$
* per $p=0$ $P_{X}(0) = 1$ e $P_{X}(x)=0$ per $x \neq 0$
* per $p=1$ $P_{X}(n)=1$ e $P_{X}(x)=0$ per $x \neq n$

### Caso 2: Distribuzione Ipergeometrica
Supponiamo di avere $n_{1}$ oggetti di "tipo 1" e $n_{2}$ oggetti di "tipo 2". Estraggo a caso $n$ oggetti con $n <n_{1} + n_{2}$, uno alla volta senza reinserire. Ho successo se estraggo tipo 1 altrimenti ho un fallimento. 

Considero $w = (1,\dots ,1,0,\dots,0)$ dove ho $k$ volte 1 e $n-k$ volte 0

$$
P(\{ w \}) = \frac{n_{1}}{n_{1}+n_{2}} \cdot \frac{n_{1}-1}{n_{1}+n_{2}-1} \cdot \dots \cdot \frac{n_{1}-(k-1)}{n_{1}+n_{2}-(k-1)} \cdot
$$
$$
\cdot \frac{n_{2}}{n_{1} +n_{2} -k} \cdot \frac{n_{2}-1}{n_{1}+n_{2}-k-1} \cdot\dots \cdot \frac{n_{2}-(n-k)}{n_{1}+n_{2}-(n-1)}
$$
ogni volta che estraggo cambiano le probabilita.

in conclusione:
> [!note] densità discreta della v.a con distribuzione ipergeometrica
>$$
P_{X}(k) = \frac{\binom{n_{1}}{k} \binom{n_{2}}{n-k}}{\binom{n_{1}+n_{2}}{n}}
$$

dove $n < n_{1} + n_{2}$. 

## esempi
### esempio
Un'urna ha 500 palline **bianche** e 500 palline **nere**. Estraggo 3 palline alla volta, senza reinserimento. Trovare la densità della v.a. $X$ che conta il numero di palline bianche estratte.

Ricado nel caso della distribuzione ipergeometrica dato che ho $n_{1}$ oggetti di tipo 1 e $n_{2}$ oggetti di tipo 2.

$$
P_{X}(k) = \frac{\binom{500}{k} \binom{500}{3-k}}{\binom{1000}{3}} = 
$$