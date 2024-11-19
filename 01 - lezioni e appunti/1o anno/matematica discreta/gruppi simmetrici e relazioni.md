# esempi e controesempi
* esempio: non dimostra
* controesempio: dimostra che un affermazione non e' sempre vera
### es.
$P_{1}$ =*"ogni primo e' dispari"*
- *"3 e' primo ed e' dispari"*, e' un **esempio**
- *"2 e' primo ed e' pari"*, e' un controesempio e dimostra che $P_{1}$ non e' sempre vera, quindi e' **falsa**

---
> $[n] = \{1,2,3,\dots ,n\}$ 

# 1.3 gruppi simmetrici

> [!note] DEF.
> **gruppo simmetrico**:
> $$S_{n} = \{  f: [n] \to [n]: \text{ biunivoca }\}$$

ovvero sia $X = |n|$, allora $S_{n}$ e' un insieme composto da **tutte** le biezioni da $X$ a $X$.  
in $S_{n}$, $n$ indica il numero di elementi di $X$.

> le permutazioni (ovvero tutte le funzioni possibili in $S_{n}$) in un insieme di $3$ elementi sono $6$

> [!note] DEF.
 sia $f \in S_{n}$, allora scriviamo: $f = a_{1} a_{2}  \dots a_{n}$  (tutto attaccato, senza virgole o punti) per dire che:
>$$
>f(1)=a_{1}, f(2)=a_{2},\dots,f(n) = a_{n}
>$$

esempio:
$$
S_{3} = \{ 123,132,213,231,312,321 \}
$$
ovvero, l'insieme di tutte le possibili permutazioni di 3 elementi. ogni elemento rappresenta una funzione, per esempio:
* $123$ vuol dire che $f(1)=1, f(2)=2, f(3)=3$
* $213$ vuol dire che $f(1) = 2, f(2) = 1, f(3) = 3$

>[!note] OSS.
> **composizione**:
> siano $f,g \in S_{n}$, $f=a_{1}\dots a_{n}$ e $g=b_{1}\dots b_{n}$ allora: $$ 
> f \circ g = a_{b_{1}} a_{n_{2}} \dots a_{b_{n}} 
> $$

es.
$f=7153426$ e $g=6712534$, allora:

$$
f \circ g = 2671453
$$

>[!note] OSS.
>$f^{-1} = b_{1}\dots b_{n}$ dove $b_{i}$ e' la posizione di i in $a_{1}\dots a_{n}$
 ricordando che: $f^{-1}(b) = a$ e $f(a) = b$

es.
$n=7, f=5132647$ allora $f^{-1} = 2436157$

> [!note] OSS.
> considerando l'esempio sopra, più semplicemente data $f$, per ricavare $f^{-1}$ il procedimento e' il seguente:
> - il 1o elemento di $f^{-1}$ ha indice 1, quindi bisogna inserire il numero dell'indice in $f$, in cui si trova 1, ovvero 2
> - il 2o elemento di $f^{-1}$ ha indice 2, quindi come prima, 2 in $f$ si trova all'indice 4, quindi inserisco 4
> - ecc... fino ad n

>[!note] DEF. 
> gli elementi di $S_{n}$ si dicono **permutazioni**.
> il numero di permutazioni possibili in $[n]$ e' $n!$

> [!note] DEF.
> siano $A,B$ insiemi, $f: A\to B$ e siano $X \subseteq A$ e $Y \subseteq B$
> l'**immagine** di $X$ mediante $f$ e' $$f(x) = \{ f(a): a \in X \}$$
> la **controimmagine** di $Y$ mediante $f$ e' $$f^{-1}(Y) = {a \in A: f(a) \in y}$$

es. 
siano $A=[3]$ e $B=[2]$ e $f: A\to B$ tale che: $f(1)=2, f(2)=1, f(3)=2$. allora con $Y = {2}$ abbiamo che: $f^{-1}(Y) = \{ 1,3 \}$, ovvero ritorna l'insieme contente gli indici di 2.

### prop 1.3.3: 
siano $A,B$ insiemi, $f:A\to B$, $X,Y \subseteq B$. allora:  
1. $f^{-1} (X \cap Y) = f^{-1}(X) \cap f^{-1}(Y)$
2. $f^{-1} (X \cup Y) = f^{-1}(X) \cup f^{-1}(Y)$

