> [!note] termine
> Un termine puo' essere una **costante**, una **variabile** o un **termine complesso** (funtore + termini)

> [!note] match
> due termini fanno match **se sono uguali** o se le *loro variabili possono essere istanziate nello stesso modo*.

**Regole per il match**:
* `term1` e `term2` costanti fanno **match** se sono lo stesso atomo
* se `term1` e `term2` costante, fanno **match** se `term1` puo' essere istanziato a `term2`.
* se `term1` e `term2` sono complessi fanno match se: hanno lo stesso funtore e numero di parametri, gli argomenti fanno match uno ad uno e se le istanze delle variabili sono compatibili.
* 2 termini fanno match se e solo se posso determinarlo da una di queste 3 clausole.

### occur check
Prolog non applica l'algoritmo di unificazione (descritto in AIMA, corso di Intelligenza Artificiale).

**Prendendo in considerazione la query**: `father(X) = X.` se usiamo un'algoritmo di unificazione applicando una regola sappiamo che i due termini non sono unificabili.

> **Nota**: `father(X) = X` non sono unificabili per il fatto che hanno numero di simboli differenti.

**In Prolog**: otteniamo una risposta del tipo `Not enough memory to complete the query!`. Questo perche' prolog prova ad istanziare `X`, nella speranza di arrivare alla soluzione.

> **Nota**: alcune implementazioni di prolog identificano la presenza di cicli di questo tipo nella ricerca della soluzione .

La differenza tra l'algoritmo di unificazione standard e l'algoritmo di prolog sta nell'**assunzione** che la query fornita dall'utente, si assume in prolog abbia un goal raggiungibile. (*Anche perche' nessun utente farebbe a prolog query impossibili da risolvere)*.

Sotto questa assunzione, prolog non fa `occurs check` (non cerca di applicare la regola descritta nell'algoritmo di unificazione). In questo modo l'algoritmo di prolog e' molto veloce!

### prof search
Quando prolog deve verificare una regola del tipo `k(X) :- f(X),g(X),h(X)`, (seguendo quello che farebbe l'algoritmo di unificazione), invece di usare `X` usa una nuova variabile da condividere. 

Dunque la formula diventa per esempio: `k(_G348) :- f(_G348)...`.

Successivamente, prolog prova ad assegnare valori a `_G348` e vede se riesce a **soddisfare tutte le formule dell'insieme** che costituisce il `goal`.

Quando prolog fallisce, fa **backtracking**, ritorna all'ultima assegnazione da ritentare per arrivare alla soluzione.





