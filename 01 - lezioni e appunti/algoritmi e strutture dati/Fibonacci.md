# computare Fibonacci

>[!note] Fibonacci
>$$F_{n} = F_{n-1} + F_{n-2}$$ con $F_{1,2} = 1$

## Fibonacci 1: formula immediata 
Per ottenere la formula immediata, bisogna risolvere la ricorsione lineare: 
$$
a^n = a^{n-1} + a^{n-2}
$$
si ha che:
$$
a^n - a^{n-1} - a^{n-2} = 0
$$
imponendo $a\neq 0$, posso eliminare il termine $n$:
$$
a^2 - a - 1 = 0
$$

si ottengono le radici che sono: 
* $\phi = \frac{1+\sqrt{ 5}}{2}$
* $\hat{ \phi} = \frac{1-\sqrt{ 5 }}{2}$

ho trovato le funzioni: $\phi^n$ e $\hat{\phi}^n$, che non approssimano accuratamente il valore di Fibonacci. Bisogna imporre i casi base. Per farlo costruisco una combinazione lineare di $\phi$ e $\hat{\phi}$, con coefficienti $c_{1}$ e $c_{2}$, imponendo i casi base:
* $c_{1} \phi + c_{2}\phi = 1$, dove $n=1$
* $c_{1} \phi^2 + c_{2} \hat{\phi}^2 = 1$, dove $n=2$

ottengo che $c_{1,2} = \pm \frac{1}{\sqrt{ 5 }}$.

Quindi la funzione che approssima Fibonacci e':
$$
F_{n} = \frac{1}{\sqrt{ 5 }}(\phi^n - \hat{\phi}^n)
$$
Problema: $\phi$ e $\hat{\phi}$ sono numeri Irrazionali. In quanto tali sul calcolatore, i loro valori sono approssimanti ad un valore finito, la cui precisione e' limitata dalla capacita di elaborazione del calcolatore.

Per esempio se il calcolatore assumesse che $\phi = 1.618$ e $\hat{\phi} = 0.618$, allora per $n=18$, arrotondando (eliminando la parte decimale) il valore ottenuto ho che $2583 \neq F_{18} =2584$.

## Fibonacci 2: metodo ricorsivo
```
algoritmo fibonacci2(intero n) -> intero
1	if(n <= 2) then return 1
2	else return fibonacci2(n-1) + fibonacci(n-2)
```
Cerchiamo di capire se questo algoritmo e' efficiente, e se puo' essere migliorato!

Introduco un **modello di calcolo** rudimentale: `ogni linea di codice eseguita, costa una unita' di tempo`. Con questo, ho un metodo universale per calcolare quanto tempo un generico calcolatore impiega ad eseguire l'algoritmo.

Al variare di `n`:
* se `n<=2` eseguo una sola riga
* se `n=3` eseguo 2 righe (la 1 e la 2), infine eseguo `fibonacci(2)` e `fibonacci(1)` che costano 1 riga ciascuno. Quindi ho $4$ righe eseguite

Chiamiamo $T(n)$, la funzione che esprime il **numero di righe eseguite** al variare di $n$.

Per qualsiasi valore di $n$, vengono eseguite almeno 2 righe, piu' le due chiamate ricorsive, quindi posso dire che:
$$
T(n) = 2 + T(n-1) + T(n-2)
$$

Pero' questa formula non mi permette di calcolare immediatamente il numero di righe eseguite. Per risolvere questo problema introduco l'**albero della ricorsione**, o più generalmente: **albero binario**. 

#inserireimmagine

Per ogni albero di questo tipo, costruito a partire dalla successione di Fibonacci, valgono le seguenti proprietà:
> [!note] Lemma 1.1: Numero di foglie nell'albero binario
> $$ \#foglie = F_{n}$$

> [!note] Lemma 1.2: Numero di nodi interni nell'albero binario
> $$\#nodi = \#foglie - 1$$

Entrambi i lemmi si dimostrano, "facilmente", per induzione.
### dimostrazione lemma 1.1
Verifico il **passo base**: per $F_{1}$ e $F_{2}$ ottengo degli alberi composti di un solo nodo, e quindi una sola foglia. 

