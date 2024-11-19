# algoritmi di ricerca 
## come funzionano le liste in python
python usa il seguente algoritmo per determinare se un elemento e' in una lista:

```python
for i in range(len(L)):
	if L[i] == e:
		return True
return False
```

ovvero un semplice controllo con un if.  diremo che il codice esegue in tempo `lineare`. Ma per affermare cio' dobbiamo essere sicuri anche del fatto che ad ogni iterazione, tutte le istruzioni dentro al loop vengano eseguite sempre in tempo costante! altrimenti non sarebbe $O(n)$ ma qualcosa di leggermente diverso...

ci dobbiamo chiedere quindi se `L[i]`, ovvero l'operazione di accesso all'elemento `i-esimo` di `L` avviene in tempo costante.  
se tutti gli elementi di una lista fossero di uno *stesso tipo ed a lunghezza fissa*, es. gli **interi**, e che la lista si trovi in un *area contigua di memoria*: posso ipotizzare che python calcoli l'indirizzo da raggiungere in memoria usando la formula `start + 4*i`, dove:
* `start`: dove si trova il primo elemento della lista
* `4`: lunghezza in byte di ogni singolo elemento
* `i`: indice dell'elemento da ottenere
quindi python e' in grado di ottenere l'indirizzo in **tempo costante**. *ma se l'area di memoria non fosse contigua? e se gli elementi non hanno grandezze fisse?* python riesce a gestire anche questi casi.  

in python una lista e' implementata usando una **lista di puntatori** ad oggetti dove l'ultimo elemento punta alla **lunghezza della lista**.  questa tecnica e' chiamata **indirection**. accediamo ad un elemento accedendo prima al suo puntatore, non accediamo direttamente all'elemento stesso.

questa tecnica e' particolarmente efficace perche' i *puntatori sono sempre della stessa grandezza*, e quindi posso accederci linearmente. mentre il *valore reale a cui punta il valore puo' essere di grandezza variabile*.  

## se po fa de mejo?
facendo delle **assunzioni** posso migliorare il mio algoritmo di ricerca?   
per esempio se devo cercare un intero in una **lista di numeri ordinati** posso fare di meglio.  

```python
def search(L,e):
"""Assumo he L sia una lista di elementi di ordine ascendente."""
for i in range(len(L))
	if L[i] == e:
		return True
	if L[i] > e:
		return False
```

con questo algoritmo miglioro il caso medio ma non quello peggiore, facciamo ancora di meglio:
### binary search
```python
def search(L,e):
	"""assumi che L sia una lisita di elementi di ordine ascendente"""
	def bSearch(L, e, low, high):
	"""
	L: lista
	e: elemento da cercare
	low, high: estremi della lista in cui cercare
	"""
		# la funzione decrementa high-low, ad ogni iterazione
		if high == low: # high-low = 0
			return L[low] == e
		mid = (low+high)//2
		if L[mid] == e:
			return True
		elif L[mid]>e:
			if low == mid:
				return False
			else:
				return bSearch(L, e, low, mid -1)
		else:
			return bSearch(L, e, mid+1, high)
	if len(L) == 0:
		return False
	else:
		return bSearch(L, e, 0, len(L)-1)
```

>[!note] OSS.
> in gergo, `search` e' una funzione **wrapper**, ossia che chiama altre funzioni base, in questo caso `bSearch`, usando i parametri appropriati, semplificando il lavoro dell'utente.  lo facciamo perche' i parametri `high` e `low` di `bSearch` sono dei parametri che dovrebbero essere nascosti al programmatore che vuole semplicemente cercare elementi in una lista.  

la complessità e' $O(\log_{2}(high-low)) = O(\log_{2}(len(l)))$.
#### perché???
nel codice, l'intervallo `high/low` viene tagliato in 2 ad ogni iterazione, finche' non trovo il valore che cerco.  ed al caso peggiore vuol dire che io ho tagliato in 2 il segmento high-low per `n` volte, quindi se moltiplico 2 per n volte, ovvero $2^n$, ottengo $len(l)$, questa e' la definizione di logaritmo in base 2.  (????)

# sorting
sapendo che possiamo migliorare i nostri algoritmi facendo delle assunzioni, e' conveniente `sort-are` una lista prima di cerca un elemento? no, perche' comunque bisogna **sempre** scorrere tutti gli elementi di una lista durante un sort.  questo tipo di approccio e' conveniente quando dobbiamo cercare piu volte nella stessa lista, quindi possiamo chiederci se:
$$
\text{sortComplexity(L)} + k \cdot \log(len(L)) < k \cdot len(l)
$$
* $k \cdot \log(len(L))$ e' il numero di volte per cui cerco nella lista
* $\text{sortComplexty}(L)$ e' il costo del sort
* $k*len(L)$ e' il numero di volte in cui cerco nella lista un elemento, scorrendo tutta la lista

l'implementazione di python del sort esegue in tempo: $O(n \cdot \log n)$

## selection sort
```python
def selSort(L):
	"""
	L: e' una lista di elementi comparabili con >
	"""
	suffixStart = 0
	while suffixStart != len(L):
		for i in range(suffixStart, len(L)):
			if L[i] < L[suffixStart]:
				L[suffixStart], L[i] = L[i], L[suffixStart]
		suffixStart += 1
```

il codice prende in esame ogni elemento a partire dal primo, e lo confronta scorrendo la lista $\text{suffixStart} - len(L)$ volte cercando ogni volta il numero piu piccolo, cosi in questo modo sono sicuro di aver messo al posto del suffisso l'elemento piu piccolo possibile.  e procede allo stesso modo per gli elementi dopo. 

viene eseguito in tempo: $O(n^2)$ dato che scorriamo n volte la lista, e per ogni volta scorriamo $n-\text{suffixStart}$ gli elementi dopo `suffixStart`
## merge sort: potemo fallo mejo
usiamo l'algoritmo **divide and conquer**: si divide il problema in piccoli sotto-problemi e si risolvono in modo indipendente. alla fine vengono ricongiunti i risultati.  

questo tipo di algoritmo e' adatto al nostro scopo perche' von Neumann osservo che e' semplice ed efficiente unire due liste ordinate in un unica lista ordinata (def. di **merge**).    