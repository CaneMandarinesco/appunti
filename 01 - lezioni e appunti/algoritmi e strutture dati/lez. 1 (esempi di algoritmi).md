## esempio di un problema
* **problema**: individuare 1 moneta falsa tra `n` monete
* **istanza**: quanto vale `n`? ovvero la configurazione dei dati in input
* **dimensione dell'istanza**: valore di `n` in questo caso
* **modello di calcolo**: bilancia a due piatti. specifica quello che posso fare per risolvere il problema
* **algoritmo**: strategia di pesatura. sequenza di operazioni che vanno eseguite per qualsiasi istanza del problema
* **correttezza**: per qualsiasi istanza, trovo sempre la moneta falsa.
* **complessità temporale**: `#` di pesate eseguite. Dipende dalla dimensione dell'istanza, e' espressa come *funzione della dimensione dell'istanza*
* **efficiente**: l'algoritmo deve fare poche pesate, quindi veloce, ma rispetto a cosa?

### Algoritmo 1
* `n=7`
* **algoritmo**: prendo la prima moneta e la confronto con tutte le altre
* il numero di pesate dipende da dove si trova la moneta falsa
* e' **corretto**!
* `#pesate`: dipende.
* Al caso peggiore ne fa `n-1`
* efficiente? boh. *ma posso fare meglio*?
**pseudo codice**: 

```python
Alg1(X=[x1,x2,...,xn])
	for i=2 to n do
		if peso(x1) > peso(xi) then return x1
		if peso(x1) < peso(xi) then return xi
```

osservazioni:
* l'ultima pesata non serve, posso passare quindi a $n-2$ pesate. ***meh***

### Algoritmo 2
* algoritmo: peso a coppie
* al caso peggiore: $\frac{n}{2}$
* meglio di $n-2$

```python
Alg2(X=[x1,x2,...,xn])
	for i=1 to k do
		if peso(X2i-1) > peso(x2i) then return x2-1
		if peso(x2i-1) < peso(x2i) then return x2i
```

### Algoritmo 3
* posso pesare molte monete insieme
* peso sempre gruppi pari, se ho monete dispari una la lascio fuori