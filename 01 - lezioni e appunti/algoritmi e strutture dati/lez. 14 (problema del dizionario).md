Il tipo dizionario permette di fare le seguenti operazioni:
* `insert(elemento e, chiave k)`
* `delete(elemento e)`
* `search(chiave k)`
Posso implementare il dizionario con una struttura dati che esegue tutte le operazioni in tempo $O(\log n)$?

Due idee:
* definisco un albero binario in cui ogni operazione richiede O(altezza albero)
* l'albero deve avere altezza $O(\log n)$: Albero AVL.

### Albero Binario di Ricerca (BST)
Questo tipo di albero soddisfa le seguenti proprietà:
* ogni nodo ha `elem(v)` e `chiave(v)` presa da un dominio ordinato.
* le chiavi nel sotto-albero sinistro di `v` sono `<= chiave(v)`
* le chiavi nel sotto-albero destro di `v` sono `> chiave(v)`

Da queste proprietà segue che il minimo si trova nel nodo più a `sx`, il massimo invece nel nodo più a `dx`.

Se visito l'albero in ordine **simmetrico**, ho una visione **ordinata** degli elementi.

### `search(k)`
sfruttando le proprieta dell'ablero BST, posso semplicemente confrontare le chiavi dei nodi e scendere l'albero di conseguenza. Questa operazione esegue in $O(h)$, se l'albero e' sbilanciato avrò complessità $O(n)$. In generale non e' vero che $O(h) = O(\log n)$.

### `insert(e,k)`
devo cercare il nodo che diventerà genitore del nodo $(e,k)$. Applico il search, successivamente impostare il puntatore del nodo genitore e' un'operazione costante, quindi $O(h)$

### `max(n)`
Trovare il massimo in un sotto albero con radice `n` equivale a navigare l'albero fino al nodo **piu a destra**. 

### `pred(n)`
Il predecessore del nodo `n` e' il nodo con chiave massima `<= chiave(u)`. 
Se `n` ha un nodo sinstro, allora la chiave che cerco si trova nel suo sottoalbero
Se `n`ha solo un nodo destro, allora la chiave e' l'antenato piu basso del...

### Albero AVL
E' un albero BST **bilanciato in altezza**, se in valore assoluto $\beta(v)\leq 1$ per tutti i nodi. 
$\beta(v) = \text{ altezza sottoalbero sinistro di } v - \text{ altezza del sottoalbero destro di } v$.

#### Altezza di un albero AVL
Un'albero AVL di `n` nodi ha altezza $O(\log n)$. Vado a considerare l'albero di Fibonacci di altezza `h`, che e' un albero AVL che ha $n_{h}$ nodi. Da qui intuisco che se un albero di Fibonacci ha altezza $O(\log n)$ allora tutti gli alberi AVL hanno altezza $O(\log n)$. 

Un'albero di Fibonacci di altezza $h$ equivale ad un'albero AVL con il numero minimo di nodi possibili. Se a $T_{i}$ (albero di Fibonacci di altezza $i$) tolgo un nodo o **diventa sbilanciato** o **cambia la sua altezza**.

> un'albero che fissata un'altezza ha il numero minimo di nodi possibile e' quello piu sbilanciato possibile

> un'albero che fissato il numero di nodi ha l'altezza massima possibile e' quello piu sbilanciato possibile

Inoltre: **ogni nodo** ha $\beta(n) = 1$.

![[Pasted image 20241125152129.png]]

Graficamente si nota che c'e' un'algoritmo per generare l'albero $T_{i}$ a partire dagli alberi precedenti (L'albero $T_{3}$ ha come figli l'albero $T_{2}$ e $T_{1})$, e il numero di nodi cresce esponenzialmente rispetto all'altezza. Da qui segue che:

#### Lemma
Sia $n_{h}$ il numero di nodi di $T_{h}$, allora $n_{h} = F_{h+3}-1$. Si dimostra per induzione su $n_{h} = 1+ n_{h-1} + n_{h-2}$
![[Pasted image 20241125152558.png]]
**Ora posso concludere che** un albero AVL con $n$ nodi ha altezza $h=O(\log n)$. Da qui segue che $n_{h} = F_{h+3}-1 = \Theta(\phi^n) \to h = \Theta(\log n_{h}) = O(\log n) \to n_{h} \leq n$ .

### Implementare `insert` e `delete`
Se faccio un `insert`/`delete` sicuramente cambiano i fattori di bilanciamento dei nodi lungo il cammino radice-nodo di $\pm 1$. A fronte di molte operazioni di tipo `insert/delete` potrei sbilanciare troppo l'albero e la ricerca potrebbe costare arrivare a costare: $O(n)$ 

L'operazione di `search` potrebbe procedere come in BST, ma l'albero potrebbe essere sbilanciato: posso mantenerlo grazie a delle **rotazione**.

La **rotazione e' un'operazione che richiede tempo** $O(1)$ e viene effettuata su nodi sbilanciati. Vado a considerare  alberi con sbilanciamento $\pm 2$, allora c'e' un sotto-albero di $T$ che lo sbilancia e posso avere 4 casi:
* **SS**: $+2$, $T =$ sotto-albero sinistro del figlio sinistro di $v$.
* **DD**: $-2$, $T =$ sotto-albero destro del figlio destro di $v$
* **SD**: $+2$, $T=$ sotto-albero sinistro del figlio destro di $v$
* **DS**: $-2$, $T=$ sotto-albero destro del figlio sinistro di $v$

Le operazioni di rotazione sono simmetriche:
![[Pasted image 20241125171505.png]]

I casi si risolvono cosi:
* **SS**: allora ruoto verso **destra**. il fattore di bilanciamento passa da 2 a 0.
	* **caso 1**: l'altezza da $h+3$ passa a $h+2$
	* **caso 2**: altezza **invariata**
	* **nota**: *inserire una foglia porta solo al caso 1*
	* **nota**: *cancellare una foglia può provocare entrambi i casi*
* **SD**: ruoto verso **sinistra** con perno $z$ (figlio sinistro di $v$) e poi ruoto verso **destra** con perno $v$:
	* lo sbilanciamento passa da $h+3$ a $h+2$
	* puo' essere provocato da inserimenti o cancellazioni

Ora posso implementare le operazioni di inserimento e eliminazione.

#### `insert`
1. crea un nuovo nodo
2. inseriscilo usando gli algoritmi del BST
3. **ricalcola** i fattori di bilanciamento dei nodi nel cammino *dalla **radice** a **u***
4. esegui una rotazione opportuna su `v`.

#### `delete`
1. cancella il nodo come in BST
2. Ricalcola il fattore di bilanciamento del padre ed esegui la rotazione opportuna
3. Ripeti il punto 2 passo passo fino alla radice dell'AVL, potrebbero essere necessario $O(\log n)$

#### Considerazioni
Queste operazioni hanno tutte costo $O(n)$