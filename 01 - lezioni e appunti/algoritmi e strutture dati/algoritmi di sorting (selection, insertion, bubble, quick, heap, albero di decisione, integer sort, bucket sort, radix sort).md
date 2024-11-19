Quando progettiamo un algoritmo di sorting, cosa ci dobbiamo aspettare dalla sua complessità temporale? Come misuriamo i tempi di questi algoritmi?

> un'algoritmo di sorting e' basato su confronti se e solo se utilizza operazioni del tipo `<,<=,>=,>` per effettuare delle decisioni.

> vedremo poi alcuni algoritmi che non si basano su confronti.

Vedremo algoritmi che eseguono al caso peggiore in $O(n \log n)$, mentre altri in $O(n^2)$.

Possiamo dare una **delimitazione inferiore** al problema del sorting per algoritmi che si basano su confronti. Anticipo già che questa delimitazione e': $\Omega(n \log n)$.

Introduciamo l'**albero di decisione**: una struttura che si applica a *tutti gli algoritmi basati su confronti* e descrive tutte le sequenze possibili che un algoritmo potrebbe seguire per determinate istanze di grandezza $n$ (esiste un albero per $n=3$ ma esiste un'altro albero per $n=4$, ecc....)

* Proprieta 4.1: la sequenza di confronti eseguita da una particolare istanza e' rappresentata dal **cammino** che va dalla radice dell'albero fino ad una delle sue foglie.
* Proprieta 4.2: il numero di confronti al caso peggiore e' **pari all'altezza dell'albero**
* Proprieta 4.3: il numero di confronti al caso medio e' l'**altezza media dell'albero** (lunghezza del cammino dalla radice fino a tutte le foglie)

> [!note] Lemma 4.1 un albero di decisione ha almeno $n!$ foglie
> Un albero di decisione per l'ordinamento di $n$ elementi **contiene almeno** $n!$ foglie. 
> Dimostrazione: il cammino dalla radice ad ogni foglia corrisponde ad una soluzione del problema (dato che ho finito i confronti da fare), ed **ogni soluzione non e' altro che una permutazione** dell'array iniziale. Quindi devo avere almeno $n!$ foglie.

#### Delimitazione inferiore al caso peggiore
>[!note] Proprietà 4.2: l'altezza di T e' almeno $\log_{2}k$
> Con T albero i cui nodi interni hanno almeno 2 figlie e con $k$ il numero delle foglie, allora l'altezza di $T$ e' almeno $\log_{2}k$. 

Dimostrazione di 4.2, per Induzione: con $h(k) = \text{ altezza di un albero con k foglie}$. 
* caso base: $k=1$ allora $0 \geq \log_{2} 1$ (dato che ha altezza $0$)
* ipotesi induttiva: $h(i)\geq \log_{2}i$ con $i \leq k-1$. Uno dei sotto alberi pero' ha almeno meta foglie quindi: 
* $h(k) \geq 1+h\left( \frac{k}{2} \right)$
* per ipotesi induttiva: $h\left( \frac{k}{2} \right) \geq \log_{2}\left( \frac{k}{2} \right)$
* mettendo insieme i pezzi: $$ h(k) \geq 1 + h\left( \frac{k}{2} \right) \geq 1 + \log_{2}\left( \frac{k}{2} \right) = 1 + \log_{2} k - \log_2 2 = \log_{2}k $$
![[Pasted image 20241031172400.png]]

### Teorema 4.1: numero di confronti per un algoritmo al caso peggiore
>[!note] Teorema 4.1 Il numero di confronti al caso peggiore
>Il numero di confronti al caso peggiore per un algoritmo basato su confronti e': $\Omega(n \log n)$

Dimostrazione Teorema 4.1: 
* usando l'approssimazione di Sitrling $n! = \sqrt{ 2\pi n }\left( \frac{n}{e} \right)^n$
* sappiamo che il numero di confronti al caso peggiore e': $\geq \log n!$ (ossia logaritmo del numero di foglie di un albero con $n$ elementi, combinando le proprieta' precedenti)
* $\log n! \sim n \log n - 1.4427n + \frac{1}{2} \log n + 0.826 = \Omega(n \log n)$ 

