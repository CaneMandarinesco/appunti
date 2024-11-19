## dipendente e indipendente
un insieme di vettori $v_{1},\dots,v_{k}$ si dice **linearmente dipendente** se esistono$a_{1},\dots,a_{k} \in \mathbb R$ *non tutti nulli* (escluso quindi il caso banale in cui sono tutti nulli) tali che:
$$ 
a_{1}v_{1} + \dots + a_{k}v_{k} = O
$$

al contrario si dicono **linearmente indipendenti** se non sono linearmente dipendenti, ovvero:  
$$
a_{1}v_{1} + \dots + a_{k}v_{k} = O \iff a_{1},\dots,a_{v} = O
$$

## casi speciali
### l'elemento nullo e' tra i vettori
se fra $v_{1},\dots,v_{k}$ c'e' $O$, allora $v_{1},\dots,v_{k}$ e' un sistema linearmente dipendente, perche' vale:
$$
0v_{1} + \dots + 0v_{i-1} + 1 \cdot v_{i} + 0v_{i+1} + \dots + 0 v_{k} = O
$$
> con $v_{i} = O$

e' una relazione di dipendenza lineare fra $v_{1},\dots,v_{k}$. 

### opposti
se $\exists i \neq j \text{ } (i < j): v_{i} = v_{j}$, con $\underline{a} = (\dots, \emptyset, 1, \emptyset, -1, \emptyset, \dots)$:
$$ 
\dots + \emptyset v_{i-1} + 1 v_{i} + \emptyset v_{i+1} + \dots -1v_{j} + \dots
$$

### sono proporzionali (n=2 vettori)
sia $v_{i} = c \cdot v_{j}$ allora:
$$
1 v_{i} + (-c) v_{j} = \emptyset
$$
dove $-c = a_{j}$, e quindi:
$$
a_{1} v_{1} + a_{2}+v_{2} = \emptyset_{v} \to v_{i} = (-a_{i}^{-1} \cdot a_{j}) v_{j}
$$
> dove ho semplicemente spostato i termini nell'equazione (+ ho usato al posto degli indici numerati, gli indici variabili $i \text{ e }j$)


### oss. (dal libro: oss 4.6 pag 76)
dire che i vettori $v_{1},\dots,v_{k} \in V$ sono linearmente **dipendenti** equivale a dire che e' possibile ricavare uno di loro come combinazione lineare degli altri.  
spiegato meglio, se abbiamo $a_{1}v_{1} + \dots + a_{k} v_{k} = O$ con per esempio, $a_{1} \neq 0$ allora:
$$
v_{1} = - \frac{a_{2}}{a_{1}} v_{2} - \dots - \frac{a_{k}}{a_{j}} v_{k}
$$

> tutto cio' ha senso se guardiamo la forma  delle equazioni lineari: $a_{1}A^1 + \dots + a_{n}A^n = O$, dove $A^k$ indica una "colonna" di elementi.

quini ha senso dire che: 
$$
v_{1} \in span(\dots,v_{0}, v_{2},\dots)
$$
> da notare che nell'elenco di vettori nello span, manca proprio $v_{1}$


> [!note] OSS
> in $V=V_{0}^3$ la prop per $n=2$ vale anche per $n=3$:
> $$
> v_{1},v_{2},v_{3} \text{ sono lin. dipendenti } \iff \exists i: v_{i} \text{ sta in } span(v_{i-1}, v_{i+1}) \iff \text{ sono complanari } 
> $$

# applicazioni (p. 105)
sia $v^o \in \mathbb R^ n$ una soluzione del sistema lineare $Ax = b$ di ordine n. Allora tutte le altre soluzioni sono della forma $v = v^o + w$ dove $w$ e' una soluzione del sistema omogeneo $Ax=O$, quindi tenendo presente che $L  \subseteq \mathbb R^n$ e' l'insieme delle nostre soluzioni in $Ax=b$ e che $W$ e' l'insieme delle soluzioni del sistema omogeneo associato $Ax = O$ si ha:
$$
L = v^o + W = \{ v^o + w | w \in W \}
$$
in particolare $v^o$ e' l'unica soluzione del sistema se e solo se le colonne di $A$ sono linearmente indipendenti.

### dim.
siano $v^o = (v_{1}^o ,\dots,v_{n}^o)$ e scegliamo $w=(w_{1},\dots,w_{n})$ del sistema omogeneo, quindi:
* $v_{1}^o A^1 + \dots + v_{n}^o A^n = b$
* $w_{1}A^1 + \dots w_{n} A^n = 0$

e sommando si ottiene:
$$
(v_{1}^o + w_{1})A^1 + \dots + (v_{n}^o + w_{n})A^n = b
$$
ovvero che $v^o + w$ e' un'altra soluzione del sistema $Ax = b$