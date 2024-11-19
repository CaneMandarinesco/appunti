# Probabilità
>[!note] $P(A|B)$
>$$
>P(A|B) = \frac{P(A \cap B)}{P(B)} 
>$$

### Probabilità dell'unione
$$P(E \cup F) = P(E) + P(F) - P(E \cap F)$$
$$P(E \cup F \cup G) = P(E) + P(F) + P(G) -  P(E\cap F) - P(E \cap G) - P(F \cap G) + P(E \cap F \cap G)$$

### Probabilità di uno spazio uniforme discreto
$$P(A) = \frac{\#A}{\#\Omega} = \frac{\#A}{n}$$

>[!note] $P(A|B)$
>$$
>P(A|B) = \frac{P(A \cap B)}{P(B)} 
>$$

### Regola del prodotto
> Si applica a problemi dove il verificarsi di un'evento **influisce su un'altro**. Es: Pesco una pallina da un'urna (senza reinserirla ) e poi ne pesco un'altra

Per **2 famiglie di eventi**:
$$
P(A|B) = \frac{P(A \cap B)}{P(B) } \to P(A \cap B) =P(A|B) P(B)
$$
Per 3 famiglie di eventi:
$$
P(A \cap B \cap C) = P(A| B \cap C
) \cdot P(B|C) \cdot P(C)
$$

### Probabilità totali
> diversi eventi di tipo: $E_{1}, \dots, E_{k}$ di $\Omega$ influenzano un'evento di tipo $A$ non appartenente a $\Omega$. Esempio: il lancio dei dadi influenza la composizione delle palline in un'urna.
$$
P(A) = \sum_{i \in I} P(A|E_{i}) P(E_{i})
$$
### Formula di Bayes
> devo calcolare $P(A|B)$, ma posso calcolare $P(B|A)$

> [!note] formula di bayes
> $$
P(A|B) = \frac{P(B|A)P(A)}{P(B)}
$$

>[!note] bayes + probabilita totali
> $$
P(E_{n}|B) = \frac{P(B|E_{n})P(E_{n})}{\sum_{i \in I} P(B|E_{i})P(E_{i})}
$$

### Eventi indipendenti

$$P(A_{1} \cap A_{2}) = P(A_{1})P(A_{2})$$
per tutti gli eventi in $\mathcal A$. Disgiunti due a due
# Calcolo combinatorio
### Disposizioni Semplici
> Non voglio **ripetizioni** e mi interessa l'**ordine** in cui sono disposti gli elementi
$$
\#D_{n,k} = n(n-1)\dots(n-k+1) = \frac{n!}{(n-k)!}
$$

$$
P_{k} = \frac{\binom {n_{1}}{k} \binom {n_{2}}{n-k}}{\binom {n_{1} + n_{2}}{n}}
$$
### Combinazioni Semplici
> Non voglio **ripetizioni** e **non mi interessa l'ordine**

$$
\# C_{n,k} = \frac{\#D_{n,k}}{k!} = \frac{n!}{k!(n-k)!} = \binom {n} {k} = \binom {n}{n-k}
$$

### Probabilita con tipi di oggetti
> ho $n_{1}$ oggetti di tipo 1 e altri $n_{2}$ di tipo 2. Probabilità di pescare $k$ oggetti di tipo 1. Es: probabilità di fare terno (ossia pesco i numero 1, 2, 3).

$$
P_{k} = \frac{\binom {n_{1}}{k} \binom {n_{2}}{n-k}}{\binom {n_{1} + n_{2}}{n}}
$$
per $n_{1},\dots,n_{r}$ tipi, posso calcolare le combinazioni possibili:
$$
P_{k_{1},\dots,k_{r}} = \frac{\binom{n_{1}}{k_{1}} \dots \binom {n_{r}}{k_{r}}}{\binom {n_{1} + \dots n_{r}} {n}}
$$
# variabili aleatorie
>[!note] Probabilita' di un certo evento
>$$
>P(E) = \sum_{w \in E} m(w)
>$$


> [!note] Formula densità discreta della v.a. con distribuzione di Bernoulli
> $$ P_{X}(k) = \binom{n}{k} p^k (1-p)^{n-k} $$
> - generalmente $k \in \{ 0,1,\dots,n \}$, vale per tutte le densità discrete
> - eventi indipendenti
> - tutti hanno la stessa probabilità

> [!note] densità discreta della v.a con distribuzione ipergeometrica
>$$ P_{X}(k) = \frac{\binom{n_{1}}{k} \binom{n_{2}}{n-k}}{\binom{n_{1}+n_{2}}{n}} $$
> - oggetti tipo 1 e tipo 2
> - senza reinserimento

>[!note] densità discreta della distribuzione multinomiale
> $n$ prove indipendenti, ognuna ha $r_{k}$ risultati possibili
> $$ \frac{n!}{k_{1}! \cdot \dots \cdot k_{r}!} \cdot \left( \frac{1}{r} \right)^n $$

> [!warning] distribuzione uniforme discreta
> boh

> [!note] distribuzione di Poisson
> Parametro $\lambda > 0$
> $$ P_{X}(k) = \frac{\lambda^k}{k!}e^{-\lambda} $$

> [!note] approssimazione alla binomiale a partire da poisson
> $\lambda > 0$, con $n \geq\lambda \text{ allora } \frac{\lambda}{n}  \in [0,1]$. Posso prendere la densita di $X \sim BIN\left( n,p_{n} = \frac{\lambda}{n} \right)$. per $n\to \infty$ posso calcolare usando poisson.

>[!note] Distribuzione geometrica
> Successione di prove indipendenti, tutte con $p \in (0,1]$
> - $X = \#\text{ fallimenti prima del  1o successo } \to P(X\geq j = (1-p)^j)$ (geometrica)
> - $Y = \# \text{ prove per avere il 1o successo} \to P(X\geq j) = (1-p)^j-1$ (geometrica traslata)
> - $P(X = k+h | X \geq h) = P(X=k)$





