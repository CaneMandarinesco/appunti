## Implementare il Selection Sort
L'implementazione dell'algoritmo prende in esame un'array da `i` a `j`, e assume che i numeri prima di `i` siano correttamente ordinati. Sia `k` l'indice che scorre da `i` a `j`, allora `k+1` e' l'elemento da ordinare
```python
algoritmo selection_sort(lista A, indici i,j):
	for k in range(i,j-1):
		x = A[k+1]
		for l in range(k, i-1):
			if(A[k] > x):
				sostituisci A[k+1] con A[k]
			else:
				break
	return A
```