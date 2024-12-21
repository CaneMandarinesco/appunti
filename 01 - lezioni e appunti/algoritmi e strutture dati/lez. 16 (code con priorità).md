Operazioni di una coda con priorità:
* `findMin()`: trova l'elemento con chiave minima
* `insert(elem e, chiave k)`
* `delete(elem e)`
* `deleteMin()`: estrai l'elemento con chiave minima

Puo avere altre operazioni aggiuntive:
* `increaseKey`
* `decreaseKey`
* `merge`

Applicazioni: per gestire risorse condivise, gestire priorita' per l'uso di una risorsa. Algoritmi per trovare il cammino minimo in un grafo o di ordinamento.

Implementazioni possibili:
* `d-heap`: generalizzazione degli heap binari visti nell'heap sort.
* `Heap binomiali`: figo!!!!
* `Heap di Fibonacci`: migliore possibile.

### d-heap
> nota: L'heap sort nel suo algoritmo estrae sempre il massimo.

L'albero `d-heap` ha le seguenti proprieta:
* **Struttura**: e' completo fino al penultimo livello, con foglie compattate a sinistra.
* **Contenuto informativo**: ogni elemento ha una chiave, presa da un dominio ordinato
* **Ordinamento** parziale (inverso) dell'heap (min-heap): `chiave(v) >= chiave(parent(v))`. Nel max-heap la proprietà e' ribaltata.

All'**aumentare** del valore di `d`:
* l'altezza dell'albero diminuisce
* ogni nodo ha sempre più figli.

Conseguenze delle proprietà:
* Un `d-heap` ha altezza: $\Theta(\log_{d}n)$
* La radice contiene l'elemento con chiave minima
* Si può rappresentare con **vettori posizionali**.

Procedure per aggiustare un albero che non e' un heap:
```python
procedura muoviAlto(v):
	# scambia verso l'alto finche chiave(v) < chiave(padre(v))
	while(v != radice(T) and chiave(v) < chiave(padre(v))) do
		scambia v e padre(v) in T

# come fix-heap!
procedura muoviBasso(v):
	repeat
		sia u il figlio di v con la minima chiave(u), se esiste
		if(v non ha figli o chaive(v)<=chiave(u)) break
		scambia v e u in T
```

* `muoviAlto`: $O(\log_{d}n)$
* `muoviBasso`: $O(d \cdot \log_{d}n)$

Ora devo implementare le altre procedure:
* `findMin`: e' la radice, $O(1)$
* `insert(elem e, chiave k)`: ho un solo posto dove posso mettere il mio elemento, le fogli sono compattate a sinistra e l'albero e' completo fino al penultimo livello. Inserisco in tempo $O(1)$ e poi devo fare `muoviAlto` per aggiustare l'albero.
* `delete(elem e)`: prendo sempre l'ultima foglia e la scambio con `e`. Poi faccio o `muoviAlto` o `muoviBasso`.
* `decreaseKey(elem e, chiave d)`: dopo che decremento può essere che devo fare `muoviAlto`
* `increaseKey(elem e, chiave d)`: dopo che incremento può essere che devo fare `muoviBasso`
* `merge(coda c1, coda c2)`: non può essere implementato in modo fast. Ci sono due modi:
	1. **Distruggo le due code** e creo un nuovo heap che e' l'unione degli elementi. 
	2. **Inserimenti ripetuti**: gli elementi della *coda più piccola vengono inseriti nella coda più grande*.

### Merge sfondando le due code
$$
T(n) = d T\left( \frac{n}{d} \right) + O(d \log_{d}n)
$$
con $n=|c_{1}| + |c_{2}|$. E' come la procedura `heapify`, per ogni sotto-albero della radice chiamo `muoviBasso` sulla radice.

### Merge con inserimenti
Ora con $k=min\{c_{1},c_{2}  \}$, eseguo $O()$...


## Heap Binomiali

Entrambe le due procedure per il merge costano $O(n)$ al caso peggiore. Tutte le altre eseguono in tempo **logaritmico**. Possiamo fare di meglio.

E' una struttura piu complicata del `d-heap`. Un'**albero binomiale** e' definito ricorsivamente:
* $B_{0}$ ha un solo nodo
* $B_{1}$ e' alto 1 e ha 2 nodi.
* $B_{i+1}$ e' ottenuto prendendo $B_{i}$, e alla radice attacco un'altro $B_{i}$.

Proprietà:
* **numero di nodi**: $2^i$
* **altezza**: $i$
* il **grado** della radice e': $i$
* $B_{0},\dots,B_{h-1}$ sono sotto-alberi di $B_{h}$

Ora, un *heap binomiale e' una foresta di alberi binomiali*.
Il numero di sotto-alberi binomiali per $n$ nodi si deduce dalla rappresentazione in binario. Al piu ci sono $\log n$ alberi binomiali, ognuno ha altezza $O(\log n)$.

L'heap binomiale si realizza con liste di puntatori.

**Procedura ausiliaria**, serve a fondere due $Bi$ (ristrutturare, aggiustare) che hanno stesso numero di elementi:
```python
procedura ristruttura():
	i=0
	while(esistono ancora due Bi) do:
		si fondono i due Bi per formare un alero Bi+1.
		La radice con chiave piu piccola diventa genitore della radice con chiave piu grande
		i=i+1
```

Per ogni iterazione, diminuisce il numero di alberi. Viene eseguito in tempo lineare rispetto al numero di alberi.

```python
class HeapBinomiale implementa CodaPriorita:
dati:
	una foresta H con n nodi, ciascuno contenente un elemento di tipo elem e una chiave di tipo chiave presa da un universo totalmente ordinato.

```

* `deleteMin`: elimino la radice con valore piu piccolo. Ottengo `d` alberi nuovi che aggiungo alla foresta. Eseguo la procedura **ristruttura**.
* `decreaseKey`: come prima, faccio `muoviAlto`
* `delete`: chiamo `decreaseKey(e,-infinito)` e poi `deleteMin`.
* `increaseKey`: prendo `e`, lo elimino, poi inserisco la chiave aggiornata???
* `merge`: basta fare `ristruttura` sulla foresta fatta dalle due foreste da `unire`.

### Heap di Fibonacci
