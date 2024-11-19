* **tipo di dato**: specifica una collezione di oggetti e delle operazioni di interesse su tale collezione (inserimento, ricerca, cancellazione)
* **struttura dati**: organizzazione dei dati che permette di memorizzare la collezione e supportare le operazioni di un tipo di dato, usando meno risorse possibili. Specifica cosa fa ogni operazione definita dal tipo di dato.

> Per esempio il tempo di ricerca di un valore in un dizionario puo' dipendere da come e' strutturato il dizionario

### Dizionario
Il tipo `dizionario` usa coppie di dati: $(\text{elem, chiave})$ e permette di fare le seguenti operazioni:
* `insert(elem e, chiave k)`
* `delete(chiave k)`
* `search(chiave k)`

Ho una lista di coppie di dati che posso cercare, inserire o eliminare.

### Pila
Ho una sequenza di $n$ elementi e posso fare le seguenti operazioni:
* `isEmpty()`
* `push(elem e)`: aggiungi `e` come elemento della pila
* `pop()`: **togli** l'ultimo elemento e restituisci
* `top()`:  **senza toglierlo**, restituisce l'ultimo elemento

Una pila deve essere vista come una struttura in cui posso accedere e modificare solo l'elemento in cima.
### Coda
Una sequenza di $n$ elementi su cui posso fare:
* `isEmpty()`
* `enqueue(elem e)`: aggiungi `e` come ultimo elemento
* `dequeue()`: **togli** il primo elemento e restituiscilo
* `first()`: **senza togliere**, restituisce il primo elemento 

Generalmente quando mi metto in fila per il bar (per esempio), mi aggiungo in fila alla coda. Al contrario della Pila, gli elementi inseriti per ultimi vengono messi in fondo alla struttura. Nella Pila l'ultimo elemento inserito si trova in cima.
### Tecniche di rappresentazione
I dati possono essere rappresentati con:
* **indici**: quindi contenuti in un array e accessibili tramite indice.
	* Prop. **Forte**: gli indici delle celle di un array sono numeri consecutivi
	* Prop. **Debole**: non posso aggiungere nuove celle 
* **rappresentazioni collegate**: i dati sono contenuti in un record, e sono collegati tramite puntatori
	* ogni record e' un costituente base
	* i record sono numerati con il loro **indirizzo di memoria**
	* possono essere **creati e distrutti dinamicamente**
	* due generici record: A e B, sono collegati tramite **puntatori**
	* Prop. **Forte**: posso aggiungere o togliere record
	* Prop. **Debole**: gli indirizzi dei record non sono necessariamente consecutivi

In una rappresentazione indicizzata gli elementi possono essere collegati: 
* **Lista semplice**: In un solo verso
* **Lista doppiamente collegata**: Entrambi i versi
* **Lista circolare doppiamente collegata**: La catena di puntatori non si ferma mai!

### Realizzazione di un dizionario
si implementa grazie ad un **array non ordinato e sovradimensionato**
* `insert`: costa $O(1)$, basta inserire dopo l'ultimo elemento
* `search`: costa $O(n)$, devo scorrere tutti gli elementi
* `delete`: costa $O(n)$, devo scorrere tutti gli elementi e poi cancellare

Oppure con un **array ordinato**:
* `search`: costa $O(\log n)$, applico il **binary search**
* `insert`: costa $O(n)$ perché devo fare $n$ **trasferimenti** (tra gli elementi) per mantenere l'array ordinato una volta che con $O(\log n)$ confronti ho trovato dove metterlo. 
* `delete`: costa $O(n)$, stesso ragionamento dell'insert

Posso anche implementarlo con una **lista non ordinata**:
* `search`: $O(n)$
* `insert`: $O(1)$
* `delete`: $O(n)$

Invece con una **lista ordinata**: 
* `search`: $O(n)$ non posso usare la ricerca binaria
* `insert`: $O(n)$ devo mantenere la lista ordinata
* `delete`: $O(n)$


### Alberi
Posso anche optare per questo tipo di organizzazione, mantenendo quindi una gerarchia e dei gradi di parentela.
Posso usare un **array** (vettore di **padri**), ogni cella contiene le informazioni di un **nodo**:
* per un albero con $n$ nodi uso un array di dimensione almeno $n$.
* Una generica cella $i$ contiene una coppia $(\text{ info, parent})$ dove:
	* **info**: contenuto informativo del nodo
	* **parent**: indice del nodo padre

Gli elementi che rappresentano i nodi non devono essere necessariamente ordinati.

In questo modo posso individuare il padre in tempo costante, e per trovare i figli di un nodo mi basta scorrere la lista.

Oppure come visto precedentemente per l'heap sort, posso usare un **vettore posizionale** per rappresentare l'albero. Posso individuare il padre in tempo $O(1)$ e trovare il figlio di un nodo in $O(1)$ (pero' devo costruire l'array in modo da essere "ordinato").
### Rappresentazioni collegate degli alberi
Si implementano come visto prima con i **puntatori**.
* Con un **numero limitato** di figli, per esempio `sin` e `des` per un albero binario.
* Con una **lista di puntatori** per averne "illimitati"
* Rappresentazione del tipo: puntatore al primo figlio, e poi puntatore al fratello
![[Pasted image 20241105182339.png]]
![[Pasted image 20241105182314.png]]

## Algoritmi di vista in profondita
L'algoritmo DFS parte dalla radice: $r$ e procede visitando i nodi figlio, fino ad una foglia. Una volta toccata la foglia, si retrocede al padre e si accede all'altro figlio se non e' stato visitato e cosi via...

```python
algoritmo visitaDFS(nodo r)
	Pila S
	S.push(r)
	while (not S.isEmpty()) do 
		u = S.pop()
		if(u!=null)then
			visita il nodo u # fai delle operazioni
			S.push(figlio destro di u)
			S,push(figlio sinistro di u)
```

L'algoritmo impiega tempo $T(n)$: estraggo esattamente tutti gli $n$ nodi. Assumo che la visita del nodo $u$ impieghi tempo costante.

#### algoritmo ricorsivo per alberi binari

visita in **pre-ordine**:
```python
algoritmo visitaDFSRicorsiva(nodo r):
1.	if (r!=null) then
2.		visita il nodo r
3.		visistaDFSRicorsiva(figlio sinsitro di r)
4.		visistaDFSRicorsiva(figlio destro   di r)
```

visita  **simmetrica**: 
```python
algoritmo visitaDFSRicorsiva(nodo r):
1.	if (r!=null) then
3.		visistaDFSRicorsiva(figlio sinsitro di r)
2.		visita il nodo r
4.		visistaDFSRicorsiva(figlio destro   di r)
```

vista in **post-ordine**:
```python
algoritmo visitaDFSRicorsiva(nodo r):
1.	if (r!=null) then
3.		visistaDFSRicorsiva(figlio sinsitro di r)
4.		visistaDFSRicorsiva(figlio destro   di r)
2.		visita il nodo r
```

## Algoritmo di vista in ampiezza
```python
algoritmo visitaBFS(nodo r)
	Coda C
	c.enqueue(r)
	while(not C.isEmpty()) do
		u = C.dequeue()
		if(u!=null)
			visita il nodo u
			C.enqueue(figlio sintro di u)
			C.enqueue(figlio destro di ugi)
```