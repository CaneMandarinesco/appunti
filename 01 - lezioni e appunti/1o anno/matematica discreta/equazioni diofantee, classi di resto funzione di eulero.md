### prop. 3.8.3:
siano $p,a,b \in \mathbb P$ tali che $p|a \cdot b$. se $p$ e' primo allora $p|a$ o $p|b$
#### dim.
viene dalla 3.8.1 (???)

### teo 3.8.4. Teorema Fondamentale Dell'aritmetica
sia $n\in \mathbb P$, $n \geq 2$ allora $n$ si esprime come prodotto di numeri primi in uno ed un solo modo

# eq. diofantee lineari 3.9
### teo 3.9.1:
siano $a,b,n \in \mathbb P$. allora esistono $x,y \in \mathbb Z$ tali che:
$$
a \cdot x + b \cdot y = n
$$
**se e solo se** $(a,b) | n$ dove per $(a,b)$ si intende $M.C.D(a,b)$

### teo 3.9.2
siano $a,b,n \in \mathbb P$ tali che $(a,b)|n$ allora tutte le soluzioni di:
$$
a \cdot x + b \cdot y = n
$$
sono della forma:  
$$
\begin{array}{a}
x = x_{0} - \left( \frac{b}{d} \right)t  \\
y = y_{0} + \left( \frac{a}{d} \right)t
\end{array}
$$
dove $d = (a,b), t\in \mathbb Z$ e $x_{0}, y_{0} \in \mathbb Z$ sono una sol. particolare

# classi di resto
### relazioni di congruenza
sia $n \in \mathbb P$ una relazione $\equiv_{n}$ su $\mathbb Z$ ponendo:
$$
a \equiv_{n} b \iff n|(b-a)
$$
interpretazioni:
* $a \text{ e } b$ hanno lo **stesso resto** quando divisi per **n**
* $a=k \cdot n + b$
* $n|(a-b)$
allora: $\forall a,b \in \mathbb Z$. la relazione $\equiv_{n}$ si chiama: ***relazione di congruenza modulo n***
* se $n=0$, la congruenza vale se $a=b$, quindi diventa di **uguaglianza**
* se $n=1$, la congruenza vale **sempre**

esempi:
$$
10 \equiv 14 (\text{ mod 4})
$$
* $10 /4 = 2R 2$
* $14 / 4 = 3 R 2$
$$
10 \equiv -2 ( \text{ mod 4 }) \to 10 \equiv 2( \text{ mod } 4 )
$$
### prop 3.10.1
sia $n \in \mathbb P$ allora $\equiv_{n}$ e' una relazione di equivalenza su $\mathbb Z$

### classi di equivalenza di modulo n
> [!note] DEF.
> $[a]_{n} = \{ b \in \mathbb Z: a \equiv_{n} b \}$

esempi:
$$
[2]_{5} = \{ \dots, -8,-3,2,7,12,\dots\}
$$
infatti: $5|(-8-2), 5|(-3-2), 5|(2-2), 5|(7-2), 5|(12-2)$

---
$$
[4]_{5} = \{ \dots, -6,-1,4,9,14,\dots \}
$$
per lo stesso motivo dell'esempio precedente

---

si possono **sommare** e **moltiplicare** tra loro le classi di equivalenza:
$$
[a]_{n} + [b]_{n} = [a+b]_{n}
$$
$$
[a]_{n} \cdot [b]_{n} = [a \cdot b]_{n}
$$
valgono altre proprietà come:
$$
[a]_{n} \cdot ([b]_{n} + [c]_{n}) = [a]_{n} \cdot [b]_{n} + [a]_{n} \cdot [c]_{n}
$$

> [!note] DEF. insieme delle classi di resto su $\mathbb Z$
> $\mathbb Z_{n} = \{ [0]_{n}, [1]_{n},\dots,[n-1]_{n}\}$


>[!warning] ATTENZIONE
> non e' vero che:
> $$
> [k]_{n} \cdot [a]_{n} = [k]_{n} \cdot [b]_{n} \to [a]_{n} = [b]_{n}
> $$
> con $[k]_{n}\neq [0]_{n}$

