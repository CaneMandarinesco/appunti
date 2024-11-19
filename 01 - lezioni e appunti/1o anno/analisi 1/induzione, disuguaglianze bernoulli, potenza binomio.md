# principio induzione

se:
* $P(n_{0})$ e' vera
* $\forall n \geq n_{0}$, $P(n) \to Pn(n+1)$
allora $P(n)$ e' vera $\forall n \geq n_{0}$

### es 1
$$
1+2+\dots+n = \sum_{k=1}^n k = \frac{n(n+1)}{2} \; \;P(n)
$$
verifichiamo per induzione (le due equazioni a destra): per $n\geq 1$
#### passo base
$$
P(1) \to \sum_{k=1}^1 k = \frac{1(1+1)}{2} \to 1=1 \to \text{ e' vera!}
$$

#### passo induttivo
$$
\sum_{k=1}^nk = \frac{n(n+1)}{2} \to \sum_{k=1}^{n+1}k=\frac{(n+1)((n+1) +1)}{2}
$$
quindi dimostriamo che se vale per $n$, vale anche per $n+1$. come si fa in pratica? si fa un **mix** tra ipotesi induttiva (ovvero $P(n)$) e $P(n+1)$.

noi possiamo dire che: 
$$
(1)\to\;\;\;\;\; \sum_{k=1}^{n+1} k = \sum_{k=1}^{n}k + (n+1) \;\;\;\;\; \leftarrow  (2)
$$
cioe': nella $(2)$ ho spezzato la sommatoria in due parti, separando il passo $n+1$, dove $k=n+1$ dalla sommatoria, che ora termina ad $n$, non piu' ad $n+1$ (proprio perche' ho separato il passo $n+1$!)

infatti per esempio:
$$
\sum_{k=1}^{n=5} n = 1+2+3+4+5 = (1+2+3+4)+5 = (\sum_{k=1}^{n=4}k )+ 5 
$$

dove $n=5$ e' il nostro $n+1$ nell'esempio precedente

quindi riprendendo la dimostrazione, dato che per **ipotesi** $\sum_{k=1}^n k = \frac{n(n+1)}{2}$ ho che:
$$
(3) \;\;\; \sum_{k=1}^{n}k + (n+1) = \frac{n(n+1)}{2} + (n+1) = \frac{(n(n+1)) + 2(n+1)}{2} = \frac{(n+1)(n+2)}{2}
$$
riprendendo l'**ipotesi** invece abbiamo che:

$$
(4) \;\;\;\; \sum_{k=1}^{n+1} k = \frac{(n+1)((n+1)+1)}{2} = \frac{(n+1)(n+2)}{2}
$$
notiamo che la $(3)$ e la $(4)$ sono **uguali** quindi abbiamo concluso la dimostrazione.

# disuguaglianza di bernoulli
**ipotesi**:
$$
\forall n \in \mathbb{N^+}, \forall x \geq -1 \to (1+x)^n \geq 1+nx
$$
si dimostra per induzione:
1. passo base: **e' vero**
2. passo induttivo:
dobbiamo verificare che:
$$
(1+x)^{n+1} \geq 1+ (n+1)x
$$
in questi casi conviene evidenziare nella nostra espressione (sempre utilizzando l'ipotesi base per $n$) il fatto che l'espressione a sinistra abbia un qualche termine che renda vera la diseguaglianza.  
si procede dunque cosi:
$$
(1+x)^{n+1} = (1+x)^n (1+x)^1
$$
> nota: conviene sempre se possibile separare il termine $n+1$

ma $(1+x)^n$ sappiamo per **ipotesi** essere $\geq 1+nx$ quindi moltiplicando entrambi i membri dell'**ipotesi** per $(1+x)$ abbiamo che:

$$
(1+x)^{n+1} = (1+x)^n (1+x)^1 \geq (1+nx)(1+x)
$$
dato che per **ipotesi** e' $(1+x)^{n} \geq (1+nx)$, e dato che abbiamo **moltiplicato i due membri per la stessa quantità** ($(1+x)$), l'uguaglianza e' sempre vera.
ora per dimostrare per induzione basta far vedere che:
$$
(1) \;\;\;\; (1+nx)(1+x) \geq 1 + (n+1)x
$$
dove: $1+(n+1)x$ viene da: $(1+x)^{n+1} \geq 1+(n+1)x$

abbiamo quindi, sviluppando, che la $(1)$ equivale a:
$$
1+x+nx+nx^2 \geq 1+x+nx
$$
ad occhio ci si accorge che le quantità' a sinistra e a destra differisco solo per $nx^2$, quindi si può concludere che la disuguaglianza e' vera e che la disuguaglianza di bernoulli e' stata dimostrata.

# coefficiente e fattoriale, binomio di newton
def di fattoriale:
$$
n! =  \left \{ \begin{array}{x}
1 \text{ se  n=0} \\ \\
n(n-1)(n-2)\dots 1 \text{ se } n\geq 1
\end{array}\right \}
$$
serve per introdurre il coefficiente binomiale:
$$
\binom{n}{k} = \frac{n!}{k!(n-k)!} = \frac{n(n-1)(n-1)\dots (n-k+1)}{k!}
$$

> da ricordare a memoria

ha una rappresentazione grafica:
![[Pasted image 20240215165500.png]]

### proprieta;
1. $\binom{n}{k} = \frac{n!}{k!(n-k)!} = \frac{n!}{(n-k)!k!} = \binom{n}{n-k}$
2. $\binom{n}{k-1} + \binom{n}{k} = \frac{n!}{(k-1)!(n-k-1)!} + \frac{n!}{k!(n-k)} = \frac{n!}{k!(n-k+1)!}(k+(n-k+1)) = \binom{n+1}{k}$

# binomio di newton

$$
\forall a,b \in \mathbb R, \forall n \in \mathbb N^+ (a+b)^n = \sum_{k=0}^n \binom{n}{k} a^kb^{n-k}
$$
anche questo per induzione:
1. passo base: vero
2. passo induttivo:

$$
(a+b)^{n+1} = (a+b)^n(a+b) = (a+b) \sum_{k=0}^n \binom{n}{k}a^k b^{n-k}
$$
ora la sommatoria moltiplica $a$ e $b$ quindi ho:
$$
= \left( a\sum_{k=0}^n \binom{n}{k}a^k b^{n-k} \right) + \left( b\sum_{k=0}^n \binom{n}{k}a^k b^{n-k} \right)
$$
portando i termini dentro la sommatoria ho che:
$$
= \sum_{k=0}^n \binom{n}{k}a^{k+1} b^{n-k} + \sum_{k=0}^n \binom{n}{k}a^k b^{n+1-k}
$$
> ponete attenzione a: $a^{k+1}$ e $b^{n+1-k}$

in una dimostrazione conviene sempre fare in modo che i termini siano il più possibile simili tra loro, quindi modificando i parametri della sommatoria a sinistra ho che:

$$
= \sum_{k=1}^{n+1} \binom{n}{k-1} a^k b^{n+1-k} +\sum_{k=0}^n \binom{n}{k} a^k b^{n+1-k}
$$
spostando fuori i termini: $a^{n+1}$ e $b^{n+1}$ dalle due sommatorie le posso sommare:
$$
= a^{n+1} + \sum_{k=1}^{n} (\binom{n}{k-1}+\binom{n}{k}) a^k b^{n+1-k} + b^{n+1}
$$
infine li rimetto dentro modificando i parametri della sommatoria e ho che:
$$
= \sum_{k=0}^{n+1} \binom{n+1}{k} a^k b^{n+1-k}
$$
