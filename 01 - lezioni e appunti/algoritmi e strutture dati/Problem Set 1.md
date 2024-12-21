---
cssclasses:
  - wide-page
---
# Problema 3

Algoritmo:
1. ordino le sequenze per **valore**, da quelle con $v$ *piu grande al piu piccolo* e le metto nella lista: $X$. $O(n \log n)$
2. creo un **albero AVL**: $T$. Ogni nodo ha una **tupla che rappresenta spazi liberi** $(s,t)$ e il valore $t$ **massimo nel sotto albero**. I nodi sono ordinati nell'AVL in base al valore di $s$. $O(1)$
4. la variabile `score` e' inizializzata a 0
4. Prendo la prima sequenza in $X$ (quella con il valore piu grande), calcolo lo score e inserisco in $T$ gli spazi che non occupa (Inserisco al massimo 2 spazi)
5. Itero sulla lista $X$ dal secondo elemento fino all'ultimo. $s_{i}$ e' la sequenza presa all'iterazione $i$. $O(n)$
	1. Un contatore $v$ segna quante monete devo prendere da $s_{i}$
	2. Navigo l'albero e vedo con quali spazi vuoti $s_{i}$ si incastra, incremento $v$ di conseguenza. $O(??? \log(n))$
	3. Aggiungo le $v$ monete allo `score`
	4. elimino da $T$ gli spazi vuoti analizzati e inserisco gli spazi nuovi che si sono creati. $O(??? \log(n))$
6. il risultato si trova in `score`.
# Problema 2
Sei l'unico impiegato in un ufficio e devi servire `n` clienti. I clienti si prenotano con un'app apposita e sai quando verrano. Il cliente `i`-esimo arriva al tempo $t_{i}$, ma sei il cliente trova lo sportello occupato si mette in fila. Le pratiche vengono eseguite in $\Delta$ minuti. Dato che sei pigro puoi scegliere un intero $\Delta' \geq \Delta$ che rappresenta il tempo impiegato per smaltire ogni pratica, e dato che sei molto pigro vuoi cercare di massimizzare questo valore, senza correre il rischio di essere licenziato. Devi assicurarti di evadere le pratiche entro il tempo $M$.

Progettare un'algoritmo che calcola il massimo valore $\Delta'$ ammissibile in tempo $o(nM)$. Discutere la correttezza e la complessità' dell'algoritmo

Dati:
* $t_{i}$: tempo di arrivo di ogni cliente
* $\Delta' \geq \Delta$: tempo per smaltire ogni richiesta
* $M$: tempo entro cui devo finire
* $o(nM)$: tempo di esecuzione algoritmo

## Codice
```c
algoritmo find_delta(d, t, M, j):
    # d:  delta
    # t:  lista di tempi di arrivo
    # M:  tempo limite
    # j:  indice sinistro, considero l'array da j a n
    # ritorna: delta primo >= delta o -1 se i dati in input non sono validi

    Sia n il numero di elementi nell array.

    # -- CASO BASE --
    if n-j == 1:
        d1 = M - t[n-1] - 1
        if d1 < d:
            return -1
        return d1

    # -- CASO RICORSIVO --
    d1 = find_delta(d, t, M, j+1)

    # controlla se si crea una coda usando il tempo d1
    if t[j] + d1 > t[j+1]:
        # devo aggiornare d1 altrimenti sforo sicuramente il tempo M.
        d1 = (M - t[j] - 1) // (n-j)
        if d1 < d:
            return -1
    
    return d1
    

# metodo wrapper per find_delta
procedura alg_find_delta(d, t, M):
    return find_delta(d, t, M, 0)
```

## Tempo di esecuzione
E' stato utilizzato un metodo ricorsivo, la ricorsione e' del tipo: 
$$
T(k) = \begin{cases} 
O(1) \;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\; \text{ se } k = n-1 \\
T(k+1) + O(1) \;\;\; \text{ altrimenti }
\end{cases}
$$
per $0\leq k< n$, segue che l'algoritmo viene eseguito in tempo $\Theta(n)$, verifichiamo che sia un tempo accettabile secondo l'upper-bound $o(n \cdot M)$, usando la definizione di o-piccolo.

Per definizione $M\geq n \cdot\Delta$, quindi $M$ cresce almeno come $n$ (e quindi $M = \Omega(n)$).  

Ipotizzando che $M$ cresca esattamente come $n$ ($M \sim n$) allora, dalla definizione di o-piccolo:
$$
\lim_{ n \to \infty } \frac{n}{n \cdot M} = \frac{1}{M} \sim \frac{1}{n} = 0
$$
Nel caso in cui $M$ cresca ancora più velocemente di $n$, per esempio $n^2, n^3, \dots$ il limite tende sempre a 0, tenendo presente che $M$ non e' mai $< \Delta \cdot n$.
## Correttezza
Nel codice, i tempi di esecuzione si trovano in un array dove l'indice $0$ corrisponde al primo cliente, e l'indice $n-1$ corrisponde all'ultimo cliente.
Possiamo dimostrare la correttezza dell'algoritmo analizzando il **caso base** e il **caso induttivo**.
### Caso Base
Per $k=1$, ricado nel caso base, calcolo il valore massimo di $\Delta'$ per servire un solo cliente (l'ultimo), questo valore e' $\Delta' = M - t_{\text{n-1}} - 1$. 
### Caso induttivo
Ipotizzando che $\Delta'_{k+1}$ sia il valore massimo trovato al passo $k+1$, devo vedere se questo valore va bene anche al passo $k$ o se devo cambiarlo, ho due scenari possibili.
#### Scenario 1
$t_{k} + \Delta'_{k+1} > t_{k+1}$: questo vuol dire che se servo il cliente $k$ con tempo $\Delta'_{k+1}$, il cliente $k+1$ verrà messo in coda. Se servo i clienti da $k$ a $n$ con il tempo $\Delta'_{k+1}$ andrò sicuramente oltre il tempo limite $M$, il tempo in cui "inizio a servire" almeno uno dei clienti dopo $k$ e' cambiato, devo trovare un nuovo $\Delta'$. Similmente a come faccio per il caso base, il delta massimo possibile e': $\Delta'_{k} = (M-t_{k} -1) / (n-k)$, dove $\Delta'_{k} < \Delta'_{k+1}$.
#### Scenario 2
$t_{k} + \Delta'_{k+1} \leq t_{k+1}$: questo vuol dire che se servo il cliente $k$ con tempo $\Delta'_{k+1}$, mi avanza del tempo che potrei sfruttare per non fare nulla. Ma non posso incrementare $\Delta'_{k+1}$ dato che questo e' il delta massimo per servire i clienti da $k+1$ a $n$ entro il tempo limite $M$, quindi: $\Delta'_{k} = \Delta'_{k+1}$.
# Problema 1
Si consideri una sequenza di parentesi aperte e chiuse. La sequenza e' bilanciata se ogni parentesi aperta ha una corrispondente parentesi chiusa, e devono essere correttamente annidate.
* `(()(()()))`
* `()(()())`

Una sequenza e' `k-bilanciata` se, aggiungendo `k` parentesi aperte e `k` chiuse alla fine della sequenza si ottiene una sequenza bilanciata:
* `)()(` e' 1-bilanciata, poiché `()()()` e' bilanciata.
* `)()())((` e' 2-bilanciata ma non 1-bilanciata
* `()())` non e' `k`-bilanciata per nessuno `k>=0`

Progettare un'algoritmo che data una sequenza di `n` parentesi aperte e chiuse calcola il minimo valore di `k` per cui la sequenza e' `k`bilanciata, o restituisce $+\infty$ se non si può bilanciare.
0
Requisiti algoritmo:
* complessità temporale $O(n)$
* memoria ausiliare costante

## Codice 
```c
algoritmo bilancia(seq S):
	Sia n il numero di elementi in S

	h = 0 // numero di parentesi aperte da chiudere
	k = 0 // numero di parentesi chiuse da aprire
	for i = 0 to n do:
		if S[n] e una parentesi aperta then
			h++
		else // e' una parentesi chiusia
			if h > 0 then // ci sono parentesi aperte?
				h--       // ho chiuso una parentesi aperta  
			else k++      // ho una parentesi chiusa che deve essere aperta
	
	if h == k then        // ho lo stesso numero di parentesi aperte e chiuse?
		return h          // h o k e' il numero di parentesi da aggiungere
	else 
		return inf        // se h!=k non e' possibile k-bilanciare la sequenza
```

## Tempo di esecuzione
Il codice viene eseguito in tempo $\Theta(n)$, dato che itero su ogni carattere della sequenza. La memoria ausiliaria usata e' costante, non vado a richiedere memoria all'interno del ciclo `for`.
## Correttezza
### Lemma 
Una sequenza `S`, e' $k\text{-bilanciabile}$ se e' solo se ha lo stesso numero $h$ di parentesi aperte (che devono essere chiuse) e $k$ di parentesi chiuse (che devono essere aperte). 

#### Dimostrazione
**Per assurdo**: $k \neq h \to \text{ la sequenza S e' k-bilanciabile}$. Provo a bilanciare la $S$ con $k$. Indico con $k'$ e $h'$ le parentesi aperte e chiuse (da chiudere e aprire)  dopo che ho bilanciato la sequenza. Posso ricavare $k'$ e $h'$ cosi:
* $k' = k - k = 0$, quindi ho chiuso tutte le parentesi aperte!
* $h' = h - k > 0$, quindi ho almeno una parentesi chiusa che deve essere aperta, la dimostrazione termina qui.

Al contrario, **per assurdo**: $\text{la sequenza S e' k-bilanciabile} \to k \neq h$. Indico con $k'$ e $h'$ le parentesi aperte e chiuse (da chiudere e aprire) dopo che ho bilanciato la sequenza. Dato che $S$ e' $\text{k-bilanciabile}$ mi aspetto che $k'=h'=0$. Come prima mi posso ricavare $k'$ e $h'$:
* $k' = 0 = k-k$
* $h' = 0 = h-k$, ma $h\neq k$ quindi deve necessariamente essere che $k==h$

La dimostrazione funziona anche se provo a bilanciare con $h$ parentesi.


### Conclusione
La correttezza dell'algoritmo segue dal Lemma, che viene applicato nel codice.