## algoritmi che ordinano in tempo quadratico
selection sort e insertion sort si basano su una tecnica **induttiva** o **incrementale**. si suppone che i primi $k$ elementi siano ordinati che e bisogni ordinare l'elemento $k+1$, fino ad arrivare ad $n$. 

> [!warning] 
> Misuriamo la complessità basandoci sul numero di confronti che questi fanno.
### selection sort
* al generico passo gli elementi $A[k+1] \dots A[n]$ non sono ordinati. 
* scegli il minimo degli $(n-k)$ elementi non ordinati e buttalo nella posizione $k+1$
```python
algoritmo selectionSort(array A)
1. 	for k = 0 to n - 2 do # eseguito n-1 volte
2. 		m = k+1
3. 		for j = k+2 to n do
4. 			if(A[j] < A[m]) then m = j
5. 		scambia A[m] con A[k+1]
```

questo algoritmo ordina in loco in tempo:
$$
\sum_{k=0}^{n-2}(n-k-1) = \sum_{i=1}^{n-1}i = \Theta(n^2)
$$
> dove la prima sommatoria e' uguale alla seconda: la prima somma da $n-1$ fino a 1, la seconda somma da 1 fino ad $n-1$.
### insertion sort
* prendi l'elemento $k+1$ e ordinalo rispetto ai primi $k$ elementi
```python
algoritmo insertionSort(array A)
1.	for k = 1 to n - 1 do
2.		x = A[k+1]
3.		for j = k downto 0 do # loop che fa i confronti
4.			if( j>0 and A[j] <= x) then break # ho trovato fino a dove devo spostare!
5.		if(j < k) then # devo scambiare tutti gli elementi!
6.			for t = k downto j+1 do A[t+1] = A[t]
7.			A[j+1] = x
```
quest algoritmo ordina in loco in tempo:
$$
\sum_{k=1}^{n-1}(k+1) = \sum_{k=1}^{n-1}k + (n-1) = \Theta(n^2)
$$
Possiamo riassumere l'algoritmo cosi:
* L'algoritmo prende l'elemento $k+1$ e lo mette in `x`
* Trova il primo elemento piu piccolo di $x$ scorrendo l'array da $k$ a 0.
	* Posso fermarmi al primo elemento piu piccolo dato che da 0 a $k$ l'array e' ordinato
* Una volta trovato l'indice posso procedere a scambiare, gli elementi ( righe da 5 a 7)
### bubble sort
```python
algoritmo bubbleSort(array A)
	for i = 1 to (n-1)
		for j = 2 to (n-i+1)
			if(A[j-1] > A[j]) then scambia A[j-1] e A[j]
		if (non ci sono stati scambi) then break
```

Questo algoritmo e' molto semplice da scrivere e implementare. Al passaggio `k`-esimo sono sicuro che gli elementi $A[n-k+1] \dots A[n]$ sono sicuramente ben ordinati. Per esempio alla prima iterazione l'algoritmo posiziona correttamente l'elemento piu grande, alla seconda il 2o piu grande e cosi via... (infatti gli oggetti piu grandi risalgono nell'array come una bolla!)

$$
\sum_{k=1}^{n-1} (n-k) = \Theta(n^2)
$$
> se ad ogni iterazione del ciclo piu esterno posiziono correttamente almeno un elemento, allora il numero di confronti al prossimo ciclo diminuisce di 1. 

## algoritmi divide et impera

### merge sort
```python
algoritmo mergeSort(array A, indici i e f):
	if (i>=f) then return A # controlla
	m = (i+f)/2
	mergeSort(A,i,m)
	mergeSort(A,m+1,f)
	merge(A,i,m,f)
```

