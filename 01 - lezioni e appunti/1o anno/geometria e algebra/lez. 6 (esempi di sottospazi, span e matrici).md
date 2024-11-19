## lemma: sottospazio vettoriale
sia $W \subseteq \mathbb R^n$ l'insieme delle soluzioni di un sistema lineare omogeneo in $n$ incognite $Ax = O$. allora $W$ e' un **sottospazio** vettoriale di $\mathbb R^n$.  
dato che $W \leq V$, in $W$ valgono le stesse proprietà di $V$.   quindi come dice il prof. **spero** che per esempio:
* $\exists \emptyset_{v} \in V: v+\emptyset_{v} = v$
* $-w = -1 \cdot w$ 
* $w_{1}+w_{2} \in W$
* $t \cdot w \in W$
## esempi di sottospazi

>[!note] notazione
>$\forall \text{ insieme } E, \forall \text{ spazio vettoriale } V$:
>$$
>V^E = \{ f: E\to V \}
>$$
>quindi la notazione $V^E$ indica una funzione che ad ogni elemento di $E$ associa un elemento di $V$

### funzione continua
$$
C(a,b) = \{f \in \mathbb R ^{[a,b]}: f \text { e' continua}\}
$$
> $C(a,b)$ = funzioni continue in $[a,b]$

e' un sottospazio di $\mathbb R^{[a,b]}$, significa che di tutte le funzioni prese in un intervallo $[a,b]$ andiamo a considerare solo quelle continue in $[a,b]$.  
questo insieme di funzioni e' un sottospazio vettoriale!
* posso sommare due funzioni continue in $[a,b]$ e ottenere un'altra funzione continua
* posso moltiplicare per uno scalare una funzione continua e ottenere una funzione continua
### funzioni derivabili
$$
Der(a,b) = \{ f \in \mathbb R^{[a,b]}: f \text{ e' derivabile} \}
$$
seguendo la stessa logica di prima, anche questo e' un sottospazio, ovvero tutte le funzioni derivabili in $[a,b]$.  
* posso sommare due funzioni derivabili in $[a,b]$ e ottenere un'altra funzione derivabile
* posso moltiplicare per uno scalare una funzione derivabile e ottenere un'altra funzione derivabile

### funzioni convergenti
$$
Conv(\mathbb R^{\mathbb N}) = \{  \{ a_{n} \}_{n\in\mathbb N} \in \mathbb R^{\mathbb N}: \text{ il limite converge} \}
$$

considero le successioni in $\mathbb R ^ {\mathbb N}$, tali che il loro limite converga.  
* la somma di due successioni convergenti e' una successione convergente.
* il prodotto di uno scalare per una succ. convergente e' una successione convergente.

### funzioni infinitesime
$$
Inf(\mathbb R^{\mathbb N}) = \{  \{ a_{n} \}_{n\in\mathbb N} \in \mathbb R^{\mathbb N}: \text{ il limite tende a } \emptyset$ \}
$$
e' un sottospazio di $Conv(\mathbb R ^ {\mathbb N})$.   

### polinomi
$$
\mathbb K[x] = \{ \text{ polinomi in } x \text{ e coefficienti in } \mathbb K \}
$$
quindi:
$$
P(x) = c_{0}+c_{1}x+\dots+c_{n} x^n \;\;  \text{ dove: } c_{0},\dots ,c_{n} \in \mathbb K \text{ e } n \in \mathbb N
$$
posso scrivere un polinomio cosi:
$$
P(x) = \sum_{i=0}^n c_{i} \cdot x^i
$$
e eseguendo le operazioni di somma e prodotto scalare scopro che:
* il risultato di una somma e' sempre un polinomio in $x$ (il grado e' sempre $\leq$ del grado max.)
* il risultato di un prodotto scalare e' sempre un polinomio in $x$.

>[!warning]
> ci sono due esempi sui polinomi ma io ne ho solo uno
## span
$$
span(v_{1},\dots,v_{n}) \text{ e' un sottospazio generato da } (v_{1},\dots,v_{n}) \text{ vettori}
$$
lo span (o sottospazio) contiene tutte le possibili combinazioni lineari (soluzioni) dati dai vettori $(v_{1},\dots,v_{n})$.  quindi in simboli:
$$
Span(v_{1},\dots,v_{k}) = \{ a_{1}v_{1} + \dots + a_{k} v_{k} | a_{1},\dots,a_{k} \in \mathbb R \}
$$
bisogna di mostrare che $span$ e' un sottospazio di $V$.  

### dim.
per la somma:
$$
(a_{1}v_{1} + \dots + a_{k} v_{k}) + (b_{1}v_{1} + \dots + b_{k}v_{k})
$$
raccogliendo i fattori abbiamo che:
$$
(a_{1}+b_{1}) v_{1} + \dots +(a_{k}+ b_{k})v_{k}
$$
che sta in $span$ essendo una combinazione lineare dei vettori $(v_{1},\dots,v_{n})$.  

per il prodotto scalare:
$$
\gamma(a_{1} v_{1} + \dots + a_{k} v_{k})
$$
raccogliendo usando la proprietà distributiva:
$$
(\gamma a_{1}) v_{1} + \dots + (\gamma a_{n}) v_{n}
$$
che sta in $span$ dato che anche questa e' una combinazione lineare.

### esempio
$V = \mathbb R^3$

$$
v_{1} = (3,1,0) \text{ e } v_{2} = (-1,-1,0)
$$
allora ho che:
$$
span(v_{1},v_{2}) = \{ a_{1}(3,1,0) + a_{2} (-1,-1,0) | a_{1},a_{2} \in \mathbb R \} = \{ (3a_{1} -a_{2}, a_{1}-a_{2},0) \}
$$
avendo $z=0$, geometricamente lo span corrisponde al piano che ha $z=0$

## sistemi lineari
un sistema lineare ha una forma simile:
$$
a_{1} x_{1} + a_{2}x^2 + \dots a_{n} x^n = b_{1}
$$
ora pero' voglio poter cambiare gli indici di $a$ e $b$ in base ad un numero $i$ e di costruire un insieme dei suoi risultati:
$$
\{ a_{i,1}x_{1} + a_{i,2}x_{2} + \dots + a_{i,n}x_{n} = b_{i}; \forall i=1,\dots,n; \;\; m,n \in \mathbb N^+\}
$$
posso dire che ho due stringhe:
$$
\underline{b} = (b_{1}, \dots, b_{n}); \underline{x} \in (x_{1},\dots,x_{n})
$$
dove: $\underline{b} \in \mathbb K^n \text{ e }\underline{x} \in \mathbb K^n$.   

possiamo scrivere il sistema lineare come una matrice.

### matrici
> [!warning] lol
> non mi va di scrivere per bene le matrici in latex.  tanto so semplici
> 

### sistema lineare omogeno
ill sistema lineare omogeno e' quel sistema $Bx=0$ che e' associato ad un sistema lineare qualunque $Bx=b$
se un sistema lineare e' **omogeneo** allora:
$$
\mathbb L = \{\underline{x} \in \mathbb K^n | \underline{x} \text{ e' soluzione del sistema lineare}\}
$$
> $\mathbb L$ e' sottospazio vettoriale di $\mathbb K^n$.  

### dim.
la somma si verifica col seguente ragionamento: dato che le soluzioni valgono $0$ la somma di tutte le soluzioni e' $0$, quindi sommando due soluzioni $\underline{x_{1}} \text{ e } \underline{x_{2}}$ il risultato e' sempre $0$.   

invece per il prodotto scalare: dato che la somma e' 0, in generale $k \cdot 0$ fa $0$ per qualsiasi $k$.

un sistema lineare non omogeneo non e' un sottospazio vettoriale: non contiene il vettore nullo.

### sistema di generatori 
si dice che $v_{1},\dots,v_{n}$ e' un sistema di generatori per $V$ se $span(v_{1},\dots,v_{n}) = V$.

inoltre:
$$
\sigma_{v_{1},\dots,v_{n}} \text{ e' suriettiva } \iff v_{1},\dots ,v_{n} \text{ e' un sistema di generatori} 
$$
