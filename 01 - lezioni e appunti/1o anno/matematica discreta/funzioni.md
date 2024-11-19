# 1.3 applicazioni tra insiemi
siano $A,B$ due insiemi, una **applicazione** da A in B e' una legge che associa ad un elemento un **unico elemento** di B.

> [!note] DEF.
> una applicazione (o funzione o mappa) e' un sottoinsieme $f \subseteq A \times B$ quindi e' un sottoinsieme che per ogni $a \in A$ esiste un unico $b \in B$ tale che: $(a,b) \in f$ che scrive: $f(a)=b$

>[!note] SURIETTIVA
>se $\forall b \in B$ esiste $a \in A$ tale che: $f(a) = b$

> [!note] INIETTIVA
> se $\forall x, y  \in A$ allora se $x\neq y$ allora $f(x) \neq f(y)$]

> [!note] BIUNIVOCA
> se valgono sia la **suriettiva** e **iniettiva**

>[!note] DEF.
> **composizione**: 
> $(g \circ f)(a) = g(f(a))$

## prop 1.3.1
siano $A,B,C$ insiemi, e $f: A\to B$ e $g: B\to C$ allora:
* $f \text{ e } g$ **iniettive** allora $g \circ f$ e' **iniettiva** 
* $f \text{ e } g$ **suriettive** allora $g \circ f$ e' **suriettiva**
* $f \text{ e } g$ **biunivoche** allora $g \circ f$ e' **biunivoca**

> [!note] DIM. INIETTIVA
siano $x,y \in A$, poiche' $f(x) \neq f(y)$ allora dato che vale anche per g (e' **iniettiva**), abbiamo che: $g(f(x)) \neq g(f(y))$

> [!note] DIM. SURIETTIVA 
> sia $c \in C$, poiché $g$ e' **suriettiva** allora $\exists b \in B$ tale che $f(b) = c$, la stessa cosa vale per $f$ quindi $g(f(a)) = g(b) = c$

>[!note] DEF.
> l'inversa di $f: A\to B$ e' $f^{-1}(x): B\to A$ 
> ovvero: $$ f^{-1} = \{ (b,a) \in B\times A: (a,b) \in f \} $$
> significa che le coppie ordinate di $(b,a)$ invertite sono elementi della funzione $f$

>[!note] OSS.
> $$f(a)=b \to f^{-1}(b) = a$$

>[!note] DEF.
>la funzione identita' su A e':
>$I_{A}: A\to A$ ed e' definita come:
>$$ I_{A}(a) = a$$

## prop 1.3.2
sia $A$ un insieme, $f:A\to A$, $g: A\to A$, $h: A\to A$ allora:
1. $f \circ g$ e' tale che: $A\to A$
2. le seguenti composizioni sono equivalenti:
	* $(f \circ (g \circ h ))(a) = f(g \circ h)(a) = f(g(h(a)))$
	* $((f \circ g) \circ h)(a) = (f \circ g) (h(a)) = f(g(h(a)))$ (???)
3. $(f\circ I_{A})(a) = f(I_{A}(a)) = f(a)$ e viceversa $(I_{A}\circ f) = \dots$

inoltre:
* $f \circ (g \circ h) = (f \circ g) \circ h$
* $f \circ I_{A} = I_{A} \circ f = f$
* $f \text{ biunivoca } \to f \circ f^{-1} = f^{-1} \circ f = I_{A}$

>[!note] DIM.
>$f \circ f^{-1}$:
>- $b= f^{-1}(a) \to f(b) = a \to f(f^{-1}(a)) = a \to (f \circ f^{-1})(a) = a = I_{A}(a)$
>
>$f^{-1} \circ f$:
>- $c = f(a)$ allora $f^{-1}(c) = a$. quindi $f^{-1}(f(a)) = a = (f^{-1} \circ f)(a) = I_{A}(a)$

>[!note] OSS.
>$f,g:A\to B$ sono uguali se:$$f(a)=g(a) \; \forall a\in A$$


