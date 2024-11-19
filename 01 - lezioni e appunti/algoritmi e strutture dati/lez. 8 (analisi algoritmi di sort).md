* Upper Bound: $O(n^2)$: Insertion Sort, Selection, Quick Sort, Bubble Sort
* Upper Bound migliore: $O(n \log n )$: Merge Sort
* Lower Bound (ovvio): $\Omega(n)$, almeno una volta devo leggerli tutti

Possiamo fare di meglio? Andiamo a dare un Lower Bound per algoritmi basati su **confronti**, posso solo eseguire operazioni di confronto: $\leq, <, >, \geq, =$.

> Tutti gli algoritmi visti sono basati su confronto. 

> [!note] Teorema
> Ogni algoritmo basato su confronti che ordina $n$ elementi impiega al caso peggiore $\Omega(n \log n)$.

Si dimostra con l'**albero di decisione**, un generico algoritmo lavora nel modo seguente:
* confronta $a_{i}$ e $a_{j}$
* a seconda del risultato ordina e/o decidi il confronto successivo da eseguire.

l'albero di decisione mi descrive il modo in cui un generico algoritmo di sort procede:
* non e' associato ad un problema
* non e' associato ad un algoritmo
* e' associato ad un algoritmo e a una dimensione dell'istanza
* descrive le sequenze di confronti possibili

proprietà: 
* i confronti eseguiti sono rappresentati dal cammino: `radice->foglia`
* l'algoritmo segue un cammino diverso a seconda dell'istanza
* il numero di confronti al caso peggiore e' l'**altezza dell'albero**
* un albero di decisione di un algoritmo che ordina $n$ elementi ha almeno $n!$ foglie.

>[!note] Lemma
>Un albero binario $T$ con $k$ foglie ha altezza almeno $\log_{2}k$.

Dimostrazione (per induzione): 
* $k=1$ ha altezza almeno $\log_{2} 1 = 0$. 
* caso induttivo: $k>1$
* considera il nodo intero `v` più vicino alla radice che ha due figli.
* allora $v$ ha almeno un figlio $u$ che e' radice di un sotto albero con $k /2$. 
### lower bound $\Omega(n \log n)$
* L'altezza `h` dell'albero di decisione e' almeno $\log_{2}(n!)$. 
* Formula di Stirling: $n! = (2\pi n)^{1 /2} \cdot (n / e)^n$.

$$
h \geq \log_{2}(n!) > \log_{2}( n /e)^n = n \log_{2} (n /e) = n\log _{2} n - n \log_{2} e = \Omega(n \log n)
$$

### Integer Sort
Per ordinare $n$ interi (non in virgola mobile) con valori in $[1,k]$., con $k$ un valore "piccolo".

Prima fase: conteggio, e metto il numero di volte che compare un numero dentro l'array $X$, in un array ausiliario $Y$. l'indice coincide con il numero che sto esaminando.
Seconda fase: ordino con il conteggio, scorro $Y$ e ordino $X$.


```
IntegerSort(X,k)
	Sia Y un array di dimensione k
	for i=1 to k do Y[i] = 0           # O(k)
	for i=1 to n do incrementa Y[X[i]] # O(n)
	j=1
	for i=1 to k do                    # O(k)
		while(Y[i] > 0) do             # O(k+n)
			X[j]=i
			incrementa j
			decrementa Y[i]
```

$$
\sum_{i=1}^k(i+Y[i]) = \sum_{i=1}^k 1 + \sum_{i=1}^k Y[i] = k+n
$$

se $k=O(n)$ allora esegue in tempo lineare! Abbiamo fatto meglio di $\Omega(n \log n)$. *Non contraddice il teorema di prima perché questo algoritmo non e' basato su confronti.*

