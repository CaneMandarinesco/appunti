## dimostrazioni per assurdo
Dato una affermazione $P$ si assume che $\neg P$ sia vera e si cerca di arrivare ad un assurdo:
* $\neg P$ sia vera che falsa
* Esiste un'affermazione $Q$ che e' sia vera che falsa
* $Q$ non e' ne vera ne falsa
* $Q$ e' falsa ma anche $\neg Q$ lo e'

> [!note] Richiami di matematica
> * $\mathcal P(A) := \{ B : B \subseteq A \}$, ovvero tutti i sottoinsiemi di A
> * $\mathcal |P(A)| = 2^{|A|}$
> * definizioni di funzioni iniettive, suriettive e biunivoche


#### Esercizio 2.
Con $X$ e $Y$ due insiemi finiti, e sia $f: X\to Y$. Allora:
* $|X| \geq |Y|$ se $f$ e' iniettiva
* $|X| \leq |Y|$ se $f$ e' suriettiva
* $|X|= |Y|$ se $f$ e' biettiva

Per esempio,  $f: \mathbb N \to \text{ numeri pari }$, definita come $f(n) = 2n$ e' una funzione biettiva. 

> Notando che gli ordini degli infiniti sono gli stessi posso support che $f$ sia biettiva.

#### Esercizio 3.
Trovare una funzione biunivoca da $\mathbb N$ all'insieme dei $\text{ numeri dispari }$.

---
## Teorema di Cantor
Non esiste una funzione biunivoca fra $\mathbb N$ e $\mathcal P(\mathbb N)$. 

### dim. 
ipotizzando per assurdo che esista questa funzione, allora per ogni $S$ sottoinsieme di $\mathbb N$ c'e' un $n: A_{n} = S$. Dato che $A_{n}$ e' sottoinsieme di $\mathbb N$ allora $A_{n}$ **puo' contenere o non contenere** $n$.

Di conseguenza, prendo in considerazione $C = \{ n\in \mathbb N : n \not\in A_{n} \}$. Essendo $C$ sottoinsieme di $\mathbb N$ allora dovrebbe esistere $k: C = A_{k}$ ma $k$ sta in $C$?.

Ecco l'**assurdo**: 
* Se $k \not\in C$ questo vuol dire che $C = A_{k}$ e $k \not\in A_{k}$. Ma allora  dovrebbe essere vero che $k \in C$.
* Se $k \in C$ allora questo vuol dire che $C = A_{k}$ e quindi $k \in A_{k}$, ma similmente a prima questo vuol dire che: $k \not\in C$

Il Teorema di Cantor si applica (credo) anche a questo quesito logico:

> [!note] Paradosso del Barbiere
> In una citta c'e' un solo barbiere che rade tutti e solo quelli che **non si radono da soli**. Allora chi rade il barbiere?
> * Se il barbiere si rade da solo allora **non e' un barbiere**.
> * Se il barbiere si fa radere da qualcun'altro allora **essendo l'unico barbiere si fa radere da se stesso**!

---
### Dimostrazioni per Assurdo
....

---
### Dimostrazioni per Induzione
considerando l'affermazione: $n^2 + 3n + 5$ e' **dispari** per ogni $n \geq 0$

#### Versione 1
* verifico che $P(b)$ sia vero con $P$ definita per ogni $n \geq b$ (**Passo Base**)
* dato un qualunque $k \geq b$ se $P(k)$ e' vero allora e' vero anche $P(k+1)$ (**Passo Induttivo**)

> Nota:  $P(k)$ e' l'**ipotesi** induttiva mentre $P(k+1)$ e' la **tesi** induttiva.

#### Versione 2
Sia $b$ un numero intero e sia $P(n)$ un enunciato definito per ogni $n \geq b$ e sia $c: c \geq b$. 
* verifico che $P(k)$ sia vera per ogni $k$ t.c $b \leq k \leq c$
* dimostro che per qualunque $n\geq c$ se $P(k)$ e' vera in $b \leq k \leq c$, allora anche $P(n+1)$ e' vera.