### prop. 3.10.2
siano $a,b,n,k \in \mathbb P$ tali che $(k,n)=1$ dove ricordo che $(k,n) \text{ significa } MCD(k,n)$
$$
[k]_{n} \cdot [a]_{n} = [k]_{n} \cdot [b]_{n} \iff [a]_{n} = [b]_{n}
$$

>[!note] DEF.
> un **inversa moltiplicativa** di $[a]_{n}$ e' una classe $[b]_{n} \in \mathbb Z_{n}$ tale che:
> $$
> [a]_{n} \cdot [b]_{n} = [1]_{n}
> $$

come si calcola?

---
$$
3 \cdot ? \equiv 1 \text{ mod } 5
$$
la risposta e': 
$$
3 \cdot 2 = 1 \text{ mod } 5
$$
perche': $\frac{6}{5}$ ha resto 1

---
$$
2 \cdot ? \equiv 1 \text{ mod } 11
$$
$$
2 \cdot 10 \equiv 1 \text{ mod 11}
$$
con $10$ abbiamo resto $-1$, la risposta e': 
$$
2 \cdot 11 \equiv 1 \text{ mod }11 
$$

---

***come si calcola***: quindi si calcola trovando quel numero che moltiplicato per $n$ e diviso per il $\text{ mod } m$, abbiamo resto 1.

### prop. 3.10.3
siano $a,n \in \mathbb P$  tali che $(a,n)=1$. allora:
$$
\exists! [b]_{n} \in \mathbb Z_{n} : [a]_{n} \cdot [b]_{n} = [1]_{n}
$$
esiste un' **unica inversa moltiplicativa**!

### semplificare gli esponenti
$$
23^3 \text{ mod 30}
$$

# funzione di eulero
$$
\Phi(n) = | \{ 1 \leq i\leq n: (n,i) =1 \}|
$$
ovvero il numero di $i$ tali che $MCD(n,1)=1$

>[!note] OSS.
>se $p$ e' primo allora: $\Phi(p)=p-1$

es:
* $\Phi(5) = |\{ 1, 2, 3, 4\}| = 4$ (esempio per oss)
* $\Phi(11) = |\{ \dots \}| = 10$ (per osservazione sopra)
* $\Phi(8) = |\{ 1,3,5,7 \}| = 4$
### teo 3.11.1
siano $p,q \in \mathbb P$ con $p,q$ primi e $p\neq q$ allora:
$$
\Phi(p \cdot q) = (p-1)(q-1)
$$
### teo 3.11.2:
sia $n\in \mathbb P$ decomposto in primi:
$$
n=p_{1}^{a_{1}} \cdot p_{2}^{a_{2}} \cdot \dots \cdot p_{r}^{a_{r}}
$$
allora: 
$$
\Phi(n) = n \cdot \left( 1- \frac{1}{p_{1}} \right) \cdot \left( 1 - \frac{1}{p_{2}} \right) \cdot \dots \cdot \left( 1-\frac{1}{p_{r}} \right)
$$
### cor 3.11.3
siano $a,b \in \mathbb P$ tali che $(a,b)=1$ ($MCD(a,b)=1$). allora:
$$
\Phi(a \cdot b) = \Phi(a) \cdot \Phi(b)
$$

### oss
sia $n \in \mathbb P$:
$$
E(n) = \{ [a]_{n}: 1\leq a\leq n, (a,n) = 1 \} 
$$

### pro 3.11.4
siano $n,k \in \mathbb P$ tali che $(k,n)=1$. allora la funzione:
$$
[a]_{n} \to [k]_{n} \cdot[a]_{n}
$$
e' $E(n) \to E(n)$ ed e' biettiva

### importante: teo 3.11.5 teorema di eulero
siano $k,n \in \mathbb P$ tali che $(k,n)=1$. allora
$$
k^{\Phi(n)} \equiv 1 \text{ (mod n)}
$$
### riassumendo
$\Phi(n)$ e' il numero di **interi positivi** minori di $n$ che sono primi rispetto ad $n$ ($MCD(n,i)=1$)