```python
algoritmo merge(array A, interi i1, f1 e f2)
	sia X array ausiliario di lunghezza f2-i1+1
	i = 1
	i2 = f1 + 1
	while(i1 <= f1 e i2 <= f2) do
		if(A[i1] <= A[i2])
		then X[i] = A[i1]
			i++, i1++
		else X[i] = A[i2]
			i++, i2++
	if(i1 < f1) then copia A[i1:f1] alla fine di X
	else copia A[i2:f2] alla fine di X
	copia X in A[i1:f2]
```

Questo algoritmo e' di tipo **divide et impera**:
* **impera**: fai il merge in $l_{1} + l_{2} -1$ confronti
* **divide**: faccio chiamate ricorsive

La procedura merge prende in input due liste ordinate (ossia `A[i1:f1] e A[i2:f2]`), e con due cursori: $l_{1}$ e $l_{2}$ scorro le due liste. Alla prima iterazione prendo l'elemento piu piccolo tra $l_{1} \text{ e } l_{2}$ e decido di conseguenza chi copiare per primo e quale cursore far avanzare. il cursore `i` si occupa di gestire l'array ausiliario `X` che contiene il merge delle due liste.

la procedura merge viene eseguita in tempo $\Theta(l_{1}+l_{2})$: 
* copio fino ad esaurire $l_{1}$ o $l_{2}$
* copio quello che rimane di uno dei due, quindi copio gli elementi rimanenti in $l_{1}$ o in $l_{2}$

usando il **teorema master** ottengo che: $T(n)=O(n \log n)$.

Per quanto riguarda la complessità spaziale, questa e' $\Theta(n + \log n) = \Theta(n)$:
* $n$ perché ogni procedura merge richiede un array ausiliario
* $\log n$ perché faccio $\log n$ assegnazioni costanti, date dalle chiamate ricorsive

> [!note] oss.
> ho trovato un'algoritmo migliore di ordinamento rispetto a `insertion sort`, `selection sort` e `bubble sort`. Questo infatti al caso peggiore esegue $O(n \log n)$ confronti. Ma in termini di memoria e' meno efficiente dell'`heap sort` dato che usa della memoria ausiliaria.
### Quick Sort
```python
procedura partition(Array A, indici i e f) -> indice
# ritorna la posizione del perno
	x = A[i]
	inf = i
	sup = f+1
	while(true) do
		do inf++ while(inf <= f e A[inf] <= x)
		do sup++ while(A[sup] > x)
		if(inf < sup) then scambia A[inf] e A[sup]
		else break
	scambia A[i] e A[sup]
	return sup

algoritmo quickSort(array A, indici i e f)
	if(i>=f) then return
	m = partition(A,i,f)
	quickSort(A,i,m-1)
	quickSort(A,m+1,f)
	# merge(...) -> non serve!
```
Anche questo si basa su **divide et impera**:
* **divide**: scegli un perno $x$ e dividi in due liste con elementi $\leq x$ e $> x$
* **impera**: concatena le due liste ( senza dover ordinare, come accade nel **merge sort** )
* L'algoritmo ordina in loco, senza usare spazio aggiuntivo, infatti faccio solo chiamate ricorsive e partiziono, ma non ho bisogno di fare un `merge` delle soluzioni ricorsive.

L'efficienza dell'algoritmo dipende dalla scelta del perno. Nel codice, per ogni problema e sotto problema il perno e' il primo elemento dell'array mentre.

Ad ogni iterazione divido l'array in 2: $A_{1} \text{ e } A_{2}$ rispettivamente di grandezza $a \text{ e } b$. Ora al caso peggiore quello che potrebbe succedere se scelgo sempre il perno peggiore e' che: $a=0 \text{ e } b= n-1$. La ricorsione lineare e' quindi:
$$
T(n) = n-1 + T(0) + T(n-1) = O(n^2)
$$
Al caso peggiore non e' ottimale. Proviamo a randomizzare la scelta del perno:
$$
T(n) = \sum_{a=0}^{n-1} \frac{1}{n} \cdot (n-1 + T(a) + T(n-a-1))
$$
notando che $T(a)$ e $T(n-a-1)$ sono la stessa sommatoria:
$$
T(n) = n-1 + \sum_{a=0}^{n-1} \frac{2}{n} C(a) \leq 2n \log n
$$
La dimostrazione e' complicata, si stima maggiorando la sommatoria con un integrale.

