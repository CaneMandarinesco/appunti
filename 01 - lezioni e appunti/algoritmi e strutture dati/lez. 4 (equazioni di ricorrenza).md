### Esercizio
analizzare la complessità nel caso medio del primo algoritmo di pesatura. Rispetto alla distribuzione di probabilità sulle istanze, assumendo che la moneta falsa possa trovarsi in modo equiprobabile in una qualsiasi delle `n` posizioni. 

```python
Alg1(...):
	for i=2 to n do:
		if peso(x1) > peso(xi) then return x1
		if peso(x1) < peso(xi) then return xi
```

$$
\sum Pr(I) \cdot \#pesate(I) = \sum_{j=1}^n Pr(\text{moneta falsa e' in posizione j in I}) \cdot \# \text{ pesate}(I) 
$$
$$
= \left(  \frac{1}{n} \right) \left( 1 + \sum_{j=2}^n (j-1) \right) = \left( \frac{1}{n} \right)\left(  1+ \frac{n \cdot (n-1)}{2} \right) = \frac{1}{n} + \frac{n-1}{2}
$$
> Ricerca sequenziale delle monete

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
* $T_{\text{avg}} (n) = \frac{n-1}{2} = \Theta(n)$

### ricerca binaria, ricorsiva, su array ordinato
```python
AlgRicercabinariaRic(array L, elem x, int i, int j) -> int
	if (i>j) then return -1
	m = (i+j)/2
	if(L[m]=x) then return m
	if(L[m]>x) then return RicercaBinRic(...)
			   else return RicercaBinRic(...)
```


dato che e' una ricorsione lineare: $T(n) = T\left( \frac{n}{2} \right) + O(1)$, per capire la sua complessità, devo risolverla con:
* albero della ricorsione
* iterazione
* sostituzione
* teorema Master
* cambiamento di variabile

# analisi degli algoritmi
`
```python
AlgFibonacci2(int n) -> int
	if(n<=2) then return 1
	else return fibonacci2(n-1) + fibonacci2(n-2)
```

metodo per ottenere l'equazione di ricorrenza, deve capire:
* il **costo senza chiamate** ricorsive e': $O(1)$
* il **costo delle chiamate** ricorsive e': $T(n-1) + T(n-2)$

quindi l'equazione di ricorrenza e': 
$$
T(n) = T(n-1) + T(n-2) + O(1)
$$
### esempi
* $T(n) = T(n-1) + O(1)$
* $T(n) = T\left( \frac{n}{3} \right) + 2 T\left( \frac{n}{4} \right) + O(n \log n)$, dove il caso base impiega sempre $n \log n$
* $T(n) = T\left( \frac{n}{3} \right) + T\left( \frac{2n}{3} \right) + n$, il caso base e' costante

## equazioni di ricorrenza

> devo **srotolare** la ricorsione, ottenendo una sommatoria dipendente solo dalla dimensione `n`. 

---
se per esempio: $T(n) = c + T\left( \frac{n}{2} \right)$ con `c=costante`, allora $T\left( \frac{n}{2} \right) = c+T\left( \frac{n}{4} \right)$, ecc.... fino ad arrivare a $i\cdot c + T\left( \frac{n}{2^i} \right)$. quindi viene eseguito in tempo $\Theta(\log n)$

---
**per esempio**: $T(n) = T(n-1) + 1$
* $T(n-1) = T(n-2) +1$
* ...
* $T(n-i) + i$
che per ...

---
**per esempio**: $T(n) = 2 T(n-1) +1$
* ...
ovvero: $$ 
2^i T(n-i) + \sum_{j=0}^{i-1} 2^j
$$

per $i=n-1$:
$$
T(n) = 2^{n-1} T(1) + \sum_{j=0}^{n-2} = \Theta(2^n)
$$
dove, riprendendo il modello binario: $2^{n-2} = 2^{n-1} - 1$

---
**per esempio**: $T(n) = T(n-1) + T(n-2) + 1$:
* $= T(n-2) + T(n-3) + 1 + T(n-3) + T(n-4) + 1 + 1$
* ...
* lo schema da ricavare non e' immediato, si risolve con l'albero.

--- 
> analisi dell'albero della ricorsione

**esempio**: $T(n) = T(n-1) + 1$
Disegno l'albero delle chiamate ricorsive indicando la dimensione di ogni nodo:
* ogni nodo ha costo 1
* l'albero ha `n` nodi
* il costo dei `nodi` e': $T(n) = \Theta(n)$

**esempio**: $T(n) = T(n-1) + n$
Disegnando l'albero:
* ogni nodo costa al piu n
* ho n nodi
* la sovrastima e': $O(n^2)$, ho trovato l'upperbound: $T(n) \leq O(n^2)$
* se provo a trovare la stima esatta, tramite la sommatoria, ottengo $\Theta(n^2)$
* ottengo lo stesso risultato se (seguendo la slide), provo a sommare solo i primi $\frac{n}{2}$ nodi (ma posso affermarlo solo se ho calcolato l'upperbound!). Posso dire che $T(n) \geq \Omega(n^2)$

**esempio**: $T(n) = 2T(n-1) + 1$
Disegnando l'albero:
* ogni nodo ha 2 figli, fino ad arrivare al caso base
* ogni nodo costa 1
* albero binario completo: 
	* numero foglie: $2^n$
	* altezza: $n-1$
	* numero di nodi: $\sum_{i=0}^h 2^i = 2^{h+1}-1$
* dal numero di nodi mi ricavo che: $T(n) = 2^n -1 = \Theta(2^n)$

**esempio**: $T(n) = 2T(n-1) + n$
Disegnando l'albero:
* ogni nodo costa $n$
* posso dire che: $T(n) = O(n 2^n)$
* facendo un'analisi piu esatta: $\Theta(n^2)$

**esempio**: $T(n) = T(n-1) + T(n-2) + 1$
Disegnando l'albero, e riprendendo l'esempio di fibonacci della lezione 2:
* Il numero di nodi e': $\Theta(\phi^n)$
* posso anche **maggiorare** la ricorsione: $T(n) \leq R(n) = 2T(n-1) +n$ e ricavare un upper bound

**esempio**: $T(n) = T\left( \frac{n}{3} \right) + T\left( \frac{2n}{3} \right) + n$
Disegnando l'albero:
* $\frac{n}{27}$ e' il ramo che finisce prima, e questo ha altezza: $\log_{3}n$
* il ramo più lento ha altezza: $\log_{\frac{3}{2}}n$
* ogni singolo nodo costa al più: $n$
	* facendo un'analisi più attenta, la somma del costo di ogni nodo su una riga e' $n$
	* in verità' andando a calcolare i livelli più bassi la somma e' $\leq n$

Quindi: $T(n) = O(n \log n)$, ora possiamo stimare il costo esatto?

* $T(n) \geq n \log_{3} n$ che e' asintotico in $\Omega(n \log n)$
* quindi: $T(n) = \Theta(n \log n)$