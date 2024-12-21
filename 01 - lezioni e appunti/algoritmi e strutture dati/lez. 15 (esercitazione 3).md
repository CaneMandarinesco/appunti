### Esercizio 1
Input: vettore ordinato $A[1:n]$ di $n$ bit, ovvero $A[i]\in\{ 0,1 \}$.
Output: l'indice $k$ dell'ultimo 0.

#### Obiettivo 1
Obiettivo 1: $O(\log n)$

```c
algoritmo ricercaUltimoZero(A, i, j):
	if i > j
		return -1

	m = (j-i)/2 // divisione intera
	xm = A[m]   // valore corrispondente all'indice m
	xd = A[m+1] // valore a destra dell'indice m
	
	if (xm == 0) and (xd == 1)
		return m;
	else if(xm == 1)
		return alg(A, i, m-1)
	else
		return alg(A, m+1, j-1)
```

Il problema si ha quando l'array e' pieno di zeri! L'algoritmo `riercaUltimoZero` in questo caso andrebbe accedere all'elemento `A[n+1]`. Implemento un'altra procedura che chiama il mio algoritmo, controlla se `A[n]=0`.

#### Obiettivo 2
Obiettivo: $O(\log k)$


### Problema 2
Input: vettore $A[1:n]$  di $n$ bit.
Output: indice k tale che $\#$ di zeri in $A[1:k]=\#\text{ di uni in } A[k+1:n]$.

Goal: $O(n)$.


due contatori:
* `i`, avanza
* `j`