## Bucket Sort
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


## Radix Sort
Prende in input un vettori di interi compresi tra $1 \text{ e } k$, e li ordina in output.  (Come l'integer sort). Rappresentiamo gli elementi in base $b$ ed eseguiamo dei Bucket Sort.

Per esempio con $b=10$ se voglio ordinare 2397, 4368, 5924. Inizio a ordinare dalla cifra meno significativa quindi alla prima passata avrò 5924 come numero più piccolo. Poi inizio a ordinare rispetto alla seconda cifra meno significativa, poi rispetto alla terza cifra meno significativa ecc... .

Ad ogni passata e' come se stessi ordinando dei record, dove il valore della **chiave** corrisponde alla cifra significativa corrente. Quindi posso applicare il **Bucket Sort**

Dimostrazione di **correttezza**: dopo $t$ passate, le cifre sono ordinate fino alla $t$esima cifra.
* se $x \text{ e } y$ hanno cifre diverse allora il Bucket Sort le ordina
* se $x \text{ e } y$ per induttività il Bucket Sort ha ordinato le $t-1$ cifre e quindi e' già ordinato. 

Ora bisogna scegliere una base. Più aumento la grandezza della base, meno cifre devo ordinare, ma i numeri sono più grandi.

### Analisi Complessità
Tutti i numeri sono compresi tra $[1,k]$. Il numero più grande e' ovviamente $k$, le cifre di $k$ in base $b$ sono: $\log_{b}k$. Ho trovato un upper bound al **numero di passate** che l'algoritmo fa.
* Ogni passata di Bucket Sort e': $O(n+b)$
* Dato che il numero di passate e' $\log_{b}k$ allora: $O(\log_{b}k(n+b))$.

Ora, al crescere di $b$, $\log_{b}k$ diventa sempre più piccolo. Con $b=\Theta(n)$ si ha che $O(n \log_{n}k) = O\left( n \frac{\log k}{\log_{n}} \right)$. Se $k=O(n^c)$, con $c$ costante allora ho **tempo lineare**.

## Problema 4.10
Dato un vettore $X$ di $n$ interi in $[1,k]$, costruire in tempo $O(n+k)$ una struttura dati che sappia rispondere a delle domande in tempo $O(1)$ del tipo: "quanti interi in X cadono nell'intervallo $[a,b]$"? per ogni $a$ e $b$.
