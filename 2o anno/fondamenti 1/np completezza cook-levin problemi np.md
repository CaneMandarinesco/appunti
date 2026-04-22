Tra i problemi in $NP$ che non riusciamo a dimostrare che stanno anche in $P$, quali sono i più difficili?

> [!note] Problemi $NP\text{-completi}$
> Un problema $\Gamma =$ e' $NP\text{-completi}$ se:
> * $\Gamma \in NP$
> * $\forall \Gamma' \in NP \text{ allora } \Gamma' \leq \Gamma$.
> * $P$ chiusa rispetto a $\leq$

> Se fosse che un problema $NP$ completo stia in $P$, allora $P=NP$.

Ma questo problema $NP$ completo esiste? Ne esiste almeno uno?

# Teorema `cook-levin`
> `SAT` e' un problema NP-Completo

Per **dimostrarlo**, bisogna far vedere che ogni problema in `NP` si riduce a `SAT`, ma in `NP` ci sono tantissimi tipi di problemi, pero' *tutti sono accettati da una macchina di Turing non deterministica in tempo non polinomiale*

Considerando $\Gamma \in NP$ e $L_{\Gamma} \subseteq \{0,1\}^*$ che contiene la codifica delle istanze si del problema

## `NP-completezza`
Il teorema di cook-levin dimostra che qualsiasi problema in $NP$ puo' essere trasformato in tempo polinomiale in una istanza per `SAT`.

> Dati due problemi decisionali: $\Gamma$ e $\Delta$, quando e' che $\Gamma \leq \Delta$?
> Avviene quando: $L_{\Gamma} \leq L_{\Delta}$. 

> [!note] Riducibilita tra problemi
> $\Gamma \leq \Delta$ se esiste $f: \mathfrak T_{\Gamma} \to \mathfrak T_{\Delta}$ tale che:
> * $f \in FP$
> * $x$ e' istanza si **se e solo se** $f(x)$ e' istanza si.

### `3SAT` e' `NP-completo`
Il problema e' molto simile a SAT, infatti:  ed e' una restrizione di SAT.

Dimostriamo che `SAT` e' riducibile a `3SAT`, allora con $<X,f>$ istanza di SAT con $X = {x_{1}, ..., x_{n}}$ e $f = c_{1} \land ... \land c_{m}$ la si puo trasformare in $<X', f'>$ istanza per `3SAT` in modo tale che:

> $f$ *soddisfacibile se e solo se $f'$ soddisfacibile.* 

Per ogni clausola $c_j$ la converto in $D_j$ aggiungendo nuove variabili prese da $Y$:
* se $c_j$ ha **una sola variabile**, quindi $c_{j}= l$, allora $D_{j}= (l \lor y_{j1} \lor y _{j2}) \land ...$ dove le clausole sono tante quanto le combinazioni di $y_{j1} \text{ e } y_{j2}$ possibili. *Per qualsiasi assegnazione di verita di $y$, solo $l$ conta veramente*.
* se $c_j$ ha **due letterali** allora $D_{j} = (l_{1} \lor l_{2} \lor y_{j}) \land (l_{1} \lor l_{2} \lor \lnot y_{j})$, e similmente a quanto detto prima...
* se $c_j$ ne ha 3, e' banale...
* per $c_k$ con piu letterali allora $D_{j} = (l_{1} \lor l_{2} \lor y_{j1}) ...$

per l'ultimo caso, se $l_1$ o $l_{2}$ o  in generale $l_i$ e' vera, allora **esiste una combinazione** di $y$ che rende vera $D_j$. Se tutte le $l$ sono false, **nessuna combinazione** di $y$ puo' rendere vera l'espressione.

> [!note] Struttura di NP: Teorema Ladner
> Se $P \neq NP$, allora non e' che tutti i problemi in $NP-P$ sono $NP\text{-completi}$? **In verita no**: se $P \neq NP$ allora esiste un problema in $NP-P$ che non e' $NP\text{-completo}$.

![[Pasted image 20250829165917.png]]

## Vertex Cover
Dato un grafo $G =(V,E)$ e con $k \in \mathbb N$, esiste un sottoinsieme $V' \subseteq V$ di almeno $k$ nodi tale che e' **completo**?

* $\mathfrak T_{VC} = \{G(V,E), k: G \text{ e' grafo non orientato e } k \in \mathbb N\}$
* $S_{VC}(G,k) = \{ V' \subseteq V\}$
* $\pi_{VC}(G, k, S_{VC}) = \exists V' \in S_{VC}: |V'| \leq k \land \forall (u,v) \in E [u \in V' \lor v \in V]$

$VC$ e' in $NP$? Si, possiamo esibire come certificato un sottoinsieme $V' \subseteq V$, dopo si può verificare in tempo deterministico $O(|V| |E|)$ che questo sia `Vertex-Cover`.

Ed e' $NP\text{-completo}$? Utilizziamo le **strutture**, ossia i **gadget**.

Sia $\phi$ un predicato in 3cnf, definito sulle variabili $X = \{x_{1}, ..., x_n\}$ e $\phi = \{c_{1}, ..., c_{m}\}$ con $c_{j}=l_{j1} \lor l_{j2} \lor l_{j3}$ e $\lnot l_{ji} \in X$ allora vogliamo trasformare $\phi$ in istanza per Vertex Cover.

> [!note] gadget per vertex cover.
> Per ogni *variabile booleana* $x_i$ costruisco un **gadget-variabile** $X_i$ ossia, l'arco $(u_{i}, \lnot u_{i})$.
> Per ogni clausola $c_{i} \in \phi$ costruiamo il **gadget-clausola** $C_j$ formato dagli archi $(v_{j1}, v_{j2}), (v_{j2}, v_{j3}), (v_{j3}, v_{j1})$.
> 
> I due gadget sono collegati dall'arco $(v_{ij}, u_h)$ se $l_{ji} = x_h$ oppure dall'arco  $(v_{ij}, \lnot u_h)$ se $l_{ji} = \lnot x_h$ 

Poniamo $k=n + 2m$, vogliamo dimostrare che $\phi$ e' soddisfacibile solo se esiste un vertex cover per $G$ di cardinalità al più $n + 2m$, perche: 
* per coprire i due del gadget-variabile serve almeno 1 nodo per gadget nel Vertex Cover.
* per coprire ogni nodo del gadget-clausola servono almeno 2 nodi per gadget nel Vertex Cover.

**Supponiamo che $V'$ e vertex-cover di cardinalità** $n+2m$:
* Dunque ho un nodo per ogni gadget-variabile $X_h$, quindi possiamo associare a $V'$ l'assegnazione di verità' $a$ per $X$ in cui $a(x_{h})= \text{ vero }$ se e solo se $u_{h} \in V'$. 
* $a$ soddisfa $\phi$: ogni clausola ha 2 nodi in $V'$, l'altro nodo deve essere coperto da un gadget-variabile $X_h$ che appartiene al vertex dato che non e' coperto dal gadget-calusola.
* Questo nodo che copre l'ultimo nodo escluso dal gadget-clausola, esiste sempre, perche' l'assegnazione di verita e basata sul fatto che $V'$ sia vertex cover 

**Viceversa**, con $\phi$ soddisfacibile e $V'_{X}= \bigcup_{i=1}^{n}[\{ u_{i}\in X_{i}: a(x_{i})= \text{ vero}\} \cup \{\lnot u_{i} \in X_{i} : a(x_{i})= \text{ falso }\}]$.


![[Pasted image 20250830140731.png]]

## IS e CL

> [!note] Independent Set
> $I_{IS} = \{G, k: \text{ G grafo non orientato e } k \in \mathbb N\}$
> $S_{IS} = {I \subseteq V}$
> $\pi_{IS} = \exists I \in S_{IS} : |I| \geq k \land \forall u,v \in I[(u,v) \notin E]$.

IS e' in $NP$, poiche usando come certificato $I$, posso verificare in tempo polinomiale in $O(|E| |V|^2)$ se $I$ e' independent set.

Il problema e' $NP$ completo, poiche $V'$ e' vertex cover in $G$ se e solo se $V - V'$ e' independent set, si dimostra intuitivamente che se $V'$ vertex cover allora i nodi nella differenza non hanno archi che li collegano e viceversa.

La funzione che riduce vertex cover a independent set e': $$
f(G,k) = <G = (V,E), |V|-k>
$$

> [!note] Clique
> * $\mathfrak T_{CL} = \{G \text{ grafo non orientato e } k \in \mathbb N \}$.
> * $S_{CL} = {C \subseteq V}$
> * $\pi_{CL} = \exists C \in S_{CL}: |C| \geq k \land \forall u,v \in C [(u,v) \in E]$


Possiamo trasformare $CL$ in una istanza di $IS$, esibiamo una riduzione di $IS$ a $CL$, infatti:
> $I \subseteq V$ e' un insieme indipendente per $G$ se e soltanto se $I$ e' clique in $G^C$, ossia il grafo composto dagli archi che non stanno in $G$.

### `DS`
Il `Dominating Set` e' l'insieme di nodi tali che, tutti quelli che non stanno in $D$ hanno almeno un'arco verso un nodo in $D$.

Per dimostrare che e' $NP\text{-completo}$ riduciamo $VC$ a $DS$


### `Ciclo Hamiltoniano`, $HC$
Dato $G=(V,E)$ si vuole sapere se esiste un ciclo che tocca tutti i nodi in $G$ **una ed una sola volta**.

> Il problema e' $NP$ completo ma non dimostriamo perche

### `Path Hamiltoniano`, $HP$
Dato $G = (V,E)$ e due nodi $s,t$ si vuole sapere se esiste un path da $s$ a $t$ che tocca tutti i nodi in $G$ **una ed una sola volta**.

$HC \leq HP$, infatti dato un grafo $G$ istanza per $HC$, tale che $s,t \notin V$, allora posso costruire $G'$ istanza per $HP$ in questo modo:
* $G'=(V', E')$ dove $V' = V \cup \{s,t\}$ ed $E' = E \cup \{(s,u)\} \cup \{(u',t): (u, u') \in E\}$.

Dunque se in $G$ esiste un ciclo Hamiltoniano, posso attaccare $s$ ad un nodo qualsiasi di $V$, diciamo $u$, e $t$ ai nodi adiacenti ad $u$ in modo da ottenere un path hamiltoniano. Il ciclo Hamiltoniano coinvolge i nodi $u$ e quelli adiacenti a $u$, dunque essendo $t$ collegato ai nodi adiacenti ad $u$ esiste sto cazzo de cammino.

### `Long Path`, $LP$
Dato $G=(V,E)$ e due nodi $s,t \in V$ e $k \in \mathbb N$ voglio sapere se esiste un path da $s,t$ che usa almeno $k$ archi.

$HP$ e' riducibile ad $LP$ in quanto $LP$ e' un caso piu generale di $HP$: infatti $HP$ e' un'istanza di $LP$ dove $k=|V|$.

### `Traveling Salesman Problem`, $TSP$
Con $G = (V,E)$ grafo non orientato ma **completo** e pesato con $w: E \to \mathbb N$ e $k \in \mathbb N$, voglio sapere se esiste un ciclo Hamiltoniano il cui peso degli archi totale e' minore di $k$.

$HC$ e' riducibile a $STP$, infatti basta prendere $E$ e aggiungere gli archi che servono a far diventare $G$ completo, poi creiamo $w$ la funzione di peso tale che $\forall e \in E' t.c. e \in E$ allora $w(e) =1$ e per i restanti archi $w(e) =2$, e poniamo $k=|V|$, allora se esiste un ciclo hamiltoniano in $G$, se e solo se $G'$ soddisfa $TSP$.

### `COL` e `k-COL`.
...


# costanti!
Prendiamo il problema $3SAT[k]$, ossia $3SAT$ ma $|X|=k$, allora verificare tutte le assegnazioni di verita possibili per $f$ ha costo $O(2^k|f|)$.