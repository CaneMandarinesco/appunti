## `HeapSort`
Ad ogni iterazione cerco il numero massimo e lo butto in fondo, ma uso una **struttura dati efficiente**, mi permette di trovare il massimo in $O(\log n)$. 

> [!note] tipo di dato
> specifica il problema che voglio risolvere. Cosa voglio tenere in memoria? E quali operazioni voglio fare su questi oggetti? Per esempio su un **dizionario** posso tenere un insieme di oggetti con una chiave associata: posso fare la ricerca per chiave, inserire elementi ed eliminarli.

>[!note] struttura dati
> specifica la soluzione al problema da risolvere. E' l'implementazione degli oggetti di memoria, e specifica gli algoritmi che implementano le operazioni per il tipo di dato.

Nel caso dell'heap sort, la mia struttura dati deve:
* dato un array devo generare velocemente la **struttura dati `H`**
* trovare il massimo in `H`
* cancellare il più grande oggetto da `H`

Tipo di dato associato al problema: **coda con priorità**.

> [!note] Albero d-ario
>  ogni albero ha al più `d` figli, per esempio un albero con `d=2` e' binario. 

> [!note] Albero completo
> ogni nodo fino al livello $n-1$ ha al piu' `d` figli


### struttura dati heap
Ha un albero binario associato:
* e' completo fino al penultimo livello, i rami più lunghi si trovano a sinistra (**struttura rafforzata**).
* gli elementi di S sono memorizzati nei nodi dell'albero
* `chiave(padre(v)) >= chiave(v)` per ogni nodo v diverso dalla radice

L'heap mi garantisce delle proprietà per definizione:
1. il massimo si trova nella radice
2. l'albero ha altezza $O(\log n)$
3. possono essere rappresentati con un array di grandezza `n`

#### dim. proprieta' 2
$$
n \geq 1 + \sum_{i=0}^{h-1}2^i = 1+2^h-1=2^h
$$
quindi: 
$$
h \leq \log_{2}(n)
$$
### implementazione
* il primo indice lo lascio vuoto! 
* leggo l'albero dall'alto verso il basso, da sinistra verso destra e metto i valori nell'array
* la radice si trova in posizione: 1
* i figli allora si trovano in posizione: 2,3.
* i figli di 2 si trovano in 4,5
* i figli di 3 si trovano in 6,7

allora:
* $sin(i) = 2i$ (figlio di sinistra)
* $des(i) = 2i+1$ (figlio di destra)
* $\text{padre}(i) = \frac{i}{2}$

* `fixheap`: assume che il sotto-albero sinistro e destro siano degli heap

```
fixHeap(i,A)
	s = sin(i)
	d = des(i)
	if(s <= heapsize[A] e A[s] > A[i])
	then massimo = s
	else massimo = i

	if (d <= heapsize[A] e A[d] > A[massimo])
	then massimo = d
	if (massimo != i)
	then scambia A[i] e A[massimo]
		fixHeap(massimo,A)
```


```
heapify(heap H)
	if (H non e' vuoto) then
		heapify(sottoalbero sinistro di H)
		heapify(sottoalbero destra di H)
		fixheap(radice di H,H)
```

### analisi di heapify
> caso di albero completo
$$
T(n) = 2T\left( \frac{n}{2} \right) + \log(n)
$$
Usando teorema master: $T(n)=\Theta(n)$.

Nel caso di un albero non completo?
* sia $h$ altezza di heap con $n$ elementi
* Sia $n' \geq n$ l'intero tale che l'heap con $n'$ ha:
	* altezza $h$
	* e' completo fino all'ultimo livello

Allora vale che: $T(n) \leq T(n')$ e $n' \leq 2n$, quindi vale ancora la stima del **teorema master.

 > Se l'albero ha $2^h$ nodi e lo devo completare, posso aggiungere al massimo $2^h-1$ nodi! Al più il numero di nodi raddoppia.
 
### max-heap e min-heap
E se volessi estrarre velocemente il minimo? richiedo che: `chiave(padre(v)) <= chiave(v)`. per ogni $v$ diverso dalla radice, e perché prima abbiamo fatto un max-heap e non un min-heap?

### heap sort
* costruisco un heap tramite heapify
* Estrae ripetutamente il massimo per n-1 volte.
* Cerco di usare l'heap per contenere l'output mentre lo sto ordinando

```
heapSort(A)
	Heapify(A) 
	Heapsize[A]=n
	for i=n down to 2 do:
		scambia A[1] e A[i]
		Heapsize[A] = Heapsize[A]-1
		fixHeap(1,A)
```

* $T = O(n) + O(n \log n)$.
se avessi usato l'algoritmo min heap, avrei un array ordinato al contrario.