> [!note] oss.
> l'algoritmo al caso peggiore performa peggio del merge sort e dell'heap sort. Ma può essere migliorato con altre tecniche rendendolo ottimo al caso medio, tenendo testa ad altri algoritmi.

## Heap Sort
Usiamo lo stesso approccio incrementale ma:
* uso una struttura dati efficiente
* seleziona gli elementi dal più grande al più piccolo

Introduciamo la differenza tra **struttura di dati** e **tipo di dato**:

> [!note] tipo di dato
> specifica il problema che voglio risolvere. Cosa voglio tenere in memoria? E quali operazioni voglio fare su questi oggetti? Per esempio su un **dizionario** posso tenere un insieme di oggetti con una chiave associata: posso fare la ricerca per chiave, inserire elementi ed eliminarli.

>[!note] struttura dati
> specifica la soluzione al problema da risolvere. E' l'implementazione degli oggetti di memoria, e specifica gli algoritmi che implementano le operazioni per il tipo di dato.

Nel caso dell'heap sort, la mia struttura dati deve:
* dato un array devo **generare velocemente** la **struttura dati `H`**
* trovare il massimo in `H`
* cancellare il più grande oggetto da `H`

Prima di parlare dell'heap sort bisogna introdurre la struttura dati che useremo ossia l'**abero heap**, che e' un albero binario.

>[!note] albero d-ario
> un albero d-ario e' un grafo dove i nodi interni hanno al piu $d$ figli. Ne consegue che un albero con $d=2$ e' un **albero binario**. 
> - E' **completo** se tutti i nodi interni hanno esattamente $d$ figli.

Ogni istanza della struttura heap, ha un suo albero binario associato con le seguenti proprietà:
* e' completo fino al penultimo livello, i rami più lunghi si trovano a sinistra (**struttura rafforzata**).
* gli elementi di S sono memorizzati nei nodi dell'albero
* `chiave(padre(v)) >= chiave(v)` per ogni nodo v diverso dalla radice

L'heap mi garantisce delle proprietà per definizione:
1. il massimo si trova nella radice
2. Prop. 4.4: l'albero ha altezza $O(\log n)$
3. possono essere rappresentati con un array di grandezza `n`

***Dimostrazione della proprietà 4.4***, un heap con $n$ nodi ha altezza $\Theta(\log n)$.
Sappiamo che $h-1$ livelli sono sicuramente completi, quindi il numero di nodi **e' almeno**:
$$
\sum_{i=0}^{h-1} 2^i= 2^h - 1
$$
sappiamo anche che: $$2^h-1 \leq n \leq 2^{h+1} -1 $$
> $2^{h+1}-1$ e' l'albero completo di $h$ livelli.
> ossia che il numero di noti e' compreso

