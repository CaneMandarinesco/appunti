## algoritmi di ricerca
si intende che cerchiamo elementi che hanno una qualche proprieta' dentro uno spazio di ricerca.  
in una lista generica abbiamo che:
* caso peggiore: $O(n)$
* caso medio: $\Theta(n)$
secondo la seguente implementazione:

```python
n = ... # valore che mi interessa
for x in a
	if x == n:
		# ho trovato il valore che voglio!
```

> [!warning] attenzion!
> questa sezione e' inaccurata

>[!note] indirezione.
> quando accediamo ad una lista fatta di tipi diversi, per accedere all'elemento che cerchia non facciamo altro che accedere ad un elemento che contiene un riferimento all'oggetto che cerchiamo (questa ricerca puo' ripetersi n volte, dipende da quanti oggetti e come sono disposti nella lista).
> esempio: nella lista voglio arrivare ad `a[3]`, ma magari per arrivare ad alla posizione in memoria di `a[3]` (dato che suppongo che gli elementi in a siano di tipi diversi) devo partire per esempio da `a[0]`, prendere il suo puntatore ad `a[1]`, che punta ad `a[2]` che infine punta ad `a[3]`

> [!note] puntatori
> in python ogni elemento di una lista contiene un puntatore all'elemento successivo!

se vogliamo cercare un elemento nella lista `n` l'unico modo che ho e' scorrere tutti gli elementi finche' non trovo quello che voglio (vedi prima).

### ricerca binaria (bin search)
supponiamo ora che `n` sia una lista ordinata (dal piu piccolo al piu grande) di interi, possiamo migliorare l'algoritmo di prima  in questo modo:
```python
def search(L,e):
	for i in range(len(L)): 
		if L[i] == e:
			return True
		if L[i] > e:
			return false
	return False
```

questa implementazione migliora il tempo medio ma non quello peggiore che rimane comunque: $\Theta(n)$.  possiamo fare di meglio: posso chiedermi se `e` sia piu' grande o piu' piccolo di `L[i]` e a seconda del risultato dell'operazione mi sposto nell'array.

```python
def search(L, e):
	def bin_search(L, e, low, high):
		if high == low:
			return L[low] == e
		mid = (low + high)//2
		if L[mid] == e:
			return True
		elif L[mid] > e:
			if low == mid:
				return False
			else:
				return bin_search(L, e, mid+1, high)
		else:
			return bin_search(L, e, 0, len(L-1))
	if len(L) == 0:
		return False
	else:
		return bin_search(L,e,0,len(L) -1)
```

al caso peggiore la complessità e':
$$
O(\log(n))
$$
il perché: non e' interessante.

## algoritmi di ordinamento
### per selezione (**sort**)
quello di python esegue in tempo: $O(n \log n)$.  
di seguito abbiamo una semplice implementazione dell'algoritmo:
```python
def sel_sort(L):
	suffix_start = 0
	while suffix_start != len(L):
		for i in range(suffix_start, len(L))
			if L[i] < L[suffix_start]:
				# scambiali
				L[suffix_start], L[i] = L[i], L[suffix_start]
		suffix_start += 1
```
la complessita' e: $\Theta(n^2)$

### per fusione (meglio di prima) (**merge sort**) (divide and conquer)
* se la lista ha 0 o 1 elementi e' gia ordinata (caso banale)
* se ha piu di un elemento puo' essere divisa in due sottoliste

von Neumann ha osservato che due liste ordinate possono essere concatenate facilmente in un'unica lista ordinata: 
* si esamina il primo elemento di ciascuna lista
* il piu' piccolo viene **estratto** e messo in una lista di output
* si ripete fino ad esaurire gli elementi.

l'algoritmo di merge sort segue questi passi:
* dividi i 2 array n volte fino a che le 2 liste non sono divise in n liste da un elemento
* riordina le liste a ritroso
esempio:
![[Pasted image 20240317101921.png]]

esempio di implementazione dell'algoritmo: 
```python
def merge(left, right, compare):
	# left e right sono liste ordinate, e compare e' una funzione che comapra due numeri.
	result = []
	i,j = 0,0
	# metti p
	while i < len(left) and  j < len(right):
		if compare(left[i], right[j]):
			result.append(left[i])
			i += 1
		else:
			result.apppend(right[j])
			j += 1
	# fai l'append degli elementi rimasti
	while i < len(left):
		result.append(left[i])
		i += 1
	while j < len(right):
		result.append(right[j])
		j += 1
	return result

def merge_sort(L, compare = lambda x, y: x < y):
	if len(L) < 2:
		return L[:] # una copia di L
	else: 
		middle = len(L)//2
		left = merge_sort(L[:middle], compare)
		right = merge_sort(L[middle:], compare)
		return merge(left, right, compare)
```

ogni chiamata di `merge_sort` spezza la lista L in due liste left e right che sono create dalle loro rispettive chiamate a `merge_sort`. una volta finito il ciclo di `merge_sort` a ritroso vengono eseguiti i merge delle liste ordinate!