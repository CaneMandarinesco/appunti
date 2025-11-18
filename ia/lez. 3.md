**Definire il problema della ricerca**: 
(atomica, misura di performance, ambiente noto e deterministico)

**Formulazione di un problema**: 
* stato iniziale 
* azioni possibili
* $\text{stato} \times \text{azione} \to \text{stato}$
* goal test: $\text{stato} \to \{T,F\}$
* path cost: $g(n)$

**Definire il Search Tree**:
* search tree: rappresenta tutte le sequenze di azioni possibili a partire dalla radice, ossia lo stato iniziale
* ogni nodo rappresenta uno stato
* gli archi rappresentano la transizione da uno stato all'altro tramite l'applicazione di un'azione.
* frontiera: nodi in coda per essere espansi
* explored set: nei grafi ci possono essere dei loop, si evitano grazie all'explored set 

> **Nota**: i nodi corrispondono a stati!!!

**Tree-Search**:
```c
function treeSearch(problema){
	frontiera = {makeNode(problema.statoIniziale)}
	loop do{
		if(frontiera.empty) return failure
		node = frontiera.pop()
		if(problema.goalTest(node)) return sol(node)
		
		# nel senso: espandi il nodo (simulando le azioni), aggiungi le nuove fogli alla frontiera!
		for(child in node): frontiera.push(child)
	}
}
```
> `frontiera.pop` estrae un nodo a caso

**Graph-Search**: similmente a tree search ma introduco **explored set** 
```c
function graphSearch(problema){
	frontiera = {makeNode(problema.statoIniziale)}
	exploredset = {}
	loop do {
		if(frontiera.empty) return failure
		node = frontiera.pop()
		if(problema.goalTest(node)) return sol(node)
		exploredset.add(node)
		
		# come prima, espandi il nodo, aggiungendo le nuove foglie i cui stati non sono stati visitati in frontiera.
		for(child in node){
			if(child not in exploredset) 
				frontiera.push(child)
		}
	}
}
```

**tipi di code usate**:
	* `FIFO`: rimuove il nodo piu' recente
	* `LIFO`: rimuove il nodo piu' vecchio inserito
	* `CodaPriorita`: rimuove il nodo in base al valore `f(n)`

**definizione di nodo**: 
* `N.stato`: lo stato associato al nodo
* `N.action`: azione che lo ha generato
* `N.parent`: il padre che genera il nodo
* `N.cost`: $g(n)$ fino alla radice!!!

> **Nota**: ricordati il costo

**Analizzare performance del problem solving**: il problem solving 

**BFS**: (che coda usa???, espando nodo e poi testo i figli) usa la coda `FIFO`
```c
function BFS(problema){
	n = makeNode(problema.statoIniziale)
	if(problema.goalTest(n)) return sol(n)
	FIFO frontiera = {n} tipo
	exploredset = {}
	loop do {
		if(frontiera.empty()) return failure
		n = frontiera.pop()
		exploredset.add(n)
		for(child in n){
			if(problema.goalTest(child)) return sol(child)
			if(child not in exploredset)
				frontiera.push(child)
		}
	}
	return failure
}
```


**UC**:
```c
```

