# Edit Distance
> **Input**: due string $X=(x_{1}, ..., x_{n}), Y =(y_{1},...,y_{n})$, $\delta := \text{costo per lasciare x/y unmatched}$, $\alpha_{xy} := \text{costo per allineare x con y}$.

> **Output**: $M := \text{matching, coppie di} \; (x_{i}, y_j)$ che minimizza $cost(M) = \sum\limits_{x_{i} \notin M} \delta + \sum\limits_{x_{i} \notin M} \delta + \sum\limits_{(x_{i}, y_{j}) \in M} \alpha_{x_{i}, y_{j}}$

Ossia, date due stringhe in input, e definite: $\delta$, *la penalità per non associare un carattere* $x$ ad $y$, e la penalità $\alpha_{xy}$, che definisce il costo per accoppiare $x,y$. Di solito si ha che $\alpha_{xy} = 0 \iff x = y$.

Si risolve con programmazione Dinamica!
$$
OPT[x,y] =  \begin{cases}
\delta x \;\; \text {if y = 0 (x unmatched)}\\
\delta y \;\; \text {if x = 0 (y unmatched )}\\
min \{ OPT[x-1,y-1] + \alpha_{xy} , OPT[x-1,y] + \delta, OPT[x,y-1] + \delta \}
\end{cases}
$$

Questa semplice formula permette di calcolare in tempo $O(nm)$ e spazio $O(nm)$ l'edit distance minimo tra due strighe.
L'ottimo consiste nel scegliere se:
* non matchare $x$ con $y$
* non matchare $y$ con $x$.
* matchare $x$ con $y$, e pagare la penalita se $x \neq y$.

Risolvere il problema vuol dire riempire la tabella $M[n,m]$ **riga per riga**, fino ad arrivare a $M[n,m]$

# Hirschbert
L'algoritmo base per edit distance occupa **troppa memoria**, si applicano quindi in maniera elegante le tecniche di **Programmazione Dinamica** e di **Divide Et Impera** insieme, in modo da ridurre la complessità ad $O(n+m)$.

Il problema può' essere riformulato come problema di shortest path sul grafo G per andare da $(0,0) \text{ a } (n,m)$, dove per ogni nodo $(x,y)$  e' definita $f(x,y):= \text{distanza da} (0,0) \text{ a } (x,y)$, si vuole dunque trovare $f(n,m)$. 

Ad ogni arco di $G$ , corrisponde una scelta che può fare l'algoritmo: 
![[Pasted image 20250629163602.png]]

La formulazione tramite Grafo e' potente perché:
* E' possibile invertire gli archi di $G$ per poter risolvere comunque il problema. (**fondamentale**, per es. la edit distance di $CIAO$ e' la stessa di $OAIC$).

>[!note] $g(x,y)$
>Per applicare la tecnica divide et impera bisogna definire $g(x,y):= \text{distanza da} (x,y) \text{ a } (n,m)$ (il contrario di $f$). $g$ può essere calcolato proprio invertendo gli archi di $G$!

> [!note] Tempo per calcolare una colonna 
> E' dunque possibile calcolare per ogni colonna $x$, tutti i $g(x,y)$ in tempo $O(nm)$, e spazio $O(n+m)$, dato che non mi serve tenere in memoria tutta tla abella, *ma solo la colonna corrente e quella precedente*.

>[!nota] Shortest Path su $G$
> Lo **shortest path** che passa per $(x,y)$ e' dato da $f(x,y) + g(x,y)$.

Applichiamo **Divide Et Impera** per risolvere il problema:
* trovare $q$ che minimizza $f\left(q,\frac n2\right)+ g(q, \frac n2)$, *il nodo trovato fa parte della soluzione*.

Lo spazio occupato e' garantito essere $O(n+m)$ dato che ci sono $\Theta(n)$ chiamate ricorsive e $\Theta(m)$ spazio per tenere in memoria un numero costante di colonne.

Se ti guardi la dimostrazione ti convinci anche che il tempo di esecuzione e' $O(nm)$ anche se sembra che vengano ricalcolate più e più volte le stesse colonne. Il motivo per cui il tempo di esecuzione e' basso e che:

> Una volta trovato $q$ per la colonna $n/2$ al prossimo caso ricorsivo dovrò ricalcolare solo meta della tabella! (perché so già che l'ottimo dovrà passare per $(q,n/2)$).

# Bellman-Ford-Moore Algorithm
> **Input**: grafo $G$ pesato in modo arbitrario.
> **Output**: path $s\to t$ di lunghezza minima.

Si risolve facilmente con DP, con $OPT[i,v] := \text {shortest path da s a t che usa al più i archi}$:
$$
OPT[i,v]= \begin{cases}
\infty \text { se i=0 }\\ \\
0 \text { se v = t } \\
min \{OPT[i-1, v], \min \limits _{u \text { t.c. } (v,u) \in E} {OPT[i-1, u] + l_{vu}} \}
\end{cases}
$$
Dove la scelta e' ottima e': non aggiungere archi ed usare lo stesso path con al più $i-1$ archi, oppure mi accorgo di aver trovato un'arco $(v,u)$ che mi collega ad $u$ che mi da uno **shortest path**.

La soluzione si trova in $OPT[n-1,s]$ (dato che un cammino semplice ha al più $n-1$ archi).

> [!note] Cicli Negativi
> Se $G$ contiene cicli negativi l'algoritmo se ne accorge, e vedremo dopo come.

>[!note] Cicli Positivi
> Se un path $s \to t$ contiene un ciclo $C$ con $w(C) \geq 0$, questo puo' essere eliminato per ottenere un path $s \to t$ piu' corto (grazie ar cazzo).

Ora, invece di scrivere il solito algoritmo che applica la formula di $\text{bellman-ford}$ implementiamo l'algoritmo usando due array di dimensione $n$ e non teniamo conto in memoria del numero di archi usati nel path:
* $d[v]$: distanza stimata da $v$ a $t$ (inizialmente infinito)
* $succ[v]$: nodo successore a $v$ nello shortest path verso $t$.

>[!note] $d[v] < w(v \to t)$  con $v \to t$ path che usa al massimo $i$ archi
>Per induzione:
> * $i=0$: allora $d[t]=0$.
> * due casi: se uso un'arco $(v,u)$ per stimare $v$ allora $d[v] = l_{vu}+ d[u]$, dato che $d[u] \leq w(P)$ (per induzione e' meglio dell'ottimo), il numero di archi aumenta di 1. Altrimenti il numero di archi e' invariato.

> [!note] Ogni ciclo diretto in $succ[v]$ e' un ciclo negativo
> Quando si forma un ciclo: $v_{1}, v_{2}, ..., v_{k}$ in $succ[v_{1}]$ si ha che 
> * $d[v_{1}] \geq d[v_{2}] + l_{v_{1} v_{2}}$
> * $d[v_{2}] \geq d[v_{3}] ...$
> * $d[v_{k}] > d[v_{1]}+ l_{v_{k}v_1}$ (per forza strettamente, un nodo ha la stima aggiornata).
> 
> allora algebricamente deve essere che: $l_{v_{1} v_{2}} + ... + l_{v_{k}v_{1}} < 0$.














