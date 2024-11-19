per calcolare la complessità temporale ci basta analizzare quante istruzioni vengono eseguite dal nostro codice. esempio:
```python
for i in range(n):
	print(i)
```
per esempio questo codice esegue `print(i)` per `n` volte.  

---
ora andiamo ad analizzare un altro codice per introdurre il caso peggiore, migliore e medio:
```python
a = [...] # lista di vari elementi
n = len(a)
for i in range(n):
	if a[i] == 0:
		return 0
	print(a[i])
```

ora il numero di volte per cui eseguiamo `print(a[i])` varia in base all'input (la lista 'a'), e possiamo distinguere per qualsiasi codice di questo tipo 3 casi:
* **peggiore**, in questo caso se `a[n] = 0`, ovvero l'ultimo elemento di a e' $0$
* **medio**, in questo caso se `a[n/2]=0`, ovvero l'elemento a meta' di `a` e' $0$
* **migliore**, in questo caso se `a[0]=0`, ovvero il primo elemento e' zero
di solito e' interessante studiare il caso peggiore, e il caso medio (quello più' verosimile)

## notazione asintotica
spesso diremo per esempio che la nostra funzione viene eseguita in tempo $\Theta(n)$ (letto: "theta n"), oppure $O(n)$ (letto: "o-grande di n") e simili.
* con $O(arg)$ (o-grande) si intende che il nostro codice non viene eseguito più' di `arg` volte, *quindi e' la notazione che usiamo per descrivere il caso peggiore.*
* con $\Theta(arg)$ (theta) si intende che il nostro codice non cresce ne' piu' lentamente, ne' piu' velocemente di `arg`, *quindi e' la notazione che descrive il caso medio*

riprendendo il codice di prima possiamo affermare che:
* al caso peggiore il codice viene eseguito in $O(n)$
* al caso medio il codice viene eseguito in $\Theta\left( \frac{n}{2} \right) = \Theta(n)$ (seguendo l'algebra degli o-grandi, oppure sapendo che le due formule sono asintoticamente equivalenti)

## esempi di notazione asintotica
in ordine di velocità:
* $O(1)$: costante. nota bene che: $O(1)=O(2)=O(3)=\dots$ ed e' vero se si guarda la definizione di `o-grande`
* $O(\log n)$: logaritmico
* $O(n)$: lineare
* $O(n \log n)$: log-lineare
* $O(n^k)$: polinomiale (quadratico, cubico, ecc...)
* $O(c^n)$: esponenziale

### costante
indica che la funzione impiega un tempo che e' indipendente dall'input. cio' non preclude il fatto che esistano dei cicli ma e' sicuro che quantomeno questi cicli non dipendono dall'input.

### logaritmica
almeno uno degli input aumenta come il logaritmo, come la **ricerca binaria** che e' logaritmica rispetto alla lunghezza della lista

### lineare
basic

### log-lineare
es. bubble sort