### Problema 9.2
Considera il problema $\text{2ColOppure3Col}$ che decide se $G=(V,E)$ e' $\text{2-Colorabile}$ oppure $3-Colorabile$. Formalizzare il problema con $<I,S,\pi>$ e decidere la classe di complessità più piccola che la contiene.

> **OSS**: se $G$ e' $\text{2-Colorabile}$, allora e' anche $\text{3-Colorabile}$, basta utilizzare solo  2 dei 3 colori a disposizione.

> **OSS**: se $G$ e' $\text{3-Colorabile}$ allora e' anche $\text{2-Colorabile}$ **oppure**  $\text{3-Colorabile}$.

Quindi il problema coincide con la $\text{3-Colorabilita}$ che e' $NP$ completo. Puo' essere formalizzato come:
* $I= \{<G=(V,E)>\}$
* $S = \{<c: V \to \{1,2,3\}>\}$ dato che $c$ si può' usare anche per la $\text{2-Colorabilita}$.
* $\pi = \exists c \in S: c \text{ e' 3-colorazione di G}$ (che puo' essere formalizzato in maniera migliore...)

> Si vuole far vedere che il problema e' identico a $\text{3-Col}$ proprio perche' in $3-Col$ e' ammissibile una colorazione che usa solo due colori.

### Problema 9.4
Considera $\text{Dominating Set}$ dove si vuole decidere se $\exists D$ dominating set per $V-D$ dove $|D| \leq k \in \mathbb N$.
1. Formalizzare il problema e dimostrarne l'appartenenza a $NP$.
2. Con $f$ riduzione polinomiale da $VC$ a $\text{Dominating Set}$ dimostra che puo' essere calcolata in tempo polinomiale in $|G|$ e $k$e che se $G$ ammette un $\text{Vertex Cover}$ di al piu $k$ nodi, allora ammette anche un $\text{Dominating Set}$ di al piu $k+1$ nodi.

**Innanzitutto** $\text{DS} = <I,S,\pi>$, dove:
* $I = \{<G = (V,E), k>: k \in \mathbb N\}$.
* $S = \{D \subseteq V\}$
$\pi = \exists D \in S: [|D| \leq k \land \forall x \in V-D [\exists e=(x,y) \in E: y \in D]]$.

Il problema e' in $NP$ in quanto usando come certificato $D \subseteq V$ allora posso verificare per ogni nodo in $V-D$ se esiste l'arco che lo collega a un nodo in $D$ in tempo $O(|V| |E|) = O(|G|)$.

Oppure, come ha fatto la prof: e' in $NP$ in quanto $D \subseteq V$ puo' essere generato non deterministicamente in tempo $O(n)$. Con $V = {v_{1}, \dots, v_{n}}$ l'insieme dei nodi e considerando l'albero delle computazioni di prodondita $n$ e grado di non determinismo $2$ ad ogni livello corrisponde un sottoinsieme $X$ di ${v_{1}, \dots, v_{j}}$ nodi in $v$. Una volta generato posso verificarlo in $O(kn|E|)$ .

La riduzione per essere calcolata richiede:
$O(|E| + |V|)$ con un semplice algoritmo polinomiale, e rimane da vedere la riduzione da $VC$ a $\text{Dominating Set}$.

Ora dimostriamo che la riduzione e' corretta:
* $a$ e' un nodo aggiuntivo che viene usato per dominare i nodi che sono isolati.
* si vuole estendere il grafo aggiungendo per ogni arco $e$ il nodo $v_e$ collegato ai medesimi nodi dell'arco $e$.
Mettendo $a$ in $D$ dominiamo tutti i nodi isolati in $G$, e con $V'$ vertex cover, dominiamo tutti gli altri nodi in $G'$.

### Problema 9.5
Con $L$ problema decisionale che consiste nel decidere dati $A=\{a_{1,}\dots, a_{n}\} \subseteq \mathbb N$ e $k \in \mathbb N$, se $A$ contiene un sottoinsieme la somma dei cui elementi sia al piu $k$.

Formalizzare con $<I,S,\pi>$, dimostrare l'appartenenza in $NP$ e mostrare un'algoritmo deterministico che decida il problema.

* $I_{L}= \{<A,k>: A = \{a_{1}, \dots, a_{n}\} \subset \mathbb N, k \in \mathbb N\}$.
* $S_{L(<A,k>)}= \{A' \subseteq A\}$.
* $\pi_{L}= \exists A' \in S_L(<A,k>): \sum\limits_{a \in A} a \leq k$.
Per provare che sia in $NP$ forniamo un'algoritmo non polinomiale:
```
A = insieme vuoto
i = 0
while( i < n)
	scegli se inserire a_i in A.
	i += 1

i = 0
s = 0 
while (i < |A|)
	s += a_i
if(s == k): q = q_a
else: q = q_r
```

### Problema 9.7
Con $L_1$ e $L_2$ due linguaggi tali che:
* $|L_{1}-L_{2}| = \{a_{1}, a_{2}, a_{3}\}$
* $|L_{2}- L_{1}| = \{b_{1}, b_{2}\}$
* $L_{2} \in NPC$.
Dimostrare che in queste ipotesi, allora $L_{1} \in NPC$.
Posso costruire la riduzione da $f: L_{1} \to L_{2}$ **letteralmente inventandola**.

### Problema 
