
### Pumping Lemma per context-free
Per ogni $L$ context-free esiste $p_{L} > 0$ tale che per ogni $z \in L$, se $|z| \geq p_L$ allora esistono $u, v, w, x, y$ (5 parole) t.c:
* $z$ e' concatenazione delle 5 parole.
* $|vwx| \leq p_L$
* $|vx| \geq 1$ (non entrambe vuote)
* $uv^{h}wx^{h}y \in L \forall h \geq 0$

Ogni linguaggio context-free rispetta il pumping lemma, ma linguaggi non context-free potrebbero rispettare il pumping lemma!

Come dimostrare per assurdo:
* ipotizziamo che $L$ sia context free e che quindi abbia un valore $p_L$ e consideriamo $S = L(p_L)$, per esempio: $a^{p_{L}}b^{p_{L}} c^{p_{L}}$
* si divide la stringa $S$ in $uvwxy$
* troviamo la contraddizione.


### es
Con $L = \{a^{n} b^{n}c^{n}: n \geq 0\}$ dimostriamo che non e' context-free trovando un'assurdo ipotizzando che lo sia.

Per qualsiasi $p_L$ che scegliamo, si ha che $|z| = 3 p_{L} > p_{L}$ quindi le stringhe che prendiamo in considerazione con $p_L$ sono valide.

**OSS1**: Per costruzione, ogni sequenza di $a,b,c$ e' lunga $p_L$. 
Il pumping lemma ci dice che $|vwx| \leq p_L$, dunque la stringa $vwx$ e' contenuta in un'unica $a^{n}\subset z$, oppure in $b^{n} \subset z$, oppure in $c^{n} \subset z$.

L'altra opzione e' che $vwx$ sia contenuta in $a^nb^n$ oppure in $b^{n}c^n$.

Ricapitolando: $vwx$ e' costituita da un unico simbolo oppure da due simboli, attraversando dunque la frontiera delle $b$, o quella delle $c$.

**OSS2**: secondo quanto detto, non puo' essere che $vwx$ contenga 3 simboli, in quanto per costruzione $vwx$ dovrebbe essere lunga almeno $p_{L}+1$ caratteri, contro il fatto che $|vwx| \leq p_L$ per pumping lemma.

Quindi, se $vwx$ contiene un solo tipo di caratteri, e ricordando che $|vx| \geq 1$, allora $uv^{h}wx^{h}y \notin L$ per $h \geq 0$

Se $vwx$ sta in una sequenza di caratteri: $v$ od $x$ ha quel carattere ed $v^h$ o $x$

### Pumping Lemma per regolari
Per ogni linguaggio $L$ regolare, esiste un intero $p_{L} > 0$ tale che $\forall z \in L$, con $|z| \geq p_L$ esistono tre parole $u,w,z$ t.c:
1. $z =uvw$
2. $|uv| \leq p_L$
3. $|v| \geq 1$
4. $uv^{h}w \in L \forall h \geq 0$.

### es:
Dimostrare che $L = \{a^{n}b^{n}: n \geq 1\}$ non e' tipo 3.

Usando con $n = p_L$, si ha che $|z| = 2p_L$. 
Dato che $uv \leq p_L$, allora $uv$ contiene solo $a$, ma per qualche $h$ sicuramente la parola non sta piu in $L$.



