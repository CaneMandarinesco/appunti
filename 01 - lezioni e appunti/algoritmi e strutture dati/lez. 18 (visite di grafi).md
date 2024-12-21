Rappresentazione di un grafo non diretto:
* Matrice di adiacenza di grandezza $n \times n$.
* Lista di adiacenza: $O(m + n)$

Ora voglio implementare la ricerca di archi adiacenti in $v$ usando le matrici:
* $O(n)$ per ottenere tutti gli adiacenti
* $O(1)$: per ottenerene uno specifico 

Se uso le liste di adiacenza:
* $O(\delta(v))$
* $O(min\{ \delta(u), \delta(v) \})$

### Visita
* **BSD**: in ampiezza. Dato un grafo $G$ e un nodo $s$ trova tutte le **distanze/cammini minimi** da $s$ verso ogni nodo $v$.
	* **web crawling**
	* **social networking**
	* **network broadcast**
	* **garbage collection**
	* **model checking**
	* **puzzle**
* **DFS**: in profondità

Esercizietto, cubo di Rubik $2 \times 2 \times 2$. Uso il grafo delle configurazioni: un vertice per ogni possibile configurazione del cubo, gli archi indicano se posso passare da una configurazione all'altra con una mossa.

Questo grafo ha: $\# \text{ vertici} \leq 8! \times 3$^8. Implemento la visita in ampiezza:

```python
algoritmo visitaBFS(vertice s)
	rendi tutti i vertici non marcati
	T = albero formato da un solo nodo s
	Coda F
	marca il vertice s; dist(s) = 0
	F.enqueue(s)
	while(not F.isempty()) do
		u = F.dequeue()
		for each(arco(u,v) in G) do
			if (v non e' marcato) then
				F.enqueue(v)
				marca il vertice v; dist(v) = dist(u) + 1
				rendi u il padre di v in T
	return T
```

La complessità e' $O(n^2) + O(n) = O(n^2)$ se uso le matrici, altrimenti con liste di adiacenza diventa: $O(m+n) + O(m) + O(\delta(u)) = O(m+n) = O(n^2)$ con $m \leq \frac{n(n-1)}{2}$, per $m = o(n^2)$ la rappresentazione e' temporalmente più efficiente. 


### Teorema
Per ogni nodo $v$ il livello di $v$ nell'albero BFS e' pari alla distanza di $v$ dalla sorgente $s$.

#### dimostrazione informale...

### Visita in profondità
Utile per problemi tipo labirinti. Ho un grafo e devo esplorare le sue connessioni. 

```python
procedura visitaDFSRicorsiva(vertice v, albero T)
	marca e visita il vertice v
	for each(arco(v,w)) do
		if (w non e' marcato) then
			aggiungi l'arco (v,w) all albero T
			visitaDFSRicorsiva(w,T)

algoritmo visitaDFS(vertice s)
	T = albero vuoto
	visitaDFSRicorsiva(s,T)
	return T
```



