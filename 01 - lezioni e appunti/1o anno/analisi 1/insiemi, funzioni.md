
# insiemi numerici
* numeri naturali: $\mathbb{N} = \{ 0,1,2,3,4,\dots \}$
* numeri interi: $\mathbb{Z} = \{0,1,-1,2,-2\}$
* numeri razionali: $\mathbb{Q} = \left\{  \frac{m}{n}: m,n \in \mathbb{Z} \text{ e } n \neq 0  \right\}$
	* ossia tutte le frazioni tra numeri interi
* numeri reali: 
$$
\mathbb R = \{ n.c_{1}c_{2}c_{3}\dots: n \in \mathbb Z, c_{i} \in \{ 0,1,2,3,\dots,9 \} \forall i\geq 1 \}
$$
sono composti quindi da un **intero**, e da una sequenza di **numeri decimali**

>[!note] OSS. 
>i **reali** sono in corrispondenza biunivoca con la retta **orientata** (assimoa di continuita')

### teorema: $\sqrt{ 2 } \not\in \mathbb{Q}$
dimostriamo che $\sqrt{ 2 }$ (ovvero quel numero che elevato alla seconda fa 2), non e' razionale, quindi non si può esprimere **sotto forma di frazione**.
**per assurdo**: se $\sqrt{ 2 } \in Q$, allora vuol dire che esistono: $n \in \mathbb{Z} \text{ e } m \in \mathbb{Z}$ o meglio, dato che $\sqrt{ 2 }$ e' un numero positivo: $n \in \mathbb N^+ \text{ e } m \in \mathbb N^+$ tali che: $\frac{m}{n} = \sqrt{ 2 }$.
di conseguenza, moltiplicando entrambi i termini per n, abbiamo che: 
$$
m = \sqrt{ 2 }n
$$
ora eleviamo tutto al quadrato:
$$
m^2 = 2n^2
$$
per qualsiasi numero che scegliamo per $m \text{ e } n$, ci accorgiamo che:
* $m^2$ e' un numero con un numero **pari** di fattori 2
* $2n^2$ e' un numero con un numero **dispari** di fattori 2
ciò' contraddice l'unicità' della fattorizzazione in fattori primi.
più' semplicemente potremmo dire che: $m^2$ si scompone in modo diverso rispetto a $2n^2$, quindi non possono essere uguali

>[!note] OSS.
>$\sqrt{ 2 }$ e' un numero **illimitato**, e non periodico. se lo fosse, si potrebbe scrivere come frazione.

## proprieta' di $\mathbb R$
* **associativa**: cambiare le parentesi ($(a+b)+c = a+(b+c)$)
* **commutativa**: cambiare l'ordine degli addendi
* **elemento neutro**: 
	* somma: $a+0 = a$, $0$ e' l'elemento neutro
	* prodotto: $a * 1 = a$, $1$ e' l'elemento neutro
* **opposto**: $a+b=0$, allora $a = -b$
* **reciproco**: $a * \frac{1}{a} = 1$ dove $\frac{1}{a}$ e' il reciproco di $a$

## intervalli
easy

# funzioni
$$
f: A\to B
$$
associa ad ogni $x \in A$ un elemento $f(x) \in B$.
il grafico di $f$ e' rappresentato dall'insieme: 
$$
\{ (x,f(x)): x \in A \}
$$
> ovvero un insieme di **coppie ordinate** ($\subseteq R \times R$)

* iniettiva
* suriettiva
* biettiva
* $f$ e' invertibile se e solo se e' **biettiva**: $f^{-1}: B \to A$
