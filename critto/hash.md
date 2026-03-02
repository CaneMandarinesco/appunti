**Hash Crittografico**: e' definito su $(\mathcal X, \mathcal Y, \mathcal K, \mathcal H)$
* $\mathcal X$ e' l'insieme dei messaggi di cui calcolare la firma
* $\mathcal Y$ e' l'insieme delle firme/digest/fingerprint
* $\mathcal K$ insieme delle chiavi che determinano il fingerprint
* $\mathcal H$ insieme di hash function tale che $\text{ se } k \in K \text{ allora } h_{k} \in \mathcal H$.
* $h_{k}: \mathcal X \to \mathcal Y$: calcola il fingerprint $y$ di $x$.
* $M = |\mathcal Y|$ ed $N = |\mathcal X|$

**Unkeyed**: se $|\mathcal K|=1$ allora l'hash tecnicamente non usa chiavi, in quanto ho solo una hash function $h \in \mathcal H$

$\mathcal F ^{\mathcal X, \mathcal Y}$ e' l'insieme di tutte le funzioni da $\mathcal X$ in $\mathcal Y$. Dunque $\mathcal H \subseteq \mathcal F ^{\mathcal X, \mathcal Y}$.

Voglio che per $h$ siano difficili i seguenti problemi:
**Preimmagine**: date $y$ ed $h$, trovare $x: y = h(x)$.
**Secondapreimmagine**: date $x$ ed $h$, trovare $x': h(x) = h(x') \text{ ma } x \neq x'$.
**Collisione**: data $h$ trovare $x,x'$ tali che $h(x) = h(x') \text{ ma } x \neq x'$.

Si possono risolvere con **bruteforce**, ma ovviamente costruisco una famiglia di funzioni $\mathcal H$ su $\mathcal X$ e $\mathcal Y$ molto grandi

**Random Oracle**: voglio che $h$ si comporti come un oracolo, ossia un libro consultabile con tutte le associazioni $(x, h(x))$. Voglio che l'unico modo per calcolare $y=h(x)$ sia consultare il libro.

**Teorema**: se $h$ simile ad un oracolo (dunque abbastanza randomica), sia $\mathcal X_{0} \subseteq \mathcal X$, allora $\forall x \in \mathcal X_0 \;\; P(x = h(y)) = \frac{1}{M}$ e per ogni $y$.

**Birthday Paradox**:
* Urna con $m$ palline dove ne estraggo n con rimpiazzo.
* $E_{1} := \text{estraggo sempre palline diverse}$
* $E_{2}:= \text{ estraggo almeno una coppia di palline identiche}$

* $P(E_{1}) = \prod_{i=0}^k \frac{i}{m} = e^{-\frac{n(n-1)}{2m}}$ (ATTENZIONE AL -)
* $P(E_{2})= 1 - P(E_{1})$.
* Per $n \in O(\sqrt{ m })$, ed $m \to \infty$ allora $P(E_{2}) \geq \frac{1}{2}$.

**Implicazione del BP**: allora posso trovare collisioni su $h$ con $\mathcal X = \{ 0,1 \}^{128}$ facendo una random walk su $\mathcal X$ generando $\sqrt{ 2^{128} } = 2^{64}$ valori e calcolandone l'hash con alta probabilita.

**Gioco di attacco** sulla resistenza a collisioni:
* sia $b = \{ 0,1 \}$
* L'attaccante deve scrivere in output $m_{0}$ e $m_{1}$ tali che ho una collisione su $h$.
* Voglio che la probabilita con cui $A$ vinca sia negligibile

**Markle Demgard**: voglio costruire hash function collision resistance su messaggi lunghi utilizzando $h$ su messaggi piu corti.
* $h: \mathcal X \times \mathcal Y \to \mathcal X$  con $\mathcal X = \{ 0,1 \}^{\leq L}$ ed $\mathcal Y = \{ 0,1 \}^{m}$ e' la **funzione di compressione**, collision resistance
* Allora posso costruire $h_{MD}$ collision resistance a partire da $h$.
* $PB = 100\dots|<s>$ dove $<s>$ e' la lunghezza del messaggio a cui faccio il padding, in modo da avere un certo numero di blocchi.
* $t_{i}$ sono i valori di chaining, propago l'hash fino all'ultimo blocco e ritorno $t_{m}$
```python
def H_md(h, M1):
	t[0] = IV
	# ossia da M1 ricava i blocchi m0, m1, ...
	# con m_i di lunghezza l
	M = partiziona(M1, l)
	# aggiungi il padding block
	M.append(PB(M))
	for i=1 to m: # m incluso
		t[i] = h(t[m-1], M[i])
	return t[m]
```

**Teorema** $h_{MD}$ collision resistance se e solo se $h$ lo e'.
* sia $A$ attacco su $h_{MD}$ e $B$ attacco su $h$.
* Da $A$ posso ricavare $B$.
* $B$ esegue $A$ per trovare 2 messaggi $x_{1}, x_{2}$ **differenti**, tali che $h_{MD}(x_{1}) = h_{MD}(x_{2})$.
* Allora $h(t_{k-1}, m_{k}) = h(t_{k-1}', m_{k}')$, ci sono 2 casi
	* $t_{k-1} \neq t_{k-1}'$, oppure $m_{k} \neq m_{k}'$, dunque ho una collisione in $h$ e posso ritornarla.
	* $t_{k-1} = t_{k-1}'$ e $m_{k} = m_{k}'$ Dunque procedo a guardare indietro, e dunque i messaggi hanno quantomeno lunghezza identica per via del $PB$
* $h(t_{k-2}, m_{k-1}) = h(t'_{k-2}, m'_{k-1})$ come prima ho i 2 casi.
* Posso srotolare fino a $m_{1}$ e $m_{1}'$ ma per ipotesi trovo una collisione da qualche parte.

**Davies Mayer**: posso costruire $H_{MD}$ a partire da un cifrario a blocchi. 
* $t_{i} = E(m_{i}, t_{i-1}) \oplus t_{i-1}$. Faccio lo XOR per rompere l'invertibilita del cifrario.
* Dove $E$ non puo' essere un cifrario iterato come $AES$, senno' diventa troppo inefficiente
* SHA256 usa la sua funzione di compressione per $E$.
