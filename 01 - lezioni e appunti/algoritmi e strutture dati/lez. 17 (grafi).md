>[!note] grafo (non orientato)
> un grafo consiste di $G=(V,E)$:
> * $V$ vertici
> * $E$ coppie di vertici, **archi**. (non sono ordinati). $(A,B) = (B,A)$

Nel caso del problema di Koningsberg si dice **multigrafo**: ho più coppie di vertici uguali, $E$ e' un **multinsieme** con archi paralleli.

> `clicque`: un grafo in cui tutti i nodi sono collegati tra loro

>[!note] grafo orientato o diretto
> $E$: coppie di vertici ordinati: $(A,B) \neq (B,A)$

### Terminologia
* $n=\#\text{ di vertici}$
* $m = \#\text{ di archi}$
* $u \text{ e } v$ **adiacenti**:
	*  $(u,v)$ e' **incidente** a $u \text{ e } v$, e sono gli **estremi**
* $\sigma(u)= \text{ grado di } u$.
	* per i grafi orientati devo scegliere se voglio il grado **uscente** o **entrante**.
* $G = max_{v \in V}\{ \sigma(v) \}$
	* per i grafi orientati devo scegliere se voglio il grado **uscente** o **entrante**.

#### Proprietà
Cosa succede se sommo i gradi di ogni nodo?
$$
\sum_{v \in V} \sigma(v) = 2m
$$
Ottengo **due volte il numero di archi.** Il numero di nodi e' **sempre pari**.
Per i grafi orientati, la sommatoria del numero di archi entrati/uscenti e' $m$.

* **cammino**: sequenza di nodi connessa da archi.
* **lunghezza di un cammino**: numero di archi in un cammino
* **distanza**: lunghezza del cammino più corto.
* $G$ e' **connesso** se esiste sempre per ogni coppia di vertici un cammino che li collega.
* **ciclo**: cammino che va da un vertice a se stesso
* **diametro**: distanza massima tra due nodi.
	* $max_{u, v \in V} \text{dist}(u,v)$
	* se e' connesso allora ho $\text{diametro} = 1$

>[!note] grafi pesati
> $G = (V,E,w)$.
> Ogni arco in $E$ ha un peso calcolato dalla funzione $w$.
> * il cammino più corto e' quello che a lunghezza (somma di tutti i $w(e \in E)$) più piccolo.

* grafo **sconnesso**: $E=\emptyset$
* numero di archi massimi per $n$ nodi: $m = |E| = \frac{n(n-1)}{2}$. (grafo senza cappi o archi paralleli).
* **albero**: un grafo connesso ed aciclico, ha $n-1$ archi. $|E| = |V|-1$.
	* c'e' un nodo che ha almeno grado $1$. Se fossero tutti di grado $>1$ **allora non sarebbe aciclico**.
* Se $G$ e' connesso, allora: $m=\Omega(n)$ e $m=O(n^2)$

### Cammino Euleriano
Un ciclo/cammino e' detto Euleriano se attraversa tutti gli archi una e una sola volta. il grafo della cita di Koninghsbergh ha almeno un cammino?

#### Teorema di Eulero
Un grafo ammette un ciclo Euleriano se e solo se i nodi hanno grado pari tranne due (i nodi di grado dispari sono gli estremi del cammino). Il problema dei 7 ponti non ha soluzione quindi.








