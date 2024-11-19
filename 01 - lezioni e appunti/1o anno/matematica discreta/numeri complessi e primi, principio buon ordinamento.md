
>[!nota] OSS.
> formalmente in numeri razionali sono classi di equivalenza di $\mathbb{Z} \times (\mathbb Z \backslash \{ 0 \})$ rispetto alla relazione di equivalenza studiata in un esercizio precedente (???) #nonhocapito

### complessi
$$
\mathbb C = \{ \alpha+i\beta: \alpha,\beta \in \mathbb R \} \text{ e } i =  \sqrt{ -1 }
$$
come in analisi 1: [[numeri complessi]]

>[!note] NOTA.
>i numeri complessi si dividono razionalizzando

>[!note] OSS.
>$|z| = z * \overline {z}$
# numeri primi
siano $a,b \in \mathbb P$:

>[!note] DEF.
>si dice che $a$ **divide** $b$ (o che $b$ e' **multiplo** di $a$), scritto $a | b$ se: $$\exists k \in \mathbb Z: b = k * a$$

**osservazioni**:
* se $a|b$ allora $a \leq b$
* se $a|b$ e $b|c$ allora $a|c$
* $a|b$ e $a|c$ allora $a|(xb+yc)$ per ogni $x,y \in \mathbb Z$

> [!note] DEF.
> $a$ e' un **numero primo** se: 
> $$
> b|a \to b=1 \text{ o } b=a, \text{ e } a\geq 2
> $$
> altrimenti e' **composto**

> [!note] DEF.
> due numeri $a \text{ e } b$ si dicono **coprimi** se:
> - $c|a$
> - $c|b$
> 
> e $c=\pm 1$

### teo. 3.6.1
Sia $n \in \mathbb P, n\geq 2$, allora $n$ e' **prodotto di numeri primi**
#### DIM.
passo base:
$$
\text{ per } n = 2 \to 2=2
$$
passo induttivo, se $n+1$ e' primo allora
$$
n+1 = n+1 \to \text{ quindi n+1 e' primo}
$$
se $n+1$ non e' primo allora:
$$
\exists a,b \in \mathbb P: 1<a < n+1, \;\;\; 1<b<n+1 \text{ e } n+1=a*b
$$
ma dato che $a\leq n \text{ e } b\leq n$ per **induzione** il teorema vale per $a$ e $b$, quindi $n+1$ in ogni caso e' prodotto di numeri primi

riassumendo abbiamo dimostrato che se $n+1$ e' un numero primo, allora al massimo e' prodotto di se stesso, altrimenti dato che per induzione, supponiamo che il teorema sia vero $\forall m \in \mathbb P: m<n$, essendo $a \text{ e }b$ sicuramente numeri minori di $n+1$ allora per induzione supponiamo che siano primi, e quindi $n+1$ e' prodotto di due primi.

---
>[!note] DEF.
> $n$ si dice perfetto se $n$ e' uguale alla somma dei suoi divisori
> - $6 = 1+2+3$

> [!note] OSS.
> se $p$ e' primo -> non puo' essere perfetto
> problema aperto: esistono **numeri perfetti dispari**?

### teo 3.6.2
Ci sono infiniti numeri primi.
#### DIM.
per assurdo sono finiti: $\{ p_{1},p_{2},p_{3},\dots,p_{n} \}$, e definiamo: $N = p_{1} \cdot p_{2} \cdot p_{3} \cdot \dots \cdot p_{n}$
per il teorema 3.6.1 (p e' prodotto di primi), abbiamo che $N$ e' prodotto di primi. allora abbiamo che: $q \text{ t.c } q|N$. quindi $q|(p_{1} \cdot p_{2} \dots p_{n}+1)$ e quindi $q$ non puo' essere nessuno degli elementi in $N$.
Se per assurdo: $q=p_{1}$ allora $q|N$ e $q|p_{1}\dots N$. allora $q|1$ e quindi $q\leq1$ che e' assurdo perche' q e' primo, similmente per $q=p_{2}$ e cosi via...
$q$ non e' un elemento in $N$ quindi ci sono quindi infiniti numeri primi

### quanto sono frequenti i numeri primi?
$$
\pi (n)= |\{m\leq n: m \text{ e' primo}  \} |
$$
dove con $|A| = \text{ numero di elementi di A}$

### teo
$$
\lim_{ n \to \infty } \frac{\pi (n)}{\frac{n}{\ln(n)}} = 1
$$
 
# principio del buon ordinamento (Well Ordering Principle)
> [!note] DEF. 
> ogni insieme costituito da interi **non negativi**, contiene al suo interno un elemento che e' il più piccolo tra tutti

> [!note] OSS.
> non vale per i numeri razionali, non hanno un minimo

> [!note] OSS.
> se vale WOP, allora vale anche al dimostrazione per induzione e viceversa.