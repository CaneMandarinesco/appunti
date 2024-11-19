> [!info] DEF.
>  Una successione reale: $\{a_{n}\}_{n\geq n_{0}}$ e' una funzione che per ogni $n \in N: n \geq n_{0}$ esiste un corrispettivo **reale**: $a_{n}$
>  dove $a_{n}$ e' il termine generale
# limite di una successione
$$
\lim_{  n \to \infty } a_n = 
$$
* $+\infty$ se $\forall M \in R \text{, } \exists N\geq n_{0}: \forall n > N\text{, }$ $a_{n} > M$
	* **divergenza**
	* ovvero: per ogni M (un numero grande), esiste almeno un $a_{n}$ più  grande di M
* $-\infty$ se $\forall M \in R\text{, } \exists N\geq n_{0}: \forall n>N, a_{n} < M$
	* **divergenza**
* $\exists l \in R$ se $\forall \epsilon > 0 \text{, } \exists N\geq n_{0}: \forall n>N, |a_{n}-l| < \epsilon$ oppure $l-\epsilon < a_{n} < l+\epsilon$
	* **convergenza**
	* ovvero la **distanza tra $a_{n} \text{ e } l$ e' minore del valore di $\epsilon$** (per un certo $n>N$, quindi non sempre)
* $\nexists$ se altrimenti 
> se $l=0$ abbiamo un infinitesimo

> [!info] OSS.
> il limite di una successione $\{a_{n}\}_{n\geq n_{0}}$ non dipende dal valore di $n_{0}$

# proprietà dei limiti
## 1. unicità
L'operazione del limiti e' un operazione che ha un risultato unico (come 2+2=4 fa sempre 4)
> [!note] DIM.
>  Se per assurdo:  $\lim_{ n \to \infty } a_{n} = L_{1}$ e allo stesso tempo $\lim_{ n \to \infty } a_{n} = L_{2}$
>  Vuole dire che per la definizione di limite convergente valgono:
> - $|a_{n}-L_{1}| < \epsilon$
> - $|a_{n}-L_{2}| < \epsilon$
> 
> Quindi se le unisco insieme (facendo una somma tra i termini a sinistra e i termini a destra) ho che:
> - $|a_{n}-L_{1}| + |a_{n} - L_{2}| < 2\epsilon$
> - e sapendo che: $|a_{n} - L_{1}| = |L_{1} - a_{n}|$
> 	- dato che, per esempio: $|5 - 2| = |2-5| \to 3=|-3|$
> 	- e ancora: $|7-20| = |20-7| \to |-13| = 13$
> 
> usando la **disuguaglianza triangolare** so che: 
> - $|L_{1} - L_{2}| = |L_{1} -a_{n} + a_{n} -L_{2} | < |L_{1}-a_{n}| + |a_{n} - L_{2}| < 2\epsilon$
> - quindi: $|L_{1}-L_{2}| < 2\epsilon \to  \frac{|L_{1}-L_{2}|}{2} <\epsilon$
> 
> contraddizione: se $\epsilon = \frac{|L_{1}-L_{2}|}{2} \to \frac{|L_{1}-L_{2}|}{2} \not<\frac{|L_{1}-L_{2}|}{2}$
> dove: $\frac{|L_{1}-L_{2}|}{2}$ e' sempre un valore  > 0 che rispetta la definizione del limite finito ($\forall \epsilon > 0$)

## 2. successione limitata
Se  il limite di una successione e' limitato(/convergente) **allora** la successione sara limitata. Non vale il viceversa.

## 3. sotto successioni
Il limite di una successione $a_{n}$ esiste allora il limite di tutte le sue sotto successioni $a_{n_{k}}$ hanno lo stesso risultato e:
* $\lim_{ n \to \infty } a_{n}= \lim_{ n \to \infty } a_{n_{k}}$

## 4. permanenza del segno
se il limite diverge a $+\infty$ o converge a un valore > 0 allora: $\exists N: \forall n>N, a_{n} > 0$
> [!note] DIM.
> - supponiamo che: $\lim_{ n \to \infty }a_{n} = l > 0$
> - allora per la definizione abbiamo che:  $|a_{n} - l| > \epsilon$
> - ovvero: $(a_{n}-l < \epsilon < a_{n}+l) \to (l-\epsilon < a_{n} < l+\epsilon)$
> - possiamo dire che $\epsilon = \frac{l}{2} > 0$
> - quindi sostituendo e sommando abbiamo che: $\frac{1}{2}l < a_{n} < l+ \frac{1}{2}l$ 
> - $\frac{1}{2}l$ e' una quantità sicuramente > 0 quindi $a_{n}$ e' sempre compreso tra due numeri > 0

## 5. confronto 
siano $a_{n} \text{ e }b_{n}$ due successioni t.c.  $a_{n} \leq b_{n}$. allora se i limiti di $a_{n} \text{ e } b_{n}$ esistono e sono rispettivamente $l_{1} \text{ e } l_{2}$ **allora** possiamo affermare che $l_{1} \leq l_{2}$
dim. boh