segue che $h=\Theta(\log n)$ (boh sinceramente non l'ho capita)

***Dimostrazione vista in classe***: l'albero e' sicuramente completo fino al livello $h-1$ e ha almeno un nodo al livello $h$. Quindi:
$$
n \geq 1 + \sum_{i=0}^{h-1}2^i = 1+ 2^h - 1 = 2^h \to h \leq \log_{2}n
$$

in generale nella struttura **heap** vale che:
* il valore associato ad un nodo e' sempre più grande del valore 
### implementazione
Nel nostro array in input, per semplicità, lasciamo il primo slot "vuoto" e andiamo a definire:
* $sin(i) = 2i$, la funzione che dato l'indice di un nodo mi da l'indice del figlio sinistro nell'array.
* $des(i) = 2i +1$, la funzione che dato un nodo mi da l'indice del figlio destro nell'array.
* $\text{padre}(i) = \frac{i}{2}$ (parte intera), la funzione che dato un nodo, mi da l'indice del padre nell'array.

Come posiziono i valori dell'albero dentro l'array in modo da rispettare le definizioni date qui sopra? *Leggo l'albero da sinistra verso destra, dall'altro verso il basso e inserisco i valori uno ad uno nell'array*, di conseguenza:
* la radice si trova in posizione 1
* i figli della radice si trovano in posizione 2 e 3
* i figli del nodo in posizione 2 si trovano in 4,5
* i figli del nodo in posizione 3 si trovano in 6,7
* ecc...

>[!note] oss.
> l'array associato e' sempre completo, nel senso che se un albero ha $h$ livelli, anche se questo non e' completo, l'array associato ha $2^{h+1}-1$ slot al suo interno.

Introduciamo la procedura `fixHeap`, questa mi aggiusta l'heap in caso la radice o un generico nodo $v$ non siano stati propriamente assegnati. Quindi assume che l'albero sinistro e destro sotto la radice siano ordinati

```python
algoritmo fixHeap(nodo v, heap H)
	if(v e' una foglia in H) then return
	else
		sia u il figlio di v con chiave massima
		if(chiave(v) < chiave(u)) then
			scambia chiave(v) e chiave(u)
			fixHeap(u,H)
```

l'algoritmo viene eseguito in tempo $O(\log n)$

Ora passiamo all'algoritmo che si occupa di estrarre il massimo dalla struttura, e di aggiustare l'heap di conseguenza:
* copia nella radice la chiave più a destra nell'ultimo livello
* rimuovi la foglia 
* esegui fix heap per portare il nuovo massimo alla radice

Ora abbiamo bisogno di costruire il nostro heap: dato un array di elementi, sfruttiamo la **struttura compattata**, ossia tutte le foglie sono raggruppate insieme a sinistra in caso l'albero non sia completo. Questo tipo di struttura sara fondamentale per ordinare in loco.
```python
heapify(heap H)
	if (H non e' vuoto) then
		heapify(sottoalbero sinistro di H)
		heapify(sottoalbero destro di H)
		fixHeap(radice di H, H)
```

Ora, una volta costruita la struttura dati, posso farne il sort in questo modo:
```python
algoritmo heapSort(array A) -> Lista
	# crea un heap H dall'array A
	sia X una lista vuota
	while(H non e' vuoto) do
		rimuovi il massimo da H, aggiungilo in coda a X
		# e' sottointeso che io debba fare fixHeap
	return X
```

## Delimitazioni inferiori e superiori di algoritmi e problemi

### complessità di un algoritmo

> [!note] def. $A$ ha complessità $O(f(n))$
> Un algoritmo $A$ ha complessità $O(f(n))$ rispetto ad una certa risorsa di calcolo, se la quantita $r(n)$ di risorsa usata da $A$ al caso peggiore su istanze di dimensione $n$ verifica la relazione $r(n) = O(f(n))$

>[!note] def. $A$ ha complessità $\Omega (f(n))$
> Un algoritmo $A$ ha complessità $\Omega(f(n))$  rispetto ad una certa risorsa di calcolo se la quantità $r(n)$ di risorsa usata da $A$ al caso peggiore su istanze di dimensione $n$ verifica la relazione $r(n)=\Omega(f(n))$

### complessità di un problema 
>[!note] def. $P$ ha complessità $O(f(n))$
>$P$ ha complessità $O(f(n))$ rispetto ad una risorsa di calcolo se **esiste** un algoritmo che risolve $P$ il cui costo di esecuzione rispetto a una risorsa e' $f(n)$

>[!note] def. $P$ ha complessità $\Omega(f(n))$
>$P$ ha complessità $\Omega(f(n))$ rispetto ad una risorsa di calcolo se **ogni algoritmo** che risolve $P$ il cui costo di esecuzione rispetto a una risorsa e' $f(n)$

>[!note] Ottimalita di un algoritmo
>Dato un problema $P$ con complessita $\Omega(f(n))$ rispetto ad una risorsa di calcolo, un algoritmo che risolve $P$ e' ottimo se ha costo di esecuzione $O(f(n))$ rispetto a quella risorsa

Esempio:
* Upper Bound: $O(n^2)$, vale per: Insertion Sort, Selection, Quick Sort, Bubble Sort
* Upper Bound migliore: $O(n \log n )$: Merge Sort e Heap Sort
* Lower Bound (**ovvio**): $\Omega(n)$, almeno una volta devo leggerli tutti

Tra Upper Bound e Lower Bound c'e' un gap di $\log n$, mi posso avvicinare ancora di più ad $n$?

>[!note] oss.
> tutti gli algoritmi visti usano istruzioni di confronto per decidere cosa fare.

>[!note] Teorema
> Ogni algoritmo basto su confronti che ordina $n$ elementi impiega al caso peggiore $\Omega(n \log n)$ confronti. Vuol dire quindi che al caso peggiore, il minimo di confronti che faccio cresce come $n \log n$, non posso fare di meglio!

> Da questo teorema ne consegue che Heap Sort e Merge Sort sono degli ottimi algoritmi. 

Tutti gli algoritmi che operano per confronto possono essere descritti grazie a degli **alberi di decisione**, dato che un generico algoritmo, lavora nel seguente modo:
* confronta $a_{i}$ e $a_{j}$
* a seconda del risultato ordina e/o decidi il confronto successivo da eseguire.

In un albero della decisione, l'output non e' altro che una permutazione degli elementi nell'array che sto ordinando, quindi avrò almeno $n!$ foglie.

Ecco alcune osservazioni sull'albero di decisione
* L'albero non e' associato ad un problema
* L'albero non e' associato ad un **unico algoritmo** algoritmo
* descrive le sequenze di confronti possibili su una determinata dimensione dell'istanza.

Per una particolare istanza, i confronti eseguiti dall'algoritmo sono rappresentati dal cammino **radice - foglia**:
* il caso peggiore, e' il cammino più lungo possibile.
* il numero di confronti possibili e dato dall'altezza dell'albero
* già anticipato prima: un'albero di decisione ha almeno $n!$ foglie.


>[!note] Lemma
>Un albero binario $T$ con $k$ foglie ha altezza almeno $\log_{2}k$. (diverso dal teorema visto prima dove se un albero ha $n$ nodi allora ha altezza $\Theta(\log n)$)

**Dimostrazione (per induzione)**: 
* $k=1$ ha altezza almeno $\log_{2} 1 = 0$. 
* caso induttivo: $k>1$
* considera il nodo intero `v` più vicino alla radice che ha due figli.
* allora $v$ ha almeno un figlio $u$ che e' radice di un sotto albero con almeno $k /2$
* quindi $T$ ha altezza almeno $1+\log_{2} \frac{k}{2} = 1+\log_{2}k - \log_{2}2 = \log_{2}k$

Il lower bound: $\Omega(n \log n)$ per qualsiasi algoritmo di ordinamento e' stato dimostrato nel [[algoritmi di sorting (selection, insertion, bubble, quick, heap, albero di decisione, integer sort, bucket sort, radix sort)#Teorema 4.1 numero di confronti per un algoritmo al caso peggiore|Teorema 4.1]] (usa l'approssimazione di stirling)

