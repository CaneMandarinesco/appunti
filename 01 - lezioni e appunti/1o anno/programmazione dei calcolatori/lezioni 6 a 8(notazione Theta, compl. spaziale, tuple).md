* introduzione del concetto di funzioni
### es 9. conta parole
implementata con la [mia soluzione](https://github.com/CaneMandarinesco/esercizi_prog_dei_calc_23_24/blob/main/conta_parole.py)

# notazione Theta
$$
f(n) \in \Theta(g(n)) \to c_{1}|g(n)| \leq |f(n)| \leq c_{2}|g(n)|
$$
ovvero: se  $f(n)$ e' un Theta di $g(n)$, allora $f(n)$ e' compreso in $c_{1} \cdot |g(n)|$ e $c_{2} \cdot |g(n)|$ dove $c_{1}$ e $c_{2}$ sono due numeri diversi

indica il caso medio di esecuzione di un algoritmo, e' il caso piu' realistico

## extra: notazione Omega
$$
f(n) = \Omega(g(n)) \to f(n) \geq c_{1} \cdot g(n)
$$
indica il **migliore caso di esecuzione** per un algoritmo
### es 10. cerca occorrenze costo computazionale
```python
a = input('inserisci un testo: ')
n = len(a)

if 'A' in a:
	print('Si')
else:
	print('No')
```
complessità $\Theta(n)$ nel caso peggiore, 'A' non in a, $\Theta(n)$ anche nel caso medio.

### es 11.
#### 1a vers.

```python
def conta_occorrenze(x, y):
	# trova quante volte la stringa y sta in x
	# nota: len(y) < len(x)
	n,m = len(x), len(y)
	
	occ = 0 # numero di occorrenze
	i= 0
	while i < n-m:
		if x[i:i+m] == y: # Theta(m)
			occ += 1
		i += 1
	return occ
# esempio di utilizzo
print(conta_occorrenze('asdjdfsnbasdfdf', 'df'))
```
* complessità temporale e' $(n-m) \cdot \Theta(n)$ ovvero $\Theta(n(n-m))$
* complessità **spaziale** e' $\Theta(m)$ (spazio aggiuntivo rispetto all'I/O)

#### 2a vers.
```python
def conta_occorrenze(x, y):
    n, m = len(x), len(y) # O(1)
    
    occ = 0
    i = 0 # la prima posizione della sottostringa di x in esame
    while i <= n-m:
        j = 0
        while j < m and y[j] == x[i+j]:
            j += 1
        if j == m:
            occ += 1
        i += 1 # O(1)
        
    return occ 
```
* complessita temporale uguale a prima
* complessita spaziale: $O(1)$

#nonhocapito 

## tuple
e' una struttura ***immutabile*** (`tupla[2] = c` non si puo' fare!)
```)
### come definire una tupla
```python
t = 1, 2.3
g = (1,2.3)
t == g # True
```

### operazioni possibili
#### concatenazione
```python
t = (1,2,3)
g = ("uno", 2, "III")
h = t+g  # (1,2,3,"uno", 2, "III")
# le due tuple vengono concatenate!
```

#### moltiplicazione con numero n
vengono ripetuti n volte gli elementi della tupla
```python
t = (1, "ciao", 3)
2*t # (1, "ciao", 3, 1, "ciao", 3)
```

#### accesso ad un elemento
```python
t = ('fascio', 'merda')
t[0][1] # 'a' ovvero: il 1o elemento dell'elemento numero 0 nella tupla ('fascio')
```

#### packing e unpacking
```python
a = x,y,z # packing
c,d,e = a # unpacking
```

#### usare variabile per definire una tupla
```python
n = 1
t = (n, n+1) # (1, 2)
```

---
### es funzione list()
```python
a = list('abcdef')
a # ['a', 'b', 'c', 'd', 'e', 'f']

t = (1,2,3,4)
a = list(t)
a # [1,2,3,4]
```