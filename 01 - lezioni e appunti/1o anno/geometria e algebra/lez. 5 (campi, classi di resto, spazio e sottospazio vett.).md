$\mathbb K$ e' un campo se sono definite le operazioni di somma e prodotto:
* $+: \mathbb K \times \mathbb K  = \mathbb K$
* $\cdot : \mathbb K  \times \mathbb K = \mathbb K$

per la somma devono valere le seguenti proprietà:
* $(x-y)-z \neq x - (y -z)$
* $(x+y)+z = x+(y+z)$ (**associativa**)
* esiste un **elemento neutro** rispetto alla somma: $0_{K}$
* ogni elemento ha il suo **reciproco**: $x+(-x) = 0_{K}$
* $x+y = y +x$ (**commutativa**)

per il prodotto devono valere le seguenti proprieta':
* $(x \cdot y) \cdot z = x \cdot (y \cdot z)$
* esiste l'**elemento neutro** rispetto alla moltiplicazione: $1_{K}$
* esiste l'**inverso** di tutti i numeri tranne per $0_{K}$: $x \cdot x^{-1} = 1_{K}$

altre proprietà:
* distributiva a destra: $x \cdot (y + z) = (x \cdot y) + (x \cdot z)$
* distributiva a sinistra: $(x+y) \cdot z = (x \cdot z) + (y \cdot z)$

nota che:
* $-x = (-1) \cdot x$
* $x + (-1) \cdot x = (1-1) \cdot x = 0 \cdot x$

possiamo quindi affermare che:
* $\mathbb Z \text{ e } \mathbb N$ non sono campi
* $\mathbb Q$ e' un campo

> [!note] NOTA
> se trovo un contro-esempio per una proprieta', allora non $\mathbb K$ non e' un campo

## classi di resto
$$
[r]_{n}
$$

> [!note] DEF.
> tutti i numeri che divisi per $n$ danno resto $r$. 

* $\mathbb Z _n = \mathbb Z / \equiv_{n} \; = \{ \text{ classe di resto mod n } \}$
* $\mathbb Z_{n} = \{ [0]_{n}, [1]_{n}, \dots , [n-1]_{n} \}$

sapendo che:
$$
z = n \cdot q + r
$$
allora ho che:
$$
[z] = [r]
$$
* per altro vedi matematica discreta o internet
# spazio vettoriale
>[!note] DEF.
> un spazio vettoriale SU $\mathbb K$ e' un insieme $V$ con funzioni:
> - $V \times V \to V$
> - $\mathbb K \times V \to V$
> 
> che soddisfino certe proprieta'

### definizione 4.2
proprieta:
* **associativa**: $\forall v,w,u \to (v+w)+u = v+(w+u)$ 
* esistenza **elemento nullo** in $V$; $\forall v \to \exists \emptyset_{v} : \; v + \emptyset_{v} = v = \emptyset_{v} + v$
* esistenza **opposto** $\forall v \to \exists (-v): \; v + (-v) = \emptyset_{v} = (-v) + v$
* **commutativa**: $\forall v,w \to v + w = w + v$
* $\forall h,k \in \mathbb K; v \in V:\;(h \cdot k) \cdot v = h \cdot (k \cdot v)$
* $\exists 1_{\mathbb K}: 1_{\mathbb K } \cdot v = v$ 
* $k^{-1} \cdot (k \cdot v) = (k^{-1} \cdot k) v = 1_{\mathbb K} \cdot v = v$
* prop. distributiva a **destra**: $k \cdot(v' + v'') = (k \cdot v') + (k \cdot v'')$
* prop. distributiva a **sinistra**: $(k' + k'') \cdot v = (k' \cdot v) + (k'' \cdot v)$
* $\emptyset_{\mathbb K} \cdot v = \emptyset_{v}$

> [!note] stringhe
> $$
\forall \underline a = (a_{1},\dots,a_{k}) \in \mathbb K^n
> $$
> anche qui le stringhe si possono sommare e moltiplicare membro a membro.

## esempi di spazi vettoriali
1. $V^1_{O}, V^2_{O}, V^3_{O}$: ovvero lo spazio dei vettori applicati in un punto O (rette, piani, spazi)
2. $M_{m,n}(\text{ su }\mathbb R)$ (matrice) 

#### esempio 3:
sia $A$ un insieme qualunque, e $V$ l'insieme di tutte le funzioni da $A$ in $\mathbb R$. Definiamo una somma e un prodotto per scalari su $V$ in questo modo:

$$
\text{ se } f,g \in V \text{ e } \mu \in \mathbb R \to f+g \; \text{ e } \; \mu f: A \to R \text{ sono due funzioni}
$$
dove $\forall a$: 
* $(f+g)(a) = f(a) + g(a)$
* $(\mu f)(a) = \mu f(a)$

### esempio 4:
spazio vettoriale di tutte le funzioni: $V^E := E \to V$ (ossia che ad ogni elemento di $E$ associano un elemento di $V$, biettiva (????))

## sottospazio vettoriale
un sottospazio vettoriale e' un sottoinsieme $W \subseteq V$ che e':
* chiuso rispetto alla **somma** e al **prodotto per scalari**
* e' uno spazio vettoriale a tutti gli effetti

e quindi ho che: 
* $w_{1} + w_{2} \in W$
* $a \cdot w \in W$ 

>[!note] vettore nullo e opposto
> il **vettore nullo** si trova in qualsiasi sottospazio vettoriale dato che:
> $$
> O = 0w \in W
> $$
> analogamente ho che anche l'**opposto** di $v$ si trova in $W$:
> $$
> -v = (-1)v \in W
> $$

quindi se $W$ non contiene il **vettore nullo** o manca un **elemento opposto**, *allora non e' un sottospazio vettoriale*.   
inoltre: *un sottospazio e' uno spazio vettoriale a tutti gli effetti*(considerando le sue regole di somma e prodotto per scalare).  

> [!note] OSS.
> ${\emptyset}$ e' un sottospazio e $V$ e' sottospazio di se stesso

> [!note] OSS.
> i sottospazi di $R^n$ sono $R^m$ dove $m\leq n$.

#### esempio di sottospazio
$V$ e' uno sottospazio vettoriale di $R^{(a,b)}$ se:
* poniamo $A=\mathbb R$
* $V = \text{'funzioni continue' o 'funzioni derivabili'}$


