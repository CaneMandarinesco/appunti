**Gruppo**: $G$ su cui per ogni coppia $a,b \in G$ e' definita l'operazione $+$.
* $a+b \in G$
* $a + (b+c) = (a+b)+c$
* $a + (-a) = 0$, ossia esiste sempre l'inverso di $a$
* $a + 0 = a$, ossia esiste l'elemento neutro

$Z_{n}$: e' l'insieme dei resti in modulo $n$. Questo e' un gruppo se $n$ e' primo (???)

**Gruppo ciclico finito**: $G$ e' ciclico se $\exists g \in G$ tale che ogni elemento in $G$ può' essere descritto come somma di $i$ volte $g$: $G=\{0g, g, 2g, \dots\}$.

**Ismorfismo** verso $Z_{n}$: da prima, mediante le somme sappiamo che ogni elemento in $G$ e' descritto come $ig$. L'isomorfismo $G \to Z_{n}$  definita come $ig \to i \mod n$ mostra che $G$ e' identico a $Z_{n}$. Inoltre $(i+j)g \to i+j \mod n$.

**Sottogruppo**: $S \subseteq G$ tale che valgono le proprieta' del gruppo.

**Lagrange**: Sia $S+g = \{s+g: s\in S\}$ ed e' similmente $S+h$. Allora $S+g = S+h$ se e solo se $g-h$ sta in $S$ (altrimenti differiscono per qualche elemento). Questo dimostra il teorema di lagrange: $|S|$ divide $|G|$.

**Field**: $F$ su cui sono definite $+$ e $*$.
* $F$ e' gruppo abeliano su $+$
* $F - \{0\}$ e' gruppo abeliano su $*$.
* Vale la proprietà distributiva tra $+$ e $*$.

**Galois Field**: $F_{p}$ con $p$ primo e' un campo. Infatti se $p$ primo allora $F_{p}$ e' abeliano su $+$, ed esiste sempre l'inversa moltiplicativa $aa^{-1}=1$ in $F_{p} - \{0\}$.

**Sottocampo di** $F_{p}$: 
* Sia $S(1)$ il sottogruppo ciclico generato sommando $1$ per $n$ volte. 
* Allora: $|S(1)| = n$, ed e' isomorfo ad $Z_{n}$.
* $n$ deve essere primo altrimenti non puo' essere gruppo abeliano su $*$ con identita 1.

**Caratteristica**: preso un sottocampo $F_{p}$ di $F_{q}$, allora $p$ e' la caratteristica di $q$.