Non si può nemmeno ordinare $n$ "interi piccoli" in meno di $n \log n$ usando un algoritmo per confronti.

### Integer Sort
> Algoritmo che **non va per confronti**

Algoritmo:
* prendi l'array in input e scorrilo
* per ogni valore `x=A[i]` letto fai `L[x]++`
* scorri l'array `L`, per ogni `L[i]>0`, metti il valore di `i` in `A[k]`:
	* `k` parte da 0
	* ad ogni iterazione su `L`, incrementa `k` se `L[i]>0`
	* ad ogni iterazione su `L`, decrementa `L[i]` se `L[i]>0`

L'algoritmo viene eseguito in: $O(k+n)$.


#### esercizio
Dimostrare usando la tecnica dell'albero di decisione che l'algoritmo di pesatura che esegue (nel caso peggiore) $\log_{3}n$ pesate per trovare la moneta falsa fra $n$ monete e' ottimo.

### Bucket Sort
Serve per ordinare `n` **record** con dei campi: nome, cognome, anno di nascita, ecc... Magari posso ordinare per data di nascita o per matricola. Il problema ha in input: `n` record in un array, ogni elemento e' un record con:
* un campo chiave (rispetto a cui ordinare)
* altri campi associati alla chiave (informazione satellite)

Similmente all'integer sort, mantengo una lista in output fatta degli array dati in input. La chiave dell'array mi dice l'indice in cui si troverà il campo nella lista in output. Le liste con stesso campo chiave saranno collegate: **Linked List**.

