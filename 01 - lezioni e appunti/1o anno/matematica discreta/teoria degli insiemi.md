 sintassi: 
$$
A = \{1,3,5\}
$$
* un insieme può essere **elemento di un altro insieme**
* non ci sono ripetizioni: $A = \{ 1,3,3,5 \}$ non e' un insieme

> [!note] DEF.
> A e' un **sotto insieme** di B se ogni elemento di A e' anche elemento di B. **possono anche essere uguali**
> $$ A \subseteq B$$

> [!note] DEF.
> invece con $A \subset B$, diciamo che ogni elemento di A e' in B, ma **escludiamo** il fatto che i due insiemi **siano uguali**

> [!note] DEF.
> $A \subsetneqq B$ significa:
> $$ A \subseteq B \text{ e } A \neq B$$

> [!note] OSS.
> $A=B$ significa che:
> $$A \subseteq B \text{ e } B \subseteq A$$

### prop 1.2.1: proprietà associativa
* $A \cup{} (B \cup{} C) = (A \cup B) \cup C$
* $A \cap (B \cap C) = (A \cap B) \cap C$

### prop 1.2.2: proprietà distributiva
* $A \cap (B \cup C) = (A \cap B)\cup(A \cap C)$
* $A \cup (B \cap C) = (A \cup B) \cap (A \cup C)$

>[!note] DIM. (per la 1a  formula)
> sia $x \in A \cap (B \cup C)$ allora $x \in A \text{ e } x \in (B \cup C)$.
> se $x \in B$ allora $x \in (A \cap B)$ ma dato che puo' essere anche elemento di C allora:
> $x \in (A \cap B) \cup (A \cap C)$
> viceversa abbiamo che:
>  sia $x \in (A\cap B) \cup (A\cap C)$ allora $x \in A \text{ e: } x \in B \text{ o } x \in C$ ovvero: $x \in A \cap(B \cup C)$
>


### insieme universo, paradosso di russel
sia: $\mathbb{A} = \{ A: A\not\in A \}$, possiamo affermare che: $\mathbb{A} \in \mathbb{A}$?
abbiamo che per definizione di A :
* se $\mathbb{A} \in \mathbb{A} \to  \mathbb{A} \not\in \mathbb{A}$
* se $\mathbb{A} \not\in \mathbb{A} \to \mathbb{A} \in \mathbb{A}$
questo paradosso si risolve se consideriamo $\mathbb{A}$ come sottoinsieme di $U$ (universo)

>[!note] DEF.
> il **complementare** di $A$ e' $A'$
> $$A' = \{ x \in U: x \not\in A \}$$


### tavole di verità
prendiamo per esempio la proprietà distributiva:
$$
A \cap (B\cup C) = (A \cap B) \cup (A \cap C)
$$
possiamo dimostrarla con la tavola di verità:

| A   | B   | C   | $B \cup C$ | $A \cap B$ | $A \cap C$ | $A \cap (B \cup C)$ | $(A \cap B) \cup (A \cap C)$ |
| --- | --- | --- | ---------- | ---------- | ---------- | ------------------- | ---------------------------- |
| 1   | 1   | 1   | 1          | 1          | 1          | 1                   | 1                            |
| 1   | 1   | 0   | 1          | 1          | 0          | 1                   | 1                            |
| 1   | 0   | 1   | 1          | 0          | 1          | 1                   | 1                            |
| 1   | 0   | 0   | 0          | 0          | 0          | 0                   | 0                            |
| 0   | 1   | 1   | 1          | 0          | 0          | 0                   | 0                            |
| 0   | 1   | 0   | 1          | 0          | 0          | 0                   | 0                            |
| 0   | 0   | 1   | 1          | 0          | 0          | 0                   | 0                            |
| 0   | 0   | 0   | 0          | 0          | 0          | 0                   | 0                            |
> similmente a come si fa in logica, scriviamo una tabella con tutte le combinazioni dei valori che possono assumere A, B e C. 

abbiamo che la colonna di $A \cap (B \cup C)$ e' uguale a quella di $(A \cap B) \cup (A \cap C)$ 
\mathbb{A}
### leggi di de Morgan
1. $(A\cap B)' = A' \cup B'$
2. $(A \cup B)' = A' \cap B'$

dimostrazione con tavola di verita:
#### (1)

| $A$ | $B$ | $A'$ | $B'$ | $A \cup B$ | $A \cap B$ | $(A \cup B)'$ | $A' \cap B'$ | $(A \cap B)'$ | $A' \cup B'$ |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 1 | 1 | 0 | 0 | 1 | 1 | 0 | 0 | 0 | 0 |
| 1 | 0 | 0 | 1 | 1 | 0 | 0 | 0 | 1 | 1 |
| 0 | 1 | 1 | 0 | 1 | 0 | 0 | 0 | 1 | 1 |
| 0 | 0 | 1 | 1 | 0 | 0 | 1 | 1 | 1 | 1 |
#### (2)
similmente

--- 

>[!note] DEF. CARDINALITA
> cardinalita = numero di elementi in un insieme
> - $|\emptyset| = 0$
> - $\{ 1,2,4 \} = 3$
> - $|\mathbb{N}| = \infty$

###  diagrammi di venn
ogni insieme e' rappresentato da punti racchiusi in una curva, dove ogni punto corrisponde ad un elemento. sono utili per aiutare **l'intuizione** in quanto i diagrammi non valgono come dimostrazioni. al contrario la tavola di verità **dimostra** ma non e' un metodo **intuitivo**.

>[!note] DEF.
>$A \backslash B$ e' la **differenza** tra due insiemi: $\{ x \in A: x \not\in B \}$

> [!note] DEF.
> $A \triangle B = (A \backslash B) \cup (B \backslash A) = (A \cup B) \backslash (B \cap  A)$

>[!note] DEF.
>**prodotto cartesiano**:
>$A \times B = \{  (a,b): a \in A, b \in B \}$ 
> $(a,b)$ e' una coppia ordinata ovvero: $= \{ a, \{ a,b \} \}$
> $(b,a)$ corrisponde a: $= \{ b, \{ a,b \} \}$
> quindi per esempio: $(1,2) \neq (2,1)$

>[!note] DEF.
> **insieme delle parti** (o insieme potenza):
> $$\mathcal{P}(A) = \{ B: B \subseteq A \}$$

### prop. 1.2.3
$$A \times (B  \cap C) = (A \times B) \cap (A \times C)$$
> [!note] DIM.
> sia $A \times (B \cap C)$, allora $x \in A \text{ e } y \in (B \cap C)$ ovvero $x \in A, y \in B \text{ e } y \in C$
> possiamo riscriverlo come: $(x,y) \in (A\times B) \text{ e } (x,y) \in (A \times C)$
> ovvero: $(A \times B ) \cap (A \times C)$
> 
> viceversa abbiamo che 
> sia $(x,y) \in ((A\times B) \cap (A\times C))$ allora se $x \in A \text{ e } y \in B \text{ e } x \in A \text{ e } y \in C$ ovvero: $x \in A \text{ e } y \in B \text{ e } y \in C$ 
> si puo' riscrivere come: $x \in A \text{ e } y \in (B \cap C)$ ovvero $(x,y) \in (A \times (B \cap C))$



