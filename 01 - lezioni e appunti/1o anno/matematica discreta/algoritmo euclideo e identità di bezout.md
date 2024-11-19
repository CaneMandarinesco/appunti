siano $a,b \in \mathbb P$
>[!note] DEF.
>il massim comune divisore di $a$ e $b$ (scritto $MCD(a,b) \text{ o } GCD(a,b)$) e': 
>$$
>MCD(a,b) = max\{ c \in \mathbb P: c|a \text{ e } c|b \}
>$$
>quindi il piu' grande numero $c$ che divide sia $a \text{ che } b$
### prop 3.7.1 (teorema della divisione)
siano $a,b \in \mathbb P$ con $a \geq b$, allora $\exists q,r \in \mathbb Z$ tali che:
$$
a = b \cdot q + r
$$
e: $0\leq r<b$

# Algoritmo euclideo
serve per calcolare l'MCD tra due numeri.
siano $a,b \in \mathbb P$. sia $a\geq b$ allora $\exists q,r \in \mathbb Z$:
$$
a = b \cdot q + r \,\,\,\, , \,\,\,\, 0\leq r<b
$$
ora, se $r=0$, allora $MCD = b$  
invece se $r>0$, allora $\exists q_{1},r_{1}1 \in \mathbb Z$ tali che
$$
b = r \cdot q_{1} + r_{1} \,\,\,\,, 0\leq r_{1} < r
$$
e cosi via...  
si ottengono quindi due sequenze fatte cosi:
$$
a = b \cdot q + r \,\,\,\, , \,\,\,\,0\leq r<b
$$
$$
b = rq_{1} + r_{1} \,\,\,\, , \,\,\,\, 0\leq r_{1} < r
$$
$$
r = r_{1}q_{2}+r_{2} \,\,\,\, , \,\,\,\, 0\leq r_{2} < r_{1}
$$
e cosi via... fino a che non si ottiene $r_{k}=0$, e quindi $MCD = r_{k-1}$

calcolare $MCD(78,18)$, dove $a=78 \text{ e } b=18$.
$$
78 = 18 \cdot 4 + 6
$$
$$
18 = 6 \cdot 3 + 0
$$
quindi $MCD(78,11)=6$

> [!note] OSS.
> $MCD(a,b) = MCD(b,r)$

# prop. 3.8.1: identita' di bezout
siano $a,b \in \mathbb {P}$. allora $\exists x,y \in \mathbb Z$ t.c. $$
MCD(a,b) = a \cdot x + b \cdot y
$$quindi si puo' esprimere l'MCD come una combinazione lineare tra due numeri $a$ e $b$
### dim.
boh, tanto e' inutile.