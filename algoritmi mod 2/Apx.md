Un'algoritmo $\alpha\text{-apx}$ **approssima** il risultato rispetto all'ottimo di un fattore $\alpha$. Se $\alpha \geq 1$ allora ho un problema di minimizzazione, se $\alpha \leq 1$ invece ho un problema di massimizzazione.   

Di solito un'algoritmo $\alpha \text{-apx}$ esegue in tempo polinomiale ed e' usato per stimare il risultato di problemi difficili di cui non si conosce un'algoritmo polinomiale.

## Load Balance
> **Input**: $m$ intero, e $n$ job dove ognuno ha un $t_{i}:= \text{tempo per essere eseguito}$.
> **Output**: schedulare i job su $m$ macchine in modo da minimizzare il **makespan**.

Il **makespan** e' il carico massimo schedulato su una delle $m$ macchine, e minimizzarlo vuol dire ridistribuire al meglio il carico di lavoro sulle m macchine.

Il problema e' difficile da risolvere anche per $m=2$, infatti, $\text{PARTITIONING} \leq_{P} \text{LOAD BALANCE}$, dove partitioning e' un problema **NP-COMPLETO**.

Load Balance si approssima con il seguente algoritmo:

```pseudo
    \begin{algorithm}
    \caption{Load Balance}
    \begin{algorithmic}
	    \Procedure{LoadBalance}{$n,m,t_1,...,t_j$}
		    \For{$i=1 \text{to} m$} 
			    \State $L[i] \gets 0$
			    \State $S[i] \gets \emptyset$
			\EndFor

			\For{$j=1$ to $n$}
				\State $i \gets \text{argmin}_{k} \; L[k]$
				\State $S[i] \gets S[i] \cup \{j\}$
				\State $L[i] \gets L[i] + t_j$ 
			\EndFor
        \EndProcedure
    \end{algorithmic}
    \end{algorithm}
```

> [!note] $\text{2-apx}$
> L'algoritmo ha complessità $O(nm)$ ed e' un'algoritmo di $\text{2-apx}$

**Dimostrazione**. Per la dimostrazione serve sapere che:
* **Lemma 1**. $L^{*} \geq t_{j} \;\forall j = 1,...,n$. Dato che> cchine fa  del totale.

Vogliamo dimostrare che quando si schedula il job $j$ esimo alla macchina $i$, allora $(L[i]-t_{j})+ t_{j}\leq 2L^*\;$:
* $L[i]-t_{j} \leq L[k] \; \forall k=1,...,n$  dato che era la macchina più scarica, ed $L[k] \leq \frac{1}{m}\sum\limits_{k}L[k] \leq \frac{1}{m}\sum\limits_{k} t_{k}\leq L*$
* $t_{j}\leq L*$ viene da **Lemma 1**.

### LPT Rule
Conviene ordinare i job per durata in ordine non crescente: $t1 \geq ... \geq t_{m+1} \geq ...$.

>[!note] Soluzione Ottima
> Viene trovata sicuramente se la macchina di **bottleneck** ha un solo job.

Inoltre, se ci sono più di $m$ job, allora $L^{*} \geq 2 t_{m+1}$, dato che tutti i job pesano più di $t_{m+1}$, esiste una macchina che schedula $t_{m+1}$ e un'altro job prima.

Quindi con **LPT**: $L = (L[i] - t_{j})+ t_{j} \leq \frac{3}{2}L^*$, Ma questa non e' un'approssimazione stretta, infatti e' dimostrabile che Load Balance e' $\text{3/4-apx}$

## k-Center
> **Input**: insieme di $n$ siti (dati in input come Grafo delle distanze, o come piano cartesiano), e un intero $k$.
> **Output**: posizionare $k$ centri in modo da **minimizzare la distanza massima** tra un sito e il suo centro più vicino.

Una variante del problema vuole che si piazzi i centri dove ci sono i siti.

* $dist(x,y)$ **distanza** tra $x$ e $y$.
* $dist(s_{i}, C) = min_{c \in C} \; dist(s_i,c)$. ossia la distanza dal **centro più vicino**.
* $r(C) = max_{i}\;dist(s_i, C)$, ossia il **raggio**. Questo e' il valore da minimizzare!

**Proprietà della funzione distanza**: $dist(x,y) \leq dist(x,z)+dist(z,y)$.
**Algoritmo Greedy che risolve il problema**: per $k$ volte trova il sito $s_i$ che e' il più distante tra tutti i centri, e aggiungi un centro su quel sito.

### idea per la dimostrazione
Concetti chiave:
* supponi di conoscere $r$ il **raggio ottimo**, dove $r(C^{*}) \leq r$.
* ***Non conosciamo l'ottimo**: $C^*$, ma sappiamo che ad ogni sito appartiene ad un $c^{*}\in C^*$ e che questo $c^*$ e' a distanza al massimo $r$ da $s$, dove $r$ e' il raggio ottimo.
* Di nuovo, non conoscendo l'ottimo, posiziono un centro $c$ proprio sopra $s$ e vorremmo che questo copra tutti i siti coperti da $c^*$. Per farlo basta espandere il raggio di $c$ a $2r$.

> [!note] Greedy e' 2-apx
> L'algoritmo greedy ritorna k centri tali che $r(C) \leq 2r(C^*)$.

**Dimostrazione**. Boh
