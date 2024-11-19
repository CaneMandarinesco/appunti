## retta
> come faccio a descrivere tutti i punti di una retta che non passa per l'origine?

fisso:
* $O,\vec{i},\vec{j},\vec{k}$
* $r = \text{retta}$  ($\in A^{2/3}$)
* $P_{0} \in r$

prendo una retta $r_{0} \parallel r$ che passa per $O$ (origine).  
abbiamo che un punto $P$ in $A^2$ si trova nel piano se e solo se il il *segmento* $\overline {P_{0}P}$ e' parallelo a $r_{0}$ e quindi se e solo se (dato che sono paralleli):
$$
\vec{\mathrm{OP}} - \vec{\mathrm{OP_{0}}} = t\cdot  \vec{\mathrm{OQ}}
$$
ovvero:
$$
\vec{\mathrm{OP}} = \vec{\mathrm{OP_{0}}} + t \cdot  \vec{\mathrm{OQ}}
$$
anche detta: **equazione vettoriale della retta**.

> [!note] NOTA
>questo significa che $\vec{OP} - \vec{OP_{0}}$ si trova nella retta $r_{0}$!

![[Pasted image 20240314112344.png]]

> [!note] OSS.
> nota che il punto $Q$ non coincide con la proiezione di $P$ su $r_{0}$!

>[!note] OSS.
>$OP_{0}$ e' un valore **costante**: lo abbiamo fissato noi

> il modello che abbiamo usato per descrivere l'equazione **degenera** quando $P=P_{0}$, ovvero abbiamo un punto: non ha molto senso che un punto si parallelo ad un altro punto, c'e' bisogno di "puntualizzare" altri concetti!

con $\vec{\mathrm{OQ}} = \vec{v}$ diciamo che:
> $\vec{ v}$ e' il **vettore direttore** (decide la direzione!)

## retta per due punti
fissati come prima:
* $P_{0}$ e $P_{1}$ tali che: $\overline{P_{0}P_{1}} \parallel r$ ($\forall r \in A^{2/3}$)
* ecc...

sappiamo che, facendo riferimento al disegno sopra usando $P_{0} \text{ e } P_{1}$:
$$
\vec{\mathrm{P_{0}P}} = \vec{\mathrm{OP_{1}}} - \vec{\mathrm{OP_{0}}}
$$
e quindi similmente a prima:
$$
OP = \vec{\mathrm{OP_{1}}} + t \cdot (\vec{\mathrm{OP_{2}}} - \vec{\mathrm{OP_{1}}})
$$

> [!warning] attenzion!
> queste formule sono definite equazioni, ma il nome e' fuorviante! dato che non ci sono propriamente delle incognite da trovare. piuttosto l'equazione va intesa come una funzione la cui immagine descrivere la retta.

ora possiamo studiare:
## intersezione tra due rette
* $r: \vec{\mathrm{OP_{t}}} = \vec{\mathrm{OP_{r}}} + t \cdot  \vec{v}$
* $r': \vec{\mathrm{OP'_{t'}}} = \vec{\mathrm{OP'_{O}}} + t' \cdot \vec{v'}$

abbiamo che $P \in r \cap r'$ se e solo se:
$$
P_{t} = P_{t'}'
$$
ma per quale $t, t'$ e' vero? dobbiamo risolvere:
$$
\vec{\mathrm{OP_{O}}} + t\cdot  \vec{v} = \vec{\mathrm{OP_{O}'}} + t' \cdot   \vec{ v'}
$$
>[!note] NOTA
> la parte a sx dell'equazione rappresenta una retta che passa per $P$ e quella a dx una retta che passa per $P'$

dove: $t, t'$ sono una coppia ordinata, $(t,t') \in R^2$. quindi abbiamo scoperto che due rette si intersecano solo se *sono sullo stesso piano dei vettori direttori*.  

> inoltre se non sono parallele si intersecano sempre. solo in $V^3$ possono essere **sghembe**.

## rette sul piano
* $\pi \in A^3$
* $P_{o} \in \pi$ -> e' il piano a cui appartiene $P_{O}$
* $\forall v \in V_{o}^3 \to \exists! (s,t,d) \in R^3: s \vec{i} + t \vec{u} + d \vec{k} = \vec{v}$

similmente a prima:
$$
\vec{\mathrm{OP}} = \vec{\mathrm{OP_{0}}} + s \cdot  \vec{v} + t \cdot  \vec{w}
$$
dove $\vec{v} \text{ e } \vec{w}$ sono: **vettori di giacitura del piano**

cosi posso rappresentare tutti i punti di un piano contenuto nello spazio, mentre prima con le rette potevo rappresentare tutti i punti di una retta contenuta in un piano (formato da $r_{O} \text{ e r}$)

> ricorda che: per 3 punti non allineati passa uno e solo un piano.