> [!note] DIM. 1.
> 	sia $a \in f^{-1}(X \cap Y) \to f(x) \in X \cap Y \to f(a) \in X \text{ e } f(a) \in Y$ quindi: $a \in f(X)^{-1} \text{ e } a \in f(Y)^{-1}$
> viceversa si procede similmente

per 2. e' simile

# 1.4 relazioni
> cap. 7 libro

>[!note] DEF.
> una ***relazione*** su $A$ e' un **sottoinsieme**  $R \subseteq A\times A$.
> e si scrive: $$aRb$$
> ovvero: $a$ e' in relazione con $b$ e significa che $(a,b)\in R$ e che $a,b \in A$

e' simile a una funzione, infatti per definizione anche una funzione e' una relazione. la loro differenza e' che una relazione potrebbe associare piu' elementi di $B$ ad $A$

### proprieta'
* $R$ e' **riflessiva** se: $a \in A$ -> $aRa$ (allora per def. $(a,a) \in R$)
* $R$ e' **simmetrica** se: $aRb$ e' equivalente a $bRa$, quindi $\forall a,b\in A$
* $R$ e' **transitiva** se: $aRb$ e $bRc$ allora $aRc$ (a in relazione con b, b in relazione con c, allora a e' in relazione con c)

>[!note] DEF.
>$R$ si dice relazione di **equivalenza** se valgono tutte e 3 le proprieta'

es.
$A = [5]$, $R = \{(1,1),(2,2)(4,4),(5,5),(1,2),(2,3),(1,3),(3,2)\}$ 
> nota: $R \subseteq A \times A$

* non e' **riflessiva**: perche' $(3,3) \not\in R$, quindi con $a=3$ non vale: $aRa$
* non e' **simmetrica**: perche' $(1,2) \in R$ ma $(2,1) \not\in R$

quindi $R$ non puo' essere di equivalenza su $A$ (**ricorda** che una relazione di equivalenza e' definita in un insieme)

---

> [!note] DEF.
> sia $R$ una relazione di equivalenza su $A$. sia $a \in A$.
> la ***classe di equivalenza di*** a e':
> $$[a] = \{ b\in A: aRb \}$$
> quindi con questa notazione indichiamo tutte le $(a,b) \text{ t.c } \in A$, ovvero tutte le $a$ in relazione con $b$


### prop 1.4.1
$R$ relazione di **equivalenza** su $A$. siano $a,b \in A$. allora $[a]=[b]$ o $[a] \cap[b]= \emptyset$.
>[!note] DIM.
> supponiamo che $[a] \cap[b] \neq \emptyset$ e sia $c \in [a] \cap [b]$. Allora $cRa$ e $cRb$ e dato che e' **simmetrica** (essendo R di equivalenza) abbiamo che: $aRc$ e $cRb$
> e essendo transitiva: $aRb$, ora sia $x \in [a]$, allora $xRa$, ma essendo transitiva abbiamo che $xRb$ e quindi $x\in[b]$. **viceversa** abbiamo che:
> sia $y \in [b]$ -> $yRb$ -> $yRa$ -> $y \in [a]$



# partizioni

>[!note] DEF.
>una **partizione** di un insieme finito $A$, e' una collezione di sottoinsiemi $A_{1},A_{2},\dots,A_{n}$ la cui unione e' $A$, e i loro sottoinsiemi sono chiamati **blocchi**

per esempio, una possibile partizione di $A = {a,b,c,d,e}$ e':
$$
A_{1} = \{ a,c \}, \;\;\; A_{2}=\{ b,e \}, \;\;\; A_{3} =\{d \}
$$
* un blocco non può' essere vuoto
### es. 
$$
A = [8]
$$
allora possiamo dire che:
$$
\Omega= \{ \{ 1,3 \}, \{ 4 \}, \{ 2,5,7 \}, \{ 6,8 \} \}
$$
### coroll. 1.4.2
sia $R$ una relazione di equivalenza su $A$. Allora le classi di equivalenza di $R$ sono una partizione di $A$

> [!note] DIM.
> sia $a \in A$ allora $[a]\neq \emptyset$ (dato che $a \in [a]$)
> boh...



