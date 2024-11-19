### distribuzione multinomiale
considero $n$ prove indipendenti,ognuna con $r$ **risultati possibili**, i risultati $p_{1},\dots,p_{r}$ sono gli stessi per ogni prova e la loro somma e' 1 ($p_{1}+\dots+p_{r}=1$).
Avro' quindi una sequenza di risultati: $(r_{1},\dots ,r_{1}, r_{2},\dots ,r_{2},\dots,r_{r},\dots,r_{r})$ dove $r_{1}$ e' ripetuto $k_{1}$ volte ecc...

La sequenza ha probabilità: $p_{1}^{k_{1}} \cdot \dots \cdot p_{r}^{k_{r}}$.

voglio calcolare quindi: $$p\begin{pmatrix}
k_{1} \text{ volte } r_{1} \\
\vdots \\
k_{r} \text{ volte } r_{r}
\end{pmatrix} = \# \{ \text{ sequenze con } k_{1} \text{ volte } r_{1},\dots, k_{r} \text{ volte } r_{r}\} \cdot p_{1}^{k_{1}} \cdot \dots \cdot p_{r}^{k_{r}}$$
$$
= \frac{n!}{k_{1}! \cdot \dots \cdot k_{r}!} \cdot \left(\frac{1}{r}\right)^{n}
$$
> oss. per $k=2$ si torna al caso binomiale

#### esercizio
Un'urna contiene 3 palline bianche, 3 rosse e 2 nere. Si estraggono 4 palline a caso, una alla volta, con **reinserimento**. Calcolare le probabilita per:
1. viene estratta la sequenza: (rossa, nera, nera, bianca)
2. vengono estratte esattamente 2 palline rosse e 1 nera in qualsiasi ordine
3. vengono estratte esattamente 2 palline rosse in un qualsiasi ordine

Svolgimento:
Abbiamo che: $p_{B} = \frac{3}{8}, p_{R} = \frac{3}{8}, p_{N}=\frac{2}{8}$.

1. devo calcolare: (ricordando che l'estrazione delle palline sono eventi indipendenti)$$P(R_{1} \cap N_{2} \cap N_{3} \cap B_{4})=P(R_{1}) \cdot \dots \cdot P(B_{4})=\left( \frac{3}{8} \right)^2 \cdot \left( \frac{2}{8} \right)^2$$
2. uso la distribuzione multinomiale: $$P(\text{2R e 1N in un qualsiasi ordine (su 4 pescate)}) = \frac{4!}{1!2!1!}\left( \frac{3}{8} \right)^1 \left( \frac{3}{8} \right)^2 \left( \frac{2}{8} \right)^1$$ $$ = \frac{648}{4096}$$
3. qui ricado nel caso della **distribuzione binomiale** (divido tra oggetti rossi e oggetti non rossi): $$P(\text{ 2R in un qualsiasi ordine }) = \binom {4}{2} \left( \frac{3}{8} \right)^2 \left( 1-\frac{3}{8} \right)^{4-2}=\frac{1350}{4096}$$

## distribuzione di uniforme discreta
si applica per un insieme finito in cui: 
$$
P(X \in B) = \frac{\# (B \cap E)}{\# E} = \frac{\#(B \cap E)}{n}
$$

### distribuzione di Poisson
Una v.a. $X$ con parametro $\lambda > 0$, e' distribuzione di Poisson se:
$$
P_{X}(k) = \frac{\lambda^k}{k!} e^{-\lambda} \;\;\; \forall k \in \{ 0,1,2,3,\dots \}
$$
Oss: $\sum_{k=0}^\infty p_{X}(k)=1$ in effetti:
$$
\sum_{k=1}^\infty p_{X}(k) = \sum_{k=1}^\infty \frac{\lambda^k}{k!} e^{-\lambda} = e^{-\lambda} \cdot e ^ \lambda = 1
$$

#### esercizio
sia $X \sim \text{Poisson}(\lambda=4)$:
1. Calcolare $P(X>2)$
2. Calcolare $P(X=k | X\leq 2) \text{ per ogni } k\geq 0 \text{ intero}$

Svolgimento:
1. E' meglio calcolare la probabilità dell'evento complementare: $$ P(X>2) = 1 - P(X\leq 2) = 1 - \sum_{k=0}^2p_{X}(k) = 1-(\dots) = 1-13e^{-4}$$
2. $$P(X=k | X \leq 2) = \frac{P(\{ X=k \} \cap \{ X \leq 2 \})}{P(X\leq 2)} = \begin{cases}
\frac{P(X=k)}{P(X\leq 2)} \;\; k \in \{ 0,1,2 \} \\
0  \text{ per } \;\;\;\; k\geq 3
\end{cases}$$
#### approssimazione della binomiale con la poison
Sia $\lambda > 0$, con $n\geq \lambda$ intero, in questo modo $\frac{\lambda}{n}\in [0,1]$. Posso prendere la densita' di $X \sim BIN\left( n,p_{n}=\frac{\lambda}{n} \right)$. Per $n\to \infty$ ho che $p_{X}(k) = \frac{\lambda^k}{k!} e^{-\lambda}$.

Ossia per $X = X_{n} \sim BIN\left( n,p_{n}=\frac{\lambda}{n} \right)$ e $Z \sim POISSON(\lambda)$ allora:
$$
\lim_{ n \to \infty } p_{X_{n}}(k) = p_{Z}(k)
$$