## 6. doppio confronto (carabinieri)
easy

## 7. limite di successione limitata per successione infinitesima
sia $a_{n}$ una successione limitata, e $b_{n}$ una successione infinitesima (ovvero che $\lim_{ n \to \infty } b_{n} = 0$) allora l'operazione: $\lim_{ n \to \infty } a_{n} * b_{n} = 0$
> [!note] DIM.
> essendo $b_{n}$ limitata superiormente: $\exists M > 0: \forall n>n_{0}, b_{n} \leq M$: 
> - $|a_{n}|  |b_{n}| < M|a_{n}| < 0$
> - $0 \leq |a_{n} - b_{n}|$
> e per confronto $a_{n}*b_{n} \to 0$


## 8. algebra dei limiti
sono valide le seguenti operazioni: 
con $\lim_{ n \to \infty } a_{n} =l_{1} \text{ e } \lim_{n \to \infty } b_{n} = l_{2}$
$$
\lim_{ n \to \infty } \frac{a_{n}}{b_{n}} = \frac{l_{1}}{l_{2}}
$$
$$
\lim_{ n \to \infty } a_{n} * b_{N} = l_{1}*l_{2}
$$
$$
\lim_{ n \to \infty } a_{n} + b_{n} = l_{1} + l_{2}
$$

## 9. funzione monotona:
...
## 10. funzioni continue
se una funzione e' continua **allora**:
$$
\lim_{ n \to \infty } f(a_{n}) = f(l)
$$

# numero di nepero
la successione: $\{\left( 1+\frac{1}{n} \right)^n\}_{n\geq 1}$ ed il suo limite a $+\infty$ converge ad $e$ (numero di nepero).  
per dimostrarlo bisogna far vedere 2 cose:
1. stretta crescenza
2. e' limitato superiormente

> [!note] DIM. 1. STRETTA CRESCENZA
>vogliamo di mostrare questo:
>$$
> \forall n \geq 1: \left( 1+\frac{1}{n+1} \right)^{n+1} > \left( 1+ \frac{1}{n} \right)^n
> $$
>
> applico il [[teorema potenza di un binomio]] e sposto fuori dalla sommatoria l'elemento n+1 ed ho che:
> $$
> \left( 1+\frac{1}{n+1} \right)^{n+1} = \sum_{k=0}^{n} \binom{n+1}{k}\frac{1}{(n+1)^k} + \frac{1}{(n+1)^{n+1}}
>$$ 
> 
>- nota inoltre che l'elemento n+1: $\frac{1}{(n+1)^{n+1}} \text{ che e' sempre } > 0$ 
> - inoltre ho che facendo l'analisi del **termine generale**: 
> $$
> \sum_{k=0}^{n}\binom{n+1}{k} \frac{1}{(n+1)^k} \geq \sum_{k=0}^{n}\binom{n}{k} \frac{1}{n^k} (1)
> $$
 > infine: 
> $$
> \sum_{k=0}^{n}\binom{n}{k} \frac{1}{(n+1)^k} + \frac{1}{(n+1)^{n+1}} > \sum_{k=0}^{n}\binom{n}{k} \frac{1}{n^k}
> $$
> quindi e' crescente

>[!note] DIM. 2. LIMITATA SUPERIORMENTE 
> analizzando i primi termini della sommatoria abbiamo che: 
> $$
> \left( 1+\frac{1}{n} \right)^n = \sum_{k=0}^{n}\binom{n}{k} \frac{1}{n^k} = 1 + 1 + \sum_{k=2}^{n}\binom{n}{k} \frac{1}{n^k} 
> $$
> da qui si procede per confronto con la [[serie numeriche#^d04ed2|serie telescopica]]:
> $$
> 2 + \sum_{k=2}^{n}\binom{n}{k} \frac{1}{n^k} \leq 2 + \sum_{k=2}^{n}\left( \frac{1}{k-1} - \frac{1}{k} \right) \leq 2+1- \frac{1}{n} < 3 \text{ } (2)
> $$

 >[!note] (1) 
 > ricordando che: 
 > $$
 > \binom{n}{k} = \frac{n!}{k!(n-k)!} \to \binom{n+1}{k} = \frac{(n+1)!}{k!(n-k)!}
 > $$
 > $$
> \binom{n+1}{k} \frac{1}{(n+1)^k} = \frac{\bcancel{n+1}}{\bcancel{n+1}} \frac{n}{n+1} \dots \frac{n+1-k+1}{n+1} \frac{1}{k!}
> $$
> mentre:
> $$\binom{n}{k} \frac{1}{n^k} = \frac{\bcancel{n}}{\bcancel{n}} \frac{n-1}{n} \dots \frac{n-k+1}{n} \frac{1}{k!}
> $$
> al numeratore abbiamo lo sviluppo del fattoriale: $(n)(n-1)(n-2)(\dots)(1)$
> al denominatore abbiamo lo sviluppo di: $n^k$ ovvero: $n * n * n * \dots$ k-volte
 
 

> [!note] (2)
> $$
> \binom{n}{k} \frac{1}{n^k} \leq \frac{1}{k!} \leq \frac{1}{k(k-1)} = \frac{1}{k-1} - \frac{1}{k}
> $$
> perche' $\binom{n}{k} \frac{1}{n^k}$ non e' altro che: $\frac{1}{n^k} *$ vari termini < 1
> quindi e' sicuramente $\leq \frac{1}{k!}$


# limiti di funzioni
## Retta estesa e intorni e punto di accumulo, definizioni varie
$$
\bar{\mathbb{R}} = R \cup\{-\infty, +\infty\}
$$
un intorno: $I(x_{0},r)$ e' definito come
* $(x_{0}-r,x_{0}+r)$ se $x_{o} \in \mathbb{\mathbb{R}}$
* $(r, +\infty)$ se $x_{o} = +\infty$
* $(-\infty,r)$ se $x_{0} = -\infty$

$x_{0}$ e' un punto di accumulo in $D$ se:
* $\forall r > 0, (I(x_{0}, r) \cap D - \{x_{0}\}) \neq \emptyset$
ovvero se per ogni intorno centrato in $x_{0}$ e di ampiezza $r$ c'e' sempre un elemento in più rispetto a $x_{0}$ stesso


> [!note] DEF. LIMITE
> si dice inoltre che il limite di f(x) per x che tende  a $x_{0}$ vale $l$ se:
>$$
>\forall \epsilon>0, \exists \delta: \forall x \in ((D \cup{} I(x_{0}, \delta) \text{\\}  x_{0} ) f(x) \in I(l,\epsilon) 
>$$


> [!warning] INESATTEZZA
> sugli appunti del tauraso sta scritto cosi: $\forall \epsilon>0, \exists \delta: \forall x \in ((D \cup{} I(x_{0}, r) \text{\\}  x_{0} ) f(x) \in I(l,\epsilon)$ 
> dove suppungo che r sia stata scambiata erroneamente con epsilon
> dunque di seguito riporto una forma alternativa per dire la stessa cosa

forma alternativa della definizione (si può trovare su youmath): 
$$
\forall \epsilon, \exists\delta: \forall x \in D: 0<|x-x_{0}|<\delta \to |f(x)-l|<\epsilon
$$
ovvero: per ogni $\epsilon$ che e' **maggiore** della distanza tra $f(x) \text{ e } l$, esiste un $\delta$  **maggiora** la distanza tra $x \text{ e } x_{0}$

>[!note] TEO. PONTE
> $\lim_{ x \to x_{0} }f(x) = l$ **se e solo se** esiste una successione $x_{n}$ che tende a $x_{0}$ tale che: $\lim_{ n \to \infty } f(x_{n}) = l$.
> nota: quindi tutti i teoremi applicabili ai **limiti di successioni** sono applicabili ai **limiti di funzioni**

## proprieta' dei limiti di funzioni (non ci sono dimostrazioni!)
rispetto ai limiti di successioni in generale queste proprietà valgono dentro un **intorno** in $D\backslash\{x_{0}\}$ e sono facilmente riconducibili ai limiti di successione
### 1. unicita
ovvero: l'operazione del limite su una **funzione esiste ed e' unico**

### 2. limitatezza
se $\lim_{ x \to x_{0} } f(x) = l \in \mathbb{R}$ allora esiste un intorno $D \cap{} I(x_{0},r) \backslash \{x_{0}\}: f(x) \text{ e' limitata}$  
ovvero: f e' **limitata** in un certo **intorno** di $x_{0}$

### 3. tipo permanenza del segno
se $\lim_{ x \to x_{0} } f(x_{0}) = l > 0$ allora: $\exists r>0: \forall x \in (I(x_{0}, r)) \to f(x) > 0$
ovvero se  $f(x_{0}) \to l > 0$ allora esiste un intorno in cui $f(x)$ e' **positiva**

### 4. confronto
se $\lim_{ x \to x_{0} } f(x) = l_{1}$ e $\lim_{ x \to x_{0} }f(x)=l_{2}$ e $l_{1} \leq l_{2}$ allora esiste il solito intorno in $x_{0}$ t.c. $f(x) \leq g(x)$

### 5. carabinieri
stessa cosa dei limite di successione ma se esiste l'intorno per cui vale la proprietà.

### 6. funzione limitata per funzione infinitesima
come per i limiti di successione il limite **vale 0**

### 7. algebra dei limiti
come per i limiti di successione

### 8. limite destro e sinistro
se esistono limite destro e sinistro in $x_{0}$ **allora** esiste il limite in $x_{0}$

### 9. funzione composta
se $\lim_{ x \to x_{0}} f(x) = l$ e $\lim_{ x \to l } g(x) = L$ allora esiste un intorno nel **dominio di f(x)** t.c $f(x) \neq l$ -> **allora** $\lim_{ x \to x_{0} } g(f(x)) = L$

