 ### Esercizio
Analizzare la **complessità nel caso medio** del primo algoritmo di pesatura (`Alg1`) presentato nella prima lezione. Rispetto alla distribuzione di probabilità sulle istanze, si assuma che la moneta falsa possa trovarsi in modo equiprobabile in una qualsiasi delle n posizioni.


Devo quindi usare la formula per calcolare il caso medio:
$$
\sum Pr(I) \cdot \#pesate(I) = \sum_{j=1}^n Pr(\text{moneta falsa e' in posizione j in I}) \cdot \# \text{ pesate}(I) 
$$
$$
= \left(  \frac{1}{n} \right) \left( 1 + \sum_{j=2}^n (j-1) \right) = \left( \frac{1}{n} \right)\left(  1+ \frac{n \cdot (n-1)}{2} \right) = \frac{1}{n} + \frac{n-1}{2}
$$
in $\left(  \frac{1}{n} \right) \left( 1 + \sum_{j=2}^n (j-1) \right)$: 
* $\frac{1}{n}$ e' $Pr(I)$, che può essere portato  fuori dalla sommatoria.
* dato che `pesate(I)=1`, per j=1 lo porto fuori dalla sommatoria
* $\sum_{j=2}^n (j-1)$ e' la somma del numero di pesate per diverse disposizioni della moneta falsa.

### Problema simile: ricerca di un elemento in una lista non ordinata
```python
AlgRicercaSequenziale(array L, elem x) -> intero
	n = lunghezza di L
	j = 1
	for i=1 to n do
		if(L[i]=x) then return i # trovato!
	return -1 # non trovato
```

* caso peggiore: $\Theta(n)$

esiste un algoritmo più efficiente?  no.
* $T_{\text{worst}} (n) = \Theta(n)$
* $T_{\text{avg}} (n) =\sum_{i=1}^n \frac{1}{n}i = \frac{1}{n} \frac{n(n-1)}{2} = \frac{n-1}{2} = \Theta(n)$, come fatto similmente nell'esercizio di prima.

### Ricerca di un elemento in un array ordinato: divide et impera
Qui vediamo un'implementazione ricorsiva dell'algoritmo:
```
algoritmo RicercaBinariaRic(array L, elem x, int i, int j) -> intero
	if(i>j) then return -1
	m = [(i+j)/2] # parte intera
	if (L[m]=x) then return m
	if (L[m]>x) then return RicercaBinariaRic(L,x,i,m-1)
				else return RicercaBinariaRic(L,x,m+1,j)
```
Questo algoritmo si basa sul fatto che gli elementi nell'array sono **ordinati**, dal più piccolo al più grande.

Es: `L=[0,1,2,25,100,102,103]`. Vedremo più avanti come ordinare un array, in modo da sfruttare questo algoritmo per ottimizzare il caso in cui bisogna cercare più volte, diversi valori, ma nello stesso array. 

Dove, a meno del caso base che e' costante, vale che: $T(n) = T\left( \frac{n}{2} \right) + O(1)$ che abbiamo già visto essere: $T(n)=O(\log n)$

Questo tipo di algoritmi, dove la chiamata ricorsiva viene dimezzata, usano una tecnica chiamata: **divide et impera**, ossia divido il problema in piccoli sotto-problemi più facili da risolvere, generalmente *diminuendo l'input delle chiamate ricorsive ad ogni step*. In particolare abbiamo effettuato una **ricerca binaria**.

Le seguenti definizioni non sono state viste a lezione, ma sono riportate per completezza:
* caso **migliore**: il mio elemento che cerco e' proprio $L\left[ \frac{n+1}{2} \right]$, quindi: $T_{\text{best}}(n) = 1$
* caso **peggiore**: supponendo che $n$ sia una potenza di 2, il caso peggiore e' $T_{\text{ worst}}(n) = O(\log(n))$. Ovvero quando vado a dividere per il massimo numero di volte possibili, l'array dato in input.
* caso **medio**: $T_{\text{avg}} (n) = \sum_{i=1}^{\log n} \frac{1}{n} \cdot i \cdot 2^{i-1} = \dots = \log n -1 + \frac{1}{n}$
# Analisi di algoritmi ricorsivi
## Metodo ricorsivo
Con questo metodo, andremo a srotolare la ricorrenza fino ad arrivare al caso baso, ed andremo a stimare 
### Es. 2.3
$$
T(k) = \begin{cases}
c + T\left( \frac{k}{2} \right) \text{ se k > 1} \\
1 \text{ se k = 1}
\end{cases}
$$
Dove per $k=n, \frac{n}{2}, \frac{n}{4}, \dots$ ho che:
$$
T(n) = c + T\left( \frac{n}{2} \right);\;\;\;\; T\left( \frac{n}{2} \right) = c+ T\left( \frac{n}{4} \right); \;\;\;\;T\left( \frac{n}{4} \right) = c+T\left( \frac{n}{8} \right)
$$

