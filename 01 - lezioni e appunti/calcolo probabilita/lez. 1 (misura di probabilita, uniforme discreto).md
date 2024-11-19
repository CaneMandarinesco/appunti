>[!note] fenomeno aleatorio: $\Omega$
> - **discreto**: se $\Omega$ e' numerabile o finito
> - **continuo**: se $\Omega$ piu che numerabile
> oss: $\Omega$ e' un isieme

Esempi **discreti**:
* **finito**: $\{ 1,2,\dots,6 \}$
* **numerabile**: $\{0,1,2,\dots  \}$

Esempio **continuo**: $(0, \infty)$

Famiglia di **eventi** $A$, viene individuato come un sottoinsieme di $\Omega$.
*  "esce un numero pari": $\{2,4,6  \} \subset \{ 1,2,3,4,5,6 \} = \Omega$
* "il centralino riceve **al piu** due telefonate": $\{ 0,1,2 \} \subset \{ 0,1,2,\dots \}$
* la lampadina si fulmina dopo l'istante $t$: $(t, \infty) \subset (0, \infty)$

Famiglie di eventi con **buone proprieta'** ovvero $\sigma \text{-Algebra}$, se e solo se:
* $\Omega \in \mathcal A$
* $\forall A \in \mathcal A \to A^C \in \mathcal A$
* $\forall \{A_{n}\}_{n\geq 1} \in \mathcal A$ allora l'unione di tutti gli $A_{n} \text{ sta in } \mathcal A$
Questo mi garantisce che su qualsiasi operazione in $\mathcal A$ mi da un elemento di $\mathcal A$.

> [!note] misura di probabilita
> $P: \mathcal A \to [0, \infty)$ con $\mathcal A$ $\sigma \text{-algebra}$, allora $P$ e' una misura di probabilita se:
> * $P(\Omega) = 1$
> * $\forall \{ A_{n} \}_{n\geq 1} \subset \mathcal A \text{ tale che } [A_{m} \cap A_{n} = \emptyset]$ **disgiunti due a due**
> * $P\left( \bigcup\limits_{{i\geq 1}} A_{n} \right) = \sum _{n\geq 1} P(A_{n})$

* $P: A \to [0,\infty)$ assume valori solo in $[0,1]$. Questo verra' chiarito poi.
* $A_{m} \cap A_{n} = \emptyset$ e' una condizione piu forte di $\bigcap\limits _{n\geq 1} A_{n} = \emptyset$. se per esempio $B = A_{1} \cap A_{2}$ potrebbero avere qualche elemento in comune ma $B \cap A_{3}$ no, quindi $A_{1} \cap A_{2} \cap A_{3} = \emptyset$ anche se $A_{1} \cap A_{2} \neq \emptyset$.

### conseguenze della misura di probabilita
1. **Perche' $P(\emptyset) = 0$?** 
	1. $P(\emptyset) = 0$. se $A_{n} = \emptyset \forall n\geq 1$ si ha che $A_{m} \cap A_{n} = \emptyset$. 
	2. L'unione di tutti gli $A_{n}$ e' $\emptyset = \Omega$ 
	3. quindi $P(\Omega) = P(\emptyset) = \sum_{n\geq 1} P(\emptyset)$
	4. la *3.* e' vera solo se $P(\emptyset) = 0$
2. Se $h\geq 1$ e $B_{1},\dots,B_{h} \in \mathcal A$ e se sono tutti **disgiunti due a due**. allora la probabilità dell'unione dei vari $B_{k}$ e' uguale a: $\sum_{n=1}^hP(B_{n})$
3. $(E \cap F) \cup (E \cap F^C) = E$
	1. con $E = \Omega$ allora: $1 = P(F) + P(F^C)$
	2. con $F \in E$: $P(E) = P(F) + P(E \cap F^C)$
		1. quindi $P(A) \leq P(\Omega) = 1$
	3. $P(E \cup F) = P(E) + P(F) - P(E \cap F)$ ovvero **identita' di bonferroni**

identita di bonferroni per 3 insiemi: 
$$
P(E \cup F \cup G) = P(E) + P(F) + P(G) -  P(E\cap F) - P(E \cap G) - P(F \cap G) + P(E \cap F \cap G)
$$

## spazio di probabilità uniforme discreto
ho uno spazio di questo tipo se:
* $\Omega$ spazio finito
* $\mathcal A = \mathcal P(\Omega)$ 
* $\forall A \in \mathcal A$
ho che $P(A) = \frac{\#A}{\#\Omega} = \frac{\#A}{n}$ 

allora $\forall w \in \Omega P({w})$ **assume sempre lo stesso valore** $p$, quindi $1 = \sum_{w\in\Omega} P(\{ w \}) = n \cdot p$

* $P(A) = 0$ se e solo se $A \neq 0$
* si applica nei casi di estrazioni a caso tra $n$ oggetti
* non vale nei casi di insiemi infiniti numerabili

>[!note] $P(A|B)$
>$$
>P(A|B) = \frac{P(A \cap B)}{P(B)} 
>$$

