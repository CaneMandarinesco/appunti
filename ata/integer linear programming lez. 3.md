## Set Cover
* **Input**: $U$ universo, $S$ collezione di sottoinsiemi in $U$.
* **Output**: $C \subseteq S$ che copre $U$.
* **misura**: $\sum_{S\in C}c(S)$.

## Integer Linear Programming
* **variabili**: per ogni sottoinsieme ho $x_{S} \in \{ 0,1 \}$
* **scegliere un cover**: vuol dire scegliere quali $x_{S}$ sono 0 e 1 ($S$ preso o non preso)
* **vincolo**: $\sum_{S: e \in S} x_{s} \geq 1$. 
* **cos'e' un cover?** un vettore di $x_{S}$
* **voglio minimizzare**: $\sum_{S \in \mathcal S} c(S) x_{S}$

**Integer Linear Programming**: $x_{S}$ e' limitata su interi.

**Come si usa questa formulazione**? 
* **rilassa**: $x_{S}$ diventa $x_{S} \in [0,1]$.
* **vincolo ridondante**: $x_{S} \leq 1$. Non conviene mai mettere $x_{S} \geq 1$.

**LP-Relaxation**:
* **minimizzare**: $\sum_{S \in \mathcal S} c(S) x_{S}$
* **vincolo**: $\sum_{_{S: e \in S}} x_{S} \geq 1$ con $x_{S} \geq 0$

Sia $OPT$ ottimo di Set Cover e $OPT_{f}$ ottimo di LP-Relax:
$$OPT_{f} \leq OPT$$

**Es slides**:
* con LP-Relax poso prendere una frazione dell'insieme
 * $OPT=2$ e $OPT_{f} = 1.5$.
 
**Algoritmo LP-Rounding**:
1. scegli tutti gli $S$ t.c. $x_{S} \geq \frac{1}{f}$.

LP-Rounding e' $f \text{-apx}$:
 1. **scegli** $e$ dentro al piu $f$ insiemi.
 2. **la somma** degli insiemi che contengono $e$ e' almeno 1: $f \times \frac{1}{f}$.
 3. **copertura**: allora ogni $e$ e' coperto
 4. **costo della soluzione**: aumenta di un fattore $f$
 5. $\text{costo del cover} \leq f OPT_{f} \leq f OPT$

## Weighted Vertex Cover
* **Input**: $G=(V,E)$ con $c(v)$ peso di ogni nodo
* **Soluzione ammissibile**: $U \subseteq V$ tale che ogni arco $(u,v)$ e' coperto.
* $f=2$: ogni nodo sta in due insiemi.
* **Approssimazione**: e' $2\text{-apx}$


**esempio stretto**
* **ipergrafo**: $V_{1}, \dots, V_{k}$, disgiunti, di cardinalità $n$ ognuno.
* **iperarco**: sono $n^k$. 
* Un'iperarco corripsonde a scegliere $x_{1} \in V_{1}, x_{2} \in V_{2}, \dots, x_{k} \in V_{k}$.
* $f=k$: ogni nodo e' contenuto da $k$ iperarchi.
* $OPT_{f}=n$

## Primale duale
* **sol primale**: upperbound al duale
* **sol duale**: lowerbound al primale.
* **ottimo**: quando $\text{sol primale} = \text{sol duale}$. Esiste sempre e si incontrano sempre.

## Dual fitting e Set Cover
* Prendo la LP-Relax di Set Cover
* **duale**: lo costruisco sul primale, ossia il Relax di set cover.

**Duale**:
* **massimizzare** $\sum_{e\in U} y_{e}$
* **bound**: $\sum_{e: e \in S} y_{e} \leq c(S)$, $y_{e} \geq 0$
* $OPT_{duale} \leq \text{OPT}_{f} \leq \text{OPT}$


**strategia greedy**: calcola la cost effectiveness di ogni insieme e prendi quello minimo.
* $\alpha = \frac{cost(S)}{\text{item nuovi che copre } S}$. e' la cost effectiveness
* per ogni oggetto preso il suo costo e' $\alpha$