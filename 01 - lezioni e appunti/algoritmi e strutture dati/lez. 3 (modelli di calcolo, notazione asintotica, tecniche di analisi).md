#notazioneasintotica #ogrande #opiccolo
### macchina di turing
Il primo modello storico introdotto e' la macchina di **Turing**:
* un nastro infinito bidirezionale rappresenta la **memoria**
* il nastro e' diviso in **celle**
* ciascuna cella puo' contenere un simbolo, preso da una alfabeto finito.
* l'accesso al nastro avviene tramite una **testina**
* ogni passo di calcolo permette alla testina di: 
	* spostarsi di una cellla
	* cambiare stato di controllo
	* scrivere o leggere

### modello di von neumann
Questo modello e' di livello troppo basso, usiamo un metodo piu adeguato che si ispira alla macchina di **von neumann**, o **macchina a registri**. e' costituita da  
* **programma** finito
* **nastro** in entrata e in uscita
* **memoria** strutturata come **array** di `n` parole, con un indirizzo da 1 a `n`
* le parole possono essere un qualsiasi **intero** o un qualsiasi numero **reale**.
* **accumulatore**: contiene l'operando 
* **program counter**
* ogni istruzione permette una di queste operazioni
	* leggere o stampare
	* una operazione aritmetico/logica
	* accesso o modifica di un elemento in memoria

### macchina a puntatori
qui la memoria e' una collezione di nodi. ogni nodo puo' contenere o un intero o un puntatore ad un nodo. Non posso fare operazioni sui puntatori, a differenza della macchina di von neumann.

Queste ultime due macchine si aspettano dei programmi **sequenziali** e **deterministici**. 


### Criteri di costo uniforme e logaritmico
* **uniforme**: il costo computazionale di un'operazione e' sempre costante. anche se devo computare 1,2,100 o miliardi di cifre
* **logaritmico**: il costo computazionale dipende dalla grandezza dei dati. la grandezza dei dati influenza il tempo di esecuzione in `log(n)`

```python
 x = 2
 for x in range(1,n):
	 x = 2*x
```
per il criterio uniforme il tempo di esecuzione e' $n$. Invece per il costo logaritmico, ad ogni passo il **valore** di `x` raddoppia. Quindi all'`i`-esimo passo ho che $x=2^i$.  Il tempo speso per operare su `x` e' pari a $\log x = \log 2^i = i$.

Il tempo di esecuzione e' proporzionale ad:
$$
\sum^n_{i=1} (i+\log(i))
$$
ossia:
$$
\frac{n(n+1)}{2} = \sum^n_{i=1}i \leq \sum^n_{i=1}(i + \log i) \leq \sum^n_{i=1}2i = n(n+1)
$$
il tempo di esecuzione e' proporzionale a circa $n^2$. 

Ma questo criterio porta a complicazioni non necessarie, quindi limiteremo a $c \cdot \log n$ bit la dimensione dei numeri interi rappresentabili, con `c` costante e `n` dimensione dell'istanza del problema. Quindi il numero piu grande rappresentabile e' $n^c$



### Caso peggiore, migliore e medio

* $O(f(n)) = \{  g(n): \exists c>0 \text{ e } n_{0}\geq 0\text{ t.c. } g(n) \leq cf(n) \text{ per ogni } n\geq n_{0}  \}$
* $\Omega(f(n)) = \{ g(n): \exists c>0 \text{ e } n_{0} \geq 0 \text{ t.c. } g(n)\geq c f(n) \text{ per ogni } n\geq n_{0} \}$
* $\Theta(f (n)) = \{ g(n): \exists c_{1},c_{2} > 0 \text{ e } n_{0} \geq 0 \text{ t.c, per ogni } n\geq n_{0}, c_{1}f(n)\leq g(n) \leq c_{2} f(n) \}$
ovvero in italiano:
* se $g(n) \in O(f(n))$ allora, $g(n)$ cresce **al piu** come $f(n)$, **upper bound**
* se $g(n) \in \Omega(f(n))$ allora, $g(n)$ cresce **almeno** come $f(n)$, **lower bound**
* se $g(n) \in \Theta(f(n))$ allora, $g(n)$ cresce **esattamente** come $f(n)$


| notaz.   | simbolo logico |
| -------- | -------------- |
| $O$      | $\leq$         |
| $\Omega$ | $\geq$         |
| $\Theta$ | $=$            |
| $o$      | $<$            |
| $\omega$ | $>$            |


> Quando uso questa notazione assumo che mi interessa come l'algoritmo si comporta per istanze grandi.

> $\Theta(\log_{2} n) = \Theta(\log_{3}n)$, dato che la base del logaritmo puo' essere trasformata in una costante, che quindi viene ignorata nella notazione asintotica.

> su istanze molto grandi, $n^2$ impiega dalle 3 ore ai 12 giorni. invece $n\log n$ impiega sulla stessa impiega circa 20 secondi.

> notazione appropriata:
> - $2n^2 + 4 = O(n^3)$ non e' matematicamente corretta
> - $2n^2 + 4 \in O(n^3)$

> vale che:
> * $f(n) = o(g(n)) \to \lim_{ n \to \infty } \frac{f(n)}{g(n)} = 0$
> * $f(n) = \omega(g(n)) \to \lim_{ n \to \infty } \frac{f(n)}{g(n)} = \infty$

### Esempi
* $\Theta(\log^{10}(n))  \text{ e } \Theta(\sqrt{ n })$ chi va piu veloce? il logaritmo
* $n \log n \text{ e } n$. Posso dire che $n \log(n) = \omega (n)$

### Analisi complessita di fib 3
```python
1 algoritmo fib3(n)
2  	Fib[1] <- Fib[2] <- 1
3	for i = 3 to n do:
4		Fib[i] <- ...
5	return Fib[n]
```

* eseguo 3 linee, una sola volta
* le linee 3 e 4 vengono eseguite al piu $n$ volte
* quindi: $$

### Delimitazioni superiori e inferiori
* $O$ esprime una delimitazione superiore, del tempo di esecuzione del problema
* $\Omega$ esprime una delimitazione inferiore: meglio di cosi non si puo' fare, o almeno, con il modello di calcolo che sto usando.

## Tecniche di analisi
### Caso peggiore e caso migliore e caso medio
```python
def ricercaSequenziale(lista L, elem x) -> bool:
	for y in L:
		if(y == x):
			return True
	return False
```
* **caso peggiore**: $T_{\text{worst}}(n) = \max \limits_{\text{ istanze I di dimensione n}} \{  tempo(I) \}$
* **caso migliore**: $T_{\text{ best}} (n) = \min \limits_{\text{ istanze I di dimensione n}} \{  tempo(I) \}$
* **caso medio**: $T_{\text{avg}}(n) = \sum \limits_{\text{ istanze di I di dimensione n}} \{  P\{ I \} \cdot {tempo}(I) \}$
	* $P\{ I \}$ e' la probabilita  dell'istanza di verificarsi


# Esercizi
### es. 1
Analizzare complessità nel caso medio del primo algoritmo di pesatura. Rispetto alla distribuzione di probabilità sulle istanze, si assuma che la moneta falsa possa trovarsi in modo equiprobabile in una qualsiasi  delle `n` posizioni. 
