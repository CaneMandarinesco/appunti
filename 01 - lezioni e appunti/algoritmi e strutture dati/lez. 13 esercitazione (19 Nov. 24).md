### Esercizio
#### Input
un albero binario $T$ di $n$ nodi. Ogni nodo ha:
* valore: $val(v) > 0$
* colore: $\text{col}(v) = (R,N)$. Puo essere di colore rosso o nero

def:
* il valore di un cammino e' la somma di valori dei nodi del cammino
* un cammino e' rosso se tutti i suoi nodi sono di colore rosso

#### output
valore del cammino rosso di tipo radice-nodo di valore massimo

#### svolgimento
$T$ e' rappresentato da un record con dei puntatori: ogni nodo ha:
* puntatore al padre
* valore
* colore
* puntatore a figlio sinistro e destro

Definisco la funzione`maxRosso(v)`, richiede un nodo `v` (puntatore al record), e restituisce il valore del cammino rosso di valore massimo di tipo `v-discendente` di `v`.

```python
MaxRosso(v):
	if v=null then return 0
	if col(v) = N then return 0
	return val(v) + max(MaxRosso(sx), MaxRosso(dx))
```

L'algoritmo ha complessità: $O(n)$.

### Esercizio
#### Input
* Albero binario $T$ di $n$ nodi (rappresentato da record e puntatori)
* Intero $h\geq 0$

#### Output
* numero di nodi di $T$ con profondità almeno $h$. ($\#\text{archi}$)

`ContaProf(v,h,i)`: assume che `v` abbia profondità `i`


