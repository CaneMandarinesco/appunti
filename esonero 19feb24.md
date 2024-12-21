### es 1
#### A
Dire quali delle seguenti relazioni asintotiche sono vere:
* $n \log^2 n = o(n^2)$. **Vera**
* $\frac{n}{\log n} = \Theta(n)$. **Vera**
* $\frac{n^2 + \sqrt{ n }}{\sqrt{ n }} = \Theta(n^2)$. $\dots = \frac{\sqrt{ n } n^2 + n}{n} = \sqrt{ n } \cdot n + 1 = n^{1+1/2} = n ^ {\frac{3}{2}}$. **Falsa**
* $\sqrt{ \log n } = \omega(\log \log n)$. $\lim_{ n \to \infty } \frac{\sqrt{ \log n }}{\log \log n} \sim \frac{\log n}{\log^2 (\log n)} = \frac{\log n}{\log(2 \log n)} = \log (n-2\log n) \to \infty$. **Vera**
* $2^{\log n \log n} = \Omega(n^{10})$. $2^{\log(2n)} \sim 2^{\log n} = n \neq \Omega(n^{10})$. **Falsa**
* $3^n = \Theta(2^n)$. **Vera**
* $n 2^n = \omega (2^{2n})$. $\lim_{ n \to \infty }\frac{n 2^n}{2^{2n}} = n 2^{-n} = \frac{n}{2^n} \to 0$. **Falsa**
* $2^n = \Omega(n^{200})$. **Vera**

#### B
Fornire soluzione asintotica alle seguenti relazioni di ricorrenza:
$$
T(n) = 4T\left( \frac{n}{2} \right) + n^{9/5}
$$
Dove, applicando il teorema master:
* $n^{9/5} \neq \Omega(n^{2 + e})$ quindi non puo' essere $\Theta(n^{9/5})$ 
* $n^{9/5} = O(n^{2+e})$, quindi e' vero che: $T(n) =\Theta (n^2 \log n)$
* $n^{9/5} = O(n^2)$, quindi e' vero che: $T(n) = \Theta(n^2)$
La soluzione e' $\Theta(n^2 \log n)$.

$$
T(n)=T(n-1) + T(n-2) + 1
$$
Si viene a creare un'albero sbilanciato, infatti $T(n)$ ha come figlio sinistro $T(n-1)$, che ha come figlio sinistro $T(n-2)$, ecc... . L'altezza dell'albero e' quindi $\Theta(n)$, e i nodi in totale sono: $$(\sum_{i=0}^{n-1}2^i) + 1 = \Theta(2^n)$$
Ogni nodo ha inoltre costo costante, il che e' trascurabile.

#### C
1. Che algoritmo useresti e quanto costa se: devo costruire un heap binomiale con $n$ chiavi?
Creerei un Heap Binomiale vuoto, successivamente inserisco con la procedura: `insert` tutte le $n$ chiavi. Tempo: $O(n \log n)$.

2. Ordinare $n$ interi compresi fra 1 e $n \log \log n$.
Userei Radix Sort o Integer Sort. Con Integer Sort la complessità e' $O(n+ n \log \log n) = O(n \log \log n)$, che e' meglio rispetto ad un algoritmo basato su confronti che hanno come lower bound: $O(n \log n)$.

3. In un grafo orientato, capire se tutti i nodi possono raggiungere due specifici nodi $t_{1}$ e $t_{2}$. **Boh**
4. In un grafo pesato e non orientato, capire se esiste un cammino minimo da $s$ a $t$ che oltre ad essere minimo passa per uno specifico nodo $u$. **Boh**


### Esercizio 2
Sia $A[1:n]$ un vettore i $n$ numeri positivi. Progettare un algoritmo che in tempo $O(n)$ e memoria ausiliaria costante trova il più piccolo indice $i$ tale che la somma dei primi $i$ elementi di $A$ e' maggiore della somma dei restanti elementi in $A[i+1:n]$. Si fornisca lo pseudo-codice dettagliato dell'algoritmo.

```c
l = 1
r = n
sl = 0 // somma da 0:l
sr = 0 // somma da r:n
while l < r:
	if sl <= sr:
		sl += A[l]
		l+=1
		continue

	// else:
	sr += A[r]
	r -= 1

return l
```

### Esercizio 3
Dopo 