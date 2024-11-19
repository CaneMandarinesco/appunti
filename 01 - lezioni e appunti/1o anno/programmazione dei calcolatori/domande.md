1. le stringhe e le tuple sono strutture mutabili o immutabili?
2. come posso creare un programma che data una stringa mi sostituisca a questa  alcuni caratteri? (esempio tutti gli spazi con un `_`)
3. che complessità ha il programma della domanda precedente? suggerimento: $(n+1) \frac{n}{2}$


# risposte
1. si, infatti non e' possibile prendere un elemento di una stringa o una tupla e di modificarlo (`a[x] = ...`)
2. devo copiare man mano ogni singolo carattere  della stringa dentro un'altra stringa. e dato che anche questa stringa, essendo tale, e' immutabile, ogni volta che aggiungo un carattere con `y=y+c` sto creando una nuova stringa!
3. $O(n^2)$. 

