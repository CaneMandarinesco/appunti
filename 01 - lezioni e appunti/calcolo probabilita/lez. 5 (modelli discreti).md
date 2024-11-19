
> [!note] Variabili Aleatorie
> e' una funzione del tipo:
> $$X: \Omega \to \mathcal X$$
> - $\Omega = \text{insieme di possibili esiti del mio esperimento}$

tratteremo spesso il le funzioni dove $\mathcal X= \mathbb R$ oppure $\mathcal X = \mathbb R^n$.

> Esempio: se considero i lancio di un dado, allora:
> -  $\Omega=\{ 1,2,3,4,5,6 \}$
> - posso considerare $E=\{ 2,4,6 \}$, (evento: esce un numero pari)

### Esempio:
Si lanciano due dadi e consideriamo l'evento: la somma dei due numeri ottenuti e' 5. Posso dire che: $\Omega = \{ 1,\dots,6 \} \times \{ 1,\dots,6 \} = \{ w=(w_{1},w_{2}): w_{1} ,w_{2} \in \{ 1,\dots,6 \}\}$. Posso scegliere come $\mathcal A=\mathcal P(\Omega)$. E definire la funzione $X: \Omega \to \mathbb R$ come:
$$
X(w)= X(w_{1},w_{2}) = w_{1}+w_{2}
$$
E l'evento in questione e':
$$
\{  w = (w_{1},w_{2}) \in \Omega : X(w)=5 \}
$$
### Definizione
sia $(\Omega,\mathcal A, P)$ uno spazio di probabilità' e $X: \Omega \to \mathbb R$ una funzione. Allora la funzione $X$ e' una Variabile Aleatoria se vale:
$$
\forall t \in \mathbb R \;\;\;\;\; \{  w \in \Omega: X(w)\leq t \}
$$
Quindi:
$$
P(\{  w \in \Omega: X(w)\leq t \}) \text{ e' un numero ben definito}
$$
* $\{  w \in \Omega:  X(w) \in B \} \to  \{  X \in B\}$
* $P(\{ w\in \Omega: X(w) \in B \}) \to P(\{ X \in B \})$ 

esempio:
> lancio 4 dadi ($X$), allora la somma dei 4 dadi può essere rappresentata come: $X_{1} + X_{2}+X_{3}+X_{4}$.

### Distribuzione o legge di una v.a. reale
Corrispondenza tra gli insiemi della retta B per cui $\{ X \in B \} \in A$ e i valori $P(\{  X \in B \})$ relativi.


>[!note] notazione
>$$
>P(\{ w \}) = m(w)
>$$


> con $X$ che rappresenta il lancio di un dado. posso dire che: $m(w_{1}) + m(w_{2}) + \dots m(w_{6}) = 1$. Ogni $w_{i}$ ha associato un suo $m(w_{j})$ ossia, la **funzione di distribuzione**, e $m(w_{j})$ in un dado equo vale $\frac{1}{6}$, con le facce tutte diverse.
> Posso dire per esempio che: $P(X \leq 4) = \frac{2}{3}$

> Posso avere un dado composto cosi: $m(w_{1})=\frac{1}{2}, m(w_{2})=\frac{1}{3}, m(w_{3})=\frac{1}{6}$, ossia un dado a 6 facce dove alcune sono uguali.

> Oss. tutti gli $m(w_{i})$ hanno un denominatore comune $n$.

### Funzione di distribuzione di una v.a. reale
E' la funzione: $F_{X}: \mathbb R \to [0,1]$ definita come $F_{X}(t) = P(X\leq t)$. 

Nota che: $\sum_{x_{i} \in L_{X}} P(X=x_{i}) = 1$, o con la notazione del libro: $\sum_{w \in \Omega} m(w)=1$

>[!note] Probabilita' di un certo evento
>$$
>P(E) = \sum_{w \in E} m(w)
>$$

Per esempio, considera di lanciare una moneta due volte: $\Omega = \{\text{ HH, HT, TH, TT}\}$. Oppure potrei contare il numero di teste per ogni lancio di due monete: $\Omega = \{ 0,1,2 \}$. Ora $m(\text{HH}) = m(\dots) = m(TT) = \frac{1}{4}$.
Vado a considerare l'evento $P(E) = m(\text{HH}) + m(\text{HT}) + m(\text{TH})$. Posso calcolare $P(E)=3 \cdot \frac{1}{4} = \frac{3}{4}$
## Proprieta' funzione di distrubuzione
1. $F_{X}$ e' crescente.
2. $\lim_{ t \to -\infty } F(t) = 0$ e $\lim_{ t \to +\infty } F(t)=1$
3. $F_{X}$ e' continua a destra: $\lim_{ t \to t_{0}}F(t) = F(t_{0})$

### variabili aleatorie discrete
* $L_{X} = \{  x \in \mathbb R : \exists w \in \Omega \text{ t.c. } X(w)=x \}$, ossia le contro immagini di $X$.
$X$ e' discreta se $L_{X}$ e' discreto (finito o numerabile).

## distribuzioni notevoli
v.a. discrete -> espansione delle densita' discrete
v.a. continue -> funzioni di distribuzioni

### distribuzione bernoulliana
> distribuzione discreta notevole 

Si usa quando: $L_{X} = \{ 0,1 \}$, ovvero quando $B \in \mathcal A$ puo' verificarsi (1) o non verificarsi (0).

$$
\begin{cases}
P_{X}(1) = P(X=1) =P(B)  \\
P_{X}(0) = P(X=0) = P(B^C)
\end{cases} 
$$
dove per $P(X=1)$ si intende la probabilità che $X$ sia 1, la funzione di distribuzione si comporta cosi:

graficamente: 
* se $P(B) = 1$ ho un salto di 1 in corrispondenza di $1$ sull'asse $t$.
* se $P(B) = 0$ ho un salto di 1 in corrispondenza di 0 sull'asse $t$.\
* se $0< P(B) < 1$ l'ampiezza del salto e' distribuita tra i punti 0 e 1.

![[Pasted image 20241027201633.png]]
![[Pasted image 20241027201725.png]]

### schemi successo-fallimento su un numero finito di prove
e' una premessa per due casi:
1. distribuzione **binomiale** ($n$ prove, tutte con la stessa probabilità di **successo** $P$): si applica per esempio a consecutivi lanci di moneta
2. distribuzione **ipergeometrica** (n **estrazioni casuali**, per volta, senza **reinserimento**)
In entrambi i casi voglio studiare v.a. $X$ che conta il numero di successi.

In entrambi i casi considero: $\Omega = \{ 0,1 \} \times \dots \times \{ 0,1 \} = \{  w = (w_{1},\dots,w_{n}): w_{1},\dots,w_{m} \in \{ 0,1 \} \}$

Se voglio contare i successi:
$$
X(w) = w_{1}+\dots+w_{n} \to \text{ ne consegue che: } L_{X} = \{ 0,1,\dots,n \}
$$
Per ottenere i fallimenti:
$$
Y(w)= n-X(w)
$$