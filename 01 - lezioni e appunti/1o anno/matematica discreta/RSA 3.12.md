# 3.12 RSA
**problema fondamentale della crittografia**: spedire un messaggio da $A$ a $B$ in modo che solo $B$ possa decifrarlo

### come funziona
* $B$ sceglie due numeri primi: $p,q \in \mathbb P, p\neq q$ e calcola $n=p \cdot q$,quindi $B$ decide **il numero da usare per cifrare**
* quindi: $\exists e \in \mathbb P: MCD(e,(p-1)\cdot(q-1))$ 
* $B$ calcola l'inversa moltiplicativa $[d]_{(p-1)(q-1)}$ di $[e]_{(p-1)(q-1)}$ che esiste ed e' **unica**
* $B$ pubblica $n,e$ e tiene segreti $p,q \text{ e } d$

codifica:
* $A$ prende un messaggio $m$, $1\leq m\leq n$ e spedisce: $[\tilde{m}]_{n} = [m^e]_{n}$ con $(m,n)=1$ (mcd)

decodifica:
* $B$ riceve $\tilde{m}$ e calcola: $[\tilde{m}^d]_{n}$

---
quindi se devo:
* spedire a B un messaggio ($m$) devo usare le **chiavi pubbliche di B** per codificarlo
* decodificare un messaggio che mi ha spedito B ($\tilde{m}$) devo decodificarlo usando **le mie chiavi private**