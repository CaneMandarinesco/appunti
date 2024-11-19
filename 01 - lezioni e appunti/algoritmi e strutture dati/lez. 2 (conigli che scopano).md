* [slides](https://www.mat.uniroma2.it/~guala/cap1_2021.pdf)

> [!note] Fibonacci
> $F_{1,2} = 1$

L'albero dei conigli:
* ogni nodo rappresenta una coppia
* ogni coppia adulta, ogni anno genera una coppia giovane di conigli
* ogni anno, le coppie giovani diventano coppie adulte
* il numero di conigli si calcola con **Fibonacci**

### alg. 1
Esiste una **formula chiusa** per calcolare Fibonacci: $\frac{1}{\sqrt{ 5 }}(\phi^n - \hat{ \phi ^n})$.
Questo algoritmo non funziona sempre: per $n=18$ ho che `fibonacci(n) = 2583.1`, arrotondato diventa $2583 \neq F_{n}=2584$.

### alg. 2
Uso una **funzione ricorsiva**, l'algoritmo e' corretto.
Quante linee di codice vengono eseguite in funzione di $n$?

* $n=1 \to 1$
* $n=2 \to 1$
* $n=3 \to 4$

come trovo $f(n)$? non e' molto chiaro l'andamento di $f(n)$.

## Albero della ricorsione 
> metodo per trovare $f(n)$, per alg. 2

$T(n) = \text{\# linee di codice eseguite nel caso eggiore, su input n}$

In ogni chiamata con $n\geq 3$ si eseguono almeno 2 linee:
$$
T(n) = 2 + T(n-1) + T(n-2);\;\;\;T(1)=T(2) = 1
$$
Devo capire quante foglie, e quanti nodi interni ho:
* **ogni foglia** e' una linea di codice
* **ogni nodo interno** sono due linee di codice

Devo calcolare $T(n)$ in funzione del numero di foglie e del numero di nodi.

> [!note] numero di foglie
> il numero di foglie dell'albero e' $F_{n}$. si dimostra per induzione

> [!note] numero di nodi interni
> il numero di nodi interni e': $\#foglie - 1$. si dimostra per induzione

Quindi le linee di codice totali sono:
$$
F_{n} + 2(F_{n} - 1) = 3F_{n} - 2
$$

Quindi:
$$
T(n) \approx F_{n} \approx \Phi^n
$$
e' un algoritmo lento, per calcolare $F(100)$ ci vogliono $8000$ anni.

### Alg. 3
> [!note] oss.
> in Alg. 2, calcolo $k\leq n$ volte sempre gli stessi valori. potrei piuttosto calcolarli una volta sola e riutilizzarli.

Posso memorizzare in un array di `n` interi i valori di Fibonacci.

```python
algoritmo fibonacci3(intero n) -> intero
	sia Fib un array di n interi # 1
	Fib[1] <- Fib[2] <- 1 # 1

	for i = 3 to n do # n
		Fib[i] <- Fib[i-1] + Fib[i-2] # n
	return Fib[n] # 1
```

e' molto più veloce (programmazione dinamica).

$$
T(n) \leq n-1+n-2+3 = 2n+3
$$

per $T(45)$ eseguo 38 milioni di righe di codice in meno rispetto a fib. 2:
* fib. 2: $\Phi^n$
* fib. 3: $n$

### fib. 4: occupazione di memoria
* con fib. 3 occupo circa $n$ spazi. posso fare di meglio?
* non ho bisogno tenere in memoria tutti i valori calcolati

```fibonacci
algoritmo fibonacci4(intero n) -> intero
	a <- 1, b <- 1
	for i = 3 to n do
		c <- a+b
		a <- b
		b <- c
	return c
```

* uso solo 3 variabili, invece di un array di `n` elementi.
* e' poco leggibile.
* $T(n) \leq 4n +2$, e' piu veloce o lento di fibonacci3?

fib. 3 e' 2 volte piu veloce di fib. 4, ma sono dello stesso ordine di grandezza.

> [!note] notazione asintotica
> $f(n) = O(g(n)) \text{ se } f(n)\leq c \cdot g(n)$, per qualche $c$ con $n$ abbastanza grande  

### fib. 5: matrici
$$
\begin{bmatrix}
1 & 1 \\
1 & 0
\end{bmatrix}^n = 
\begin{bmatrix}
F_{n+1} & F_{n} \\
F_{n} & F_{n-1}
\end{bmatrix}
$$
> ringraziamo pasquale, per l'induzione!

Si per induzione su $n$, questa proprietà delle matrici, fissando $F_{0} = 0$, per convenzione, in modo che: $F_{2} = F_{1} + F_{0}$

```python
algoritmo fib3(intero n) -> intero
	M <- mat1
	for i = 1 to n - 1 do
		M <- M * mat2
		
	return M[0][0]
```

dove:
* `mat1` e' $\begin{bmatrix} 1 &0 \\ 0 & 1\end{bmatrix}$
* `mat2` e' $\begin{bmatrix} 1 & 1 \\ 1& 0\end{bmatrix}$
> Questo algoritmo ha ancora ordine $O(n)$, rispetto a memoria e costo computazionale non ci ho guadagnato bene

### fib. 6
per calcolare la potenza di una matrice ci sono metodi ***furbi***.
$$
3^8 = 3 \cdot 3 \cdot 3 \cdot 3 \cdot 3 \cdot 3 \cdot 3 \cdot 3
$$
ovvero 3 prodotti, ma posso fare anche:
* $3^8=81^2$
* $81 = 9^2$
* $9 = 3^2$ 
* ...

* $O(\log_{2}n)$