Ora per induzione, $F_{k}$ e' vera, quindi l'albero costruito a partire da $F_{k}$ ha esattamente $F_{k}$ foglie. Ora devo dimostrare usando $F_{k}$ che $F_{k+1}$ sia vera.

Per definizione di Fibonacci: $F_{k+1} = F_{k} + F_{k-1}$ dove $F_{k}$ e $F_{k-1}$ sono i nodi sotto $F_{k+1}$ ( ossia la radice dell'albero). Per induzione $F_{k}$ e' vera, ed anche $F_{k-1}$ lo e'.
### dimostrazione lemma 1.2
Verifico il **passo base**: per $F_{1}$ e $F_{2}$ ho ancora una sola foglia, quindi per entrambi ho $1-1=0$.

Ora per induzione, ma al contrario. Prendo due alberi $T$ e $\hat{ T}$, quest'ultimo costruito togliendo una coppia di foglie qualsiasi da $T$. Ora indico con $i$ il numero di nodi interni e con $f$ il numero  di foglie, quindi:
* per l'albero $T$ ho che per $T: i = f-1$
* per l'albero $\hat{ T}$ vale che $i-1 = (f-1) -1$. 

Se all'ultima aggiungo uno a entrambi i membri, ottengo l'albero $T$.
## Fibonacci 3: programmazione dinamica
```
algoritmo fibonacci3(intero n) -> intero
1	sia Fib un array di n interi
2	Fib[1] <- Fib[2] <- 1
3	for i = 3 to n do
4		Fib[i] <- Fib[i-1] + Fib[i-2]
5	return Fib[n]
```

Sfrutto la tecnica della **programmazione dinamica**. Ora provo a trovare la funzione $T(n)$:
* Le linee 1, 2, e 5 vengono eseguite una sola volta, infatti si trovano fuori dal loop
* 3 e 4 vengono eseguite sicuramente per  $k \leq n$ volte.

Posso quindi affermare che: $T(n) \leq 2n + 3$, e vedere che, per esempio: $T(45) \leq 93$. Con questo nuovo algoritmo, Fibonacci di 45 si calcola in meno di 93 righe: e' 38 milioni di volte più veloce di `fibonacci2`.

* `fibonacci3` esegue un numero di linee che e' proporzionale ad $n$
* `fibonacci2` esegue un numero di linee che e' **esponenziale**

## Fibonacci 4: occupazione di memoria
```
fibonacci4(intero n) -> intero
	a <- 1, b <- 1
	for i = 3 to n do
		c <- a+b
		a <- b
		b <- c
```
Oltre al tempo (stimato a partire dal numero di linee di codice eseguite), e' importante capire anche quanta memoria occupa il nostro algoritmo, per ovvie ragioni. 

Con questo nuovo algoritmo posso calcolare Fibonacci usando solo 3 variabili, invece di occupare un array di `n` interi.

### Breve introduzione sulla notazione asintotica.
Conviene esprimere $T(n)$ usando la notazione asintotica, anche se tuttavia andremo a perdere un po' di **precisione**, ma guadagneremo in **semplicità**.

Questa notazione consiste nel:
* ignorare i valori costanti, e le costanti moltiplicative
* ignorare i termini di ordine inferiore

Alcuni esempi:
* $T(n) = 5n + 9 = O(n)$. $n$ e' l'unico termine che compare, e ignoro il 5 che lo moltiplica.
* $T(n) = \log n + n = O(n)$, dato che $\log(n)\leq n$ per qualsiasi $n$
* $T(n) = 2n^2 + 3n + 7 = O(n^2)$, dato che $n^2$ e' il termine di ordine più grande, inoltre non mi interessa il 2 che lo moltiplica.

Posso quindi riscrivere:
* $T(n) = 3F_{n}\text{, con: } T(n) = O(F_{n})$
* $T(n)=F_{n} \text{, con } T(n) = O(\phi^n)$

L'ultima vale perche nell'algoritmo `fibonacci1` avevamo stimato il valore di Fibonacci usando la formula $F_{n}= \frac{1}{\sqrt{ 5 }}(\phi^n - \hat{\phi^n})$, questa formula cresce esponenzialmente come $\phi^n$ o come $\hat{\phi}^n$.
## Fibonacci 5
Esiste un'algoritmo ancora migliore. 

> [!note] Lemma 1.3
> $$
\begin{bmatrix}
1 & 1 \\
1 & 0
\end{bmatrix}^n = 
\begin{bmatrix}
F_{n+1} & F_{n} \\
F_{n} & F_{n-1}
\end{bmatrix}
$$

Da questa formula posso ricavare l'algoritmo:
```
fibonacci5(intero n) -> intero
	M <- mat1
	for i = 1 to n-1 do
		M <- M * mat2
	return M[0][0]
```

con:
* `mat1` e' $\begin{bmatrix} 1 &0 \\ 0 & 1\end{bmatrix}$
* `mat2` e' $\begin{bmatrix} 1 & 1 \\ 1& 0\end{bmatrix}$

ma il temo di esecuzione e' ancora: $O(n)$, infatti il ciclo viene sempre eseguito $\leq n$ volte.

## Fibonacci 6
```
algoritmo fibonacci 6(intero n) -> intero
	A <- mat1
	M <- potenzaDiMatrice(A,n-1)
	return M[0][0]

funzione potenzaDiMatrice(mat A, intero k) -> matrice
1	if(k<=1) return MatIdentita
2	else M <- potenzaDiMatrice(A,[k/2])
3		 M <- M * M
4	if( k e' dispari ) then M <- M * A
5	return M
```

> `[k/2]` e' la parte intera di `k/2`.

Dove: 
* `MatIdentita` e' la matrice identità `2x2`
* `mat1` e' $\begin{bmatrix} 1 & 1 \\ 1& 0\end{bmatrix}$

Per analizzare la complessità di $T(n)$ devo guardare la funzione`potenzaDiMatrice`:
* eseguo istruzioni in tempo costante
* eseguo una chiamata ricorsiva con input $\frac{k}{2}$

Ora verifichiamo che la funzione `potenzaDiMatrice` ritorni la potenza corretta, per **induzione**:
* i casi base sono 0 e 1: che ritornano il valore corretto
* ora per esempio
	* in riga 2 eseguo `potenzaDiMatrice(A,[k/2])`che e' $= A^{k/2}$ (ipotesi induttiva)
	* in riga 3 eseguo `M <- M * M`, ovvero: $A^{ \lfloor k/2 \rfloor } \cdot A^{\lfloor k/2 \rfloor} = A^k$ oppure $A^{k-1}$ a seconda se k e' pari o dispari
	* la riga 4 mi aggiusta il valore se k dispari.

Ora invece andiamo a stimare $T(n)$, che e': $T(n) \leq T\left( \frac{n}{2} \right) + c$.

Sviluppando i termini ricorsivi:
* $T(n)\leq T\left( \frac{n}{2} \right) + c \leq T\left( \frac{n}{4} \right) + 2c$
* $T\left( \frac{n}{4} \right) + 2c \leq T\left( \frac{n}{8} \right) + 3c$
* $\dots$
* $\dots \leq i\cdot c + T\left( \frac{n}{2^i} \right)$

per $i=\log_{2} k$:
$$
T(n) \leq c \cdot \log_{2}n + T(1) = O(\log_{2}n) = O(\log n)
$$
dove $\log_{2} k$ e' il valore che ci porta al caso base $T(1)$.

> oss: mentre $n$ diminuisce, $i$ aumenta fino a $\log_{2}k$

> Abbiamo dimostrato il Lemma 1.4.
### Lemma 1.4
> [!note] Lemma 1.4
> Data la relazione di ricorrenza $T(n)$: 
> * $T(n) = O\left( \frac{n}{2} \right) + O(1)$
> * $T(n) = O(1) \text{ per } n \leq 1$ (caso base)
> 
> questa ammette come soluzione: $T(n) = O(\log n)$.



