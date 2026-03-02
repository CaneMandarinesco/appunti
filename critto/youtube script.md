Oggi vediamo vedere come implementerei in C un cifrario molto semplice, che cifra in maniera **perfettamente sicura** i dati.

Cominciamo da un po di teoria.
Per cifrare dati, implementeremo il One Time Pad, un cifrario perfettamente sicuro, resistente a qualsiasi attacco.

Andiamo a vedere la notazione che useremo:
* $x$ rappresenta un generico testo da cifrare, il **plaintext**
* $k$ rappresenta una generica **chiave** usata per cifrare il plaintext 
* $y$ rappresenta un generico testo cifrato, il **cyphertext**
* $E(x,k)$ e' la funzione di cifratura da implementare
* $D(y,k)$ e' la funzione per decifrare il risultato di $y = E(x,k)$

Ripassiamo anche come funziona lo `XOR` tra due bit: $a \oplus b = 1$ se e solo se $a \neq b$.

| a   | b   | $a \oplus b$ |
| --- | --- | ------------ |
| 0   | 0   | 0            |
| 0   | 1   | 1            |
| 1   | 0   | 1            |
| 1   | 1   | 0            |

Definiamo $E$ e $D$ secondo OTP:
* $E(x,k)= x \oplus k$
* $D(y,k) = y \oplus k$
* $x,y,k$ sono stringhe di bit, tutte della stessa lunghezza.

**Perche' funziona**? Ossia, perche' riesco a decifrare correttamente un messaggio se uso la chiave giusta?
* vogliamo far vedere che $D(E(x,k),k) =x$ (requisito fondamentale per qualsiasi cifrario)
* Con $y = x  \oplus k$, allora $D(y,k)$ diventa, $(x \oplus k) \oplus k = x$.
* L'ultima parte 'e vera perche' $k \oplus k$ e' sempre uguale a $0$.
* Ed $x \oplus 0 = x$.

**Perche' e' sicuro**? 
Prima di tutto: cosa vuol dire **sicurezza perfetta**?
**Intuitivamente** quando utilizziamo un cifrario, per essere perfettamente sicuro vogliamo che: scegliendo una chiave $k$ randomicamente, allora il risultato $y$ sembri randomico tanto quanto $k$.

In linguaggio matematico, un cifrario e' perfettamente sicuro se:
$$
\forall x_{0}, x_{1}, \forall \bold y \text{ allora } P(E(x_{0}, k) = y) = P(E(x_{1}, k) = y)
$$

Se vale la sicurezza perfetta su un cifrario, allora un **qualsiasi attaccante** che conosce $y$ non puo' ottenere informazioni certe su come e' fatto il messaggio $x$ e la chiave $k$.

Intuizione sulla sicurezza perfetta di OTP:
sia $y=0$, allora 