Ora scorro l'output da 0 fino ad $n$ e concateno le liste in un'unica struttura dati.

Il Bucket Sort, come per l'Integer Sort ha complessità: $O(n+k)$

```python
codice ...
```

Il Bucket Sort può essere usato per implementare il **Radix Sort**, ma bisogna introdurre la probabilità di stabilita.

>[!note] Stabilita di un algoritmo
> Un'algoritmo e' **stabile** se tutte le volte che ho due elementi con la **stessa chiave** in input, allora in output li trovo vicini e nello stesso ordine con cui li ho letti.


### Radix Sort
Prende in input un vettori di interi compresi tra $1 \text{ e } k$, e li ordina in output.  (Come l'integer sort). Rappresentiamo gli elementi in base $b$ ed eseguiamo dei Bucket Sort.

Per esempio con $b=10$ se voglio ordinare 2397, 4368, 5924. Inizio a ordinare dalla cifra meno significativa quindi alla prima passata avrò (paradossalmente) 5924 come numero più piccolo. Poi inizio a ordinare rispetto alla seconda cifra meno significativa, poi rispetto alla terza cifra meno significativa ecc... .

Ad ogni passata e' come se stessi ordinando dei record, dove il valore della **chiave** corrisponde alla cifra significativa corrente. Quindi posso applicare il **Bucket Sort**

Dimostrazione di **correttezza**: dopo $t$ passate, le cifre sono ordinate fino alla $t$esima cifra.
* se $x \text{ e } y$ hanno cifre diverse allora il Bucket Sort le ordina
* se $x \text{ e } y$ per induttività il Bucket Sort ha ordinato le $t-1$ cifre e quindi e' già ordinato. 

Ora bisogna scegliere una base. Più aumento la grandezza della base, meno cifre devo ordinare, ma i numeri sono più grandi.

#### Analisi Complessità
Tutti i numeri sono compresi tra $[1,k]$. Il numero più grande e' ovviamente $k$, le cifre di $k$ in base $b$ sono: $\log_{b}k$. Ho trovato un upper bound al **numero di passate** che l'algoritmo fa.
* Ogni passata di Bucket Sort e': $O(n+b)$
* Dato che il numero di passate e' $\log_{b}k$ allora: $O(\log_{b}k(n+b))$.

Ora, al crescere di $b$, $\log_{b}k$ diventa sempre più piccolo. Con $b=\Theta(n)$ si ha che $O(n \log_{n}k) = O\left( n \frac{\log k}{\log_{n}} \right)$. Se $k=O(n^c)$, con $c$ costante allora ho **tempo lineare**.

## Problema 4.10
Dato un vettore $X$ di $n$ interi in $[1,k]$, costruire in tempo $O(n+k)$ una struttura dati che sappia rispondere a delle domande in tempo $O(1)$ del tipo: "quanti interi in X cadono nell'intervallo $[a,b]$"? per ogni $a$ e $b$.