Dove sostituendo:
$$
T(n) = c+T\left( \frac{n}{2} \right) = 2c + T\left( \frac{n}{4} \right) = 3c +T\left( \frac{n}{8} \right) = \dots
$$
Ci chiediamo quindi: dopo quante sostituzioni si arresta la ricorrenza per un generico valore $n$? Sappiamo che il caso base e' $T(1)$, allora:
$$
3c + T\left( \frac{n}{8} \right) = \dots = i \cdot c + T\left( \frac{n}{2^i} \right)
$$
dove per $i=\log_{2}n$, ho che $T\left( \frac{n}{2^i} \right) = T(1)$ (caso base). Posso concludere che: 
$$
T(n) = \log n \cdot c + T(1) = O(\log n)
$$
### Es. 2.4
$$
T(n)= \begin{cases}
9T\left( \frac{n}{3} \right) + n \text{ se n > 1}\\
1 \text{ se n = 1}
\end{cases}
$$
...

--- 

Questo metodo non funziona pero' per ricorsioni come: 
$$
T(n) = T(n-1) + T(n-2) + 1
$$
dove srotolando, il problema diventa sempre più complicato, infatti:
$$
= T(n-2) + T(n-3) + T(n-3) + T(n-4) + 2 + 1
$$

### Esempio usando l'albero della ricorsione: 
Per: $T(n) = T(n/3) + T(2n /3) + n$

* ogni nodo $T(n)$ ha costo $n$
* $T(n)$ ha due nodi figli: $T(n /3)$ e $T(2 n/ 3)$

Costruendo l'albero noto che: 
* il ramo che si sviluppa a sinistra finisce prima, posso dare un **lower bound**
* il ramo che si sviluppa a destra e' il piu lento di tutti, posso dare un **upper bound**

eventualmente se lower e upper bound coincidono posso determinare $\Theta$.

Infatti il ramo a sinistra e' alto $\log_{3}n$ nodi, mente quello a destra e' alto: $\log_{3/2}n$ nodi. E quindi, sapendo che il costo computazionale di un'intero ramo e' $\#nodi \cdot f(n)$, dove $f(n)$ e' il costo di un nodo:
* $T(n) = \Omega(\log_{3}n \cdot n) = \Omega(\log n \cdot n)$
* $T(n) = O(\log_{3 /2}n \cdot n) = O(\log n \cdot n)$
* dato che $\Omega(\log n \cdot n) = O(\log n \cdot n)$, posso concludere che $T(n) = \Theta(\log n \cdot n)$

## Metodo della sostituzione

## teorema fondamentale delle ricorrenze
Supponendo di stare analizzando una ricorrenza del tipo `divide et impera`:
$$
T(n) = a\cdot T(n/b) + f(n)
$$
dove:
* $a$ indica in quanti sotto problemi sto creando
* $\frac{n}{b}$ indica la complessità del nuovo sotto problema
* $f(n)$ e' il costo di esecuzione del problema corrente.

Posso costruire un'**albero della ricorsione** che ha le seguenti proprietà:
* (2.3): $\frac{n}{b^i}$ e' la dimensione di ogni singolo sotto problema al livello $i$ dell'albero
* (2.4): $f\left( \frac{n}{b^i} \right)$ indica il tempo di esecuzione di un nodo al livello $i$
* (2.5): $i =\log_{b} n$ indica il numero di nodi di un albero di $n$ livelli
* (2.6): $a^i$ e' il numero di nodi dell'albero.
## Teorema 2.1 (Teorema fondamentale delle ricorrenze o Teorema Master)
Ora questo tipo di problema ha soluzione a seconda del caso in cui ci troviamo:
1. $\Theta(n^{\log_{b}a})$ se $f(n) = O(n^{\log_{b}a-\epsilon})$ per $\epsilon > 0$
2. $\Theta(n^{\log_{b}a}\log n)$ se $f(n) = O(n^{\log_{b}a})$ per $\epsilon > 0$
3. $\Theta(f(n))$ se $f(n) = \Omega(n^{\log_{b}a+\epsilon})$ per $\epsilon> 0$ e $a \cdot f( \frac{n}{b} ) \leq c \cdot f(n)$ per $c<1$

### Esempio
* $T(n) = T\left( \frac{n}{2} \right) + O(1)$ ricade nel caso 2 dato che: (con $a=1, b=2$) $f(n) = O(n^{\log_{2}1}) = O(1)$. Quindi ho che $T(n) = n^{\log_{2} 1} \log(n) = \log(n)$
* $T(n) = 9T(n/ 3) + n$, ho che $a=9$ e $b=3$, $n^{\log_{9}3} = n^2$ dove $n^{2-\epsilon} = O(n)$ che per $\epsilon = 1$ e' vera. Quindi: $\Theta(n^2)$.
