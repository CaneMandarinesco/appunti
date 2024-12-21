### Esercizio 1
#### A
Dire quali delle seguenti relazioni asintotiche sono vere:
* $n^3 + n^2\log^2 n = o(n^2 \log^{30} n)$. **Falsa**
* $\log^2 n = o(\log^{30}n)$. **Vera**
* $n^2 = \Omega\left( \frac{n^{2.001}}{\log^{2001}n} \right)$. **Vera**
* $\frac{n \sqrt{ n } + \log n}{\sqrt{ n^3+3 }} = \Theta(\log n)$. **Falsa**
* $2^{2n} = \omega(2^{1.9n})$. **Falsa**
* $2^n = \Theta(2^n + 1.5^n)$. **Vera**
* $2^n = o(2^n + n^2)$. **Falsa**
* $2^n = \Theta(2^{n+8})$.  **Vera**

#### B
Fornire la soluzione asintotica alle seguenti relazioni: $$T(n) = 2T\left( \frac{n}{4} \right) + n$$
Che ha come soluzione: $\Theta (n^2)$.

$$
T(n) = 2T(n-2) + 1
$$
Che ha come soluzione: $\Theta(2^n)$.

#### C
Quale algoritmo useresti e quanto costa se devi:
* Calcolare l'n-esimo numero di Fibonacci.

Userei l'algoritmo basato sull'elevazione a potenza della matrice: $[1,1,1,0]$. Questa impiega $\Theta(\log n)$.

* Costruire un heap binomiale con n chiavi

### Esercizio 2
Ti e' dato in input un array $S[1:n]$ di $n$ numeri, ogni numero rappresenta un tipo di evento che avra luogo il giorno $i$. I numeri si possono ripetere, tu devi scegliere una finestra di $k$ giorni consecutivi e vuoi massimizzare il numero di eventi distinti contenuti nella finestra. Progetta un algoritmo che dato $S$ e $k$ risolve il problema in $O(n \log k)$. Fornisci uno pseudocodice dettagliato. 


```c

proc countEventiDiversi(S, l, r):
	// do something

eMax = 0
iMax = 0
for i=0 to n-k-1:
	e = countEventiDiversi(S, i, i+k)
	if e > eMax:
		eMax = e
		iMax = i
```