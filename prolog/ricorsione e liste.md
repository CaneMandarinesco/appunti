
### esempio 1

```prolog
is_digesting(X,Y) :- just_ate(X,Y).
is_digesting(X,Y) :- 
	just_ate(X,Z),
	is_digesting(Z,Y).

just_ate(mosquito,blood(john)).
just_ate(frog,mosquito).
just_ate(stork,frog).
```

Prolog ha 2 strategie per determinare se `X` sta digerendo `Y`, ossia graficamente:
![[Pasted image 20251201114602.png]]

### esempio 2
```prolog
descend(X,Y) :- child(X,Y).
descend(X,Y) :- child(X,Z),
	descend(Z,Y).
```

Che equivale a scrivere:
```prolog
descend(X,Y) :- child(X,Y).
descend(X,Y) :- child(X,Z),
	child(Z,Y).
descend(X,Y) :- child(X,U),
	child(U,Z),
	child(Z,Y).
ecc...
```

Quando prolog vuole risolvere una ricorsione di questo tipo, per esempio: `descend(martha, laura)` (seguendo **lpn**).
* dato che il `kb` non contiene direttamente `child(martha,laura)` deve tentare con la ricorsione.
* deve verificare `child(martha,_633), descend(_633,laura)` e prova istanziando `_633` con `charlotte` e prova a risolvere `descend(charlotte,laura)`
* ecc...

### esempio 3
```prolog
numeral(0).

numeral(succ(X)) :- numeral(X)
```

ponendo a prolog la query: `numeral(X)` ottengo:
* `X=0;`
* `X=succ(0)`;
* `X=succ(succ(0))`;
* ecc...

Ossia, prolog sta contando, proviamo ad implementare l'addizione:
```prolog
add(0,Y,Y).
add(succ(X),Y,succ(Z)) :-
	add(X,Y,Z).
```

se chiediamo `add(succ(succ(succ(0))), succ(succ(0)), R)`:
* posso applicare solo la definizione ricorsiva
* `X` diventa `succ(succ(0))`, il primo funtore viene tolto per come e' definita `add`
* `Y` diventa `succ(succ(0))`
* `Z` diventa `succ(R)`

Quello che e' succede ad ogni livello della ricorsione e' che:
* `X` ad ogni step viene troncato un `succ`
* `Z` ad ogni step viene allungato di un `succ`

La ricorsione termina quando `X` diventa `0` ed il secondo e terzo termine sono identici, quando risalgo, a `Z` aggiungo tutti i `succ` srotolati da `X` a partire da `Z=Y`.

### ordinamento delle clausole
Specialmente quando si fanno  regole ricorsive, l'ordine in cui compaiono nel sorgente prolog deve essere corretto! prima il caso base e poi il caso ricorsivo. Altrimenti prolog guarda prima il caso ricorsivo e mai quello base.

# liste

> [!note] liste
> sequenza di di elementi
> * `[mia, vincent, jules, yolanda]`
> * `[mia, robber(honey_bunny), X, 2, mia]`
> * `[]`

Ogni lista non vuota consiste di:
* `head`: il primo elemento della lista
* `tail` il resto della lista togliendo la `head`.

L'operatore `|` permette di estrarre la testa da una lista:
* `[X|Y] = [[], dead(zed), [2, []], [], Z]` `X` e' legato al primo elemento, ossia `[]`.
* `[X,Y|W] = [[], dead(zed), ...]` lega primo e secondo elemento a `X` e `Y`.
* `[_,X,_,Y|_] = [...]` vuol dire che voglio legare a `X` e `Y` il 2o e 4 termine e **scartare il resto**. 

> [!note] `_`
> E' una variabile che prolog puo' usare per istanziare valori. Si utilizza quando il valore istanziato non ci interessa. Ovviamente, ogni occorrenza di `_` e' indipendente.

