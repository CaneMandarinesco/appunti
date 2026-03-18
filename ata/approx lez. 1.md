$\alpha \text{-approx}$: e' un algoritmo polinomiale che trova una soluzione distante di un fattore $\alpha$ dall'ottimo.

**minimizzazione**: $\alpha \geq 1$. per ogni istanza $x$ con soluzione $s$ trovata dall'algoritmo $\alpha \text{-apx}$ ha costo $\text{cost}(s) \leq \alpha \text{OPT}(x)$

**massimizzazione**: $\text{cost}(s) \geq \alpha OPT(x)$.

## min vertex cover
**input**: $G=(V,E)$
**output**: $U: U \subseteq V$ tale che per ogni $(u,v) \in E$, $u \lor v \in U$.
**misura**: minimizzare $|U|$.

**sol ammissibile**: $U=V$.

## matching e vertex cover
**input**:  $G=(V,E)$ 
**output**: $M$ matching per $G$. Un sottoinsieme di archi $M \subseteq E$ in modo che ogni coppia di archi in $M$ non condividono nodi.

**misura**: $M$ massimale. Tale che se aggiungo ad $M$ un arco in $E \setminus M$ allora $M$ non e' piu un matching

**Alg 1.2** *Vertex Cover*:
1. trova un matching in $G$
2. ritorna i nodi che fanno match

**Perche?**: un matching copre tutti gli archi!

**Alg 1.2** e' $2\text{-apx}$:
1. **Lower Bounding Scheme**: $OPT \geq |M|$. L'ottimo copre tutti gli archi di $M$, dato che prendo un'estremo per ogni arco.
2. $|U|=2|M|\leq 2OPT$. Per ogni arco 2 nodi (gli archi sono disgiunti), dunque prendo $2|M|$ nodi.

**esiste un'analisi migliore?**: se prendo un grafo bipartito completo, l'algoritmo trova un vertex cover distante di un fattore $2$. Vuol dire che Alg 1.2 e' proprio $2\text{-apx}$.

**usando un'altro lower bounding scheme posso migliorare l'algoritmo**? se prendo $K_{n}$ (grafo completo di $n$ nodi), con $n$ dispari, allora la grandezza di un qualsiasi maximal matching e' $\frac{n-1}{2}$ (poiche' completo) e $OPT=n-1$.

Esatto, hai centrato il punto filosofico (e matematico) della questione: **la bontà di un algoritmo di approssimazione non dipende da quanto è fortunato su un singolo caso, ma dalla forza del suo "certificato" (il lower bound).**

**esiste un'lower bounding scheme migliore per VC che mi da un algoritmo di apx migliore?** boh!

**Teorema**: assumendo **unique games conjecture** vera, allora esiste un $\alpha \text{-apx}$ con $\alpha\leq 2$.


## Set Cover
* **Input**: $U$ universo, $S$ collezione di sottoinsiemi in $U$.
* **Output**: $C \subseteq S$ che copre $U$.
* **misura**: $\sum_{S\in C}c(S)$.

**Greedy Set Cover**: scegli il migliore insieme (**per cost effectiveness**) e rimuovi gli elementi coperti finché' non li copri tutti.

**cost-effectiveness** di $S$, con $C$ elementi già coperti: $c(S)/|S-C|$. costo con il quale copro i nuovi elementi. 

![[Pasted image 20260310101743.png]]
![[Pasted image 20260310101904.png]]
1. Sceglie per primo $S$ di costo $3$ con 4 nodi, dunque $\alpha=\frac{3}{4}$.
2. Poi continua...

**Analisi**:
1. Prendi elementi di $U$ in ordine di selezione dall'algoritmo: $e_{1}, \dots,e_{k}$
2. **Lemma**: se prendo $e_{k}$, allora $\text{price}(e_{k}) \leq \frac{\text{{OPT}}}{n-k+1}$.
3. **Dim**: quando prendo $e_{k}$ ho scelto un'insieme. 
	1. Se immagino di aggiungere la soluzione ottima, allora il costo e' $\frac{OPT}{|C'|}$.
	2. $C'$ insieme che contiene i nodi rimanenti da coprire. 
	3. La cost effectiveness dei sottoinsiemi di $U$ e' minore di $\frac{OPT}{|C'|}$
	4. $|C'| \geq n-k+1$ perche' se seleziono l'elemento $k$ in $e_{1}, \dots , e_{k}, \dots, e_{n}$ allora seleziono almeno $n-k+1$  elementi.

**greedy set cover e '** $H_{n}\text{-apx}$: l'algoritmo trova una soluzione ammissibile con costo $\sum_{k=1}^n \text{price}(e_{k}) \leq \sum_{k=1}^n \frac{OPT}{n-k+1} \leq H_{n}\text{OPT}$

![[Pasted image 20260310103830.png]]