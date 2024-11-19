---
cssclasses:
  - wide-page
---
# Introduzione 
Consideriamo un urna con 6 palline, ognuna identica all'altra, e voglio estrarne una da un'urna. Per associare il problema ad un modello matematico posso associare ad ogni pallina un numero, ottengo cosi un'insieme che per convenzione chiamo $\Omega = \{ 1,2,3,4,5,6 \}$. Di conseguenza, ogni evento che andremo a considerare e' un sottoinsieme di $\Omega$.
Es: $A=\{ 1,2 \}$, rappresenta l'evento: "estraggo o 1 o 2". $B= \{ 2,4,6 \}$ rappresenta l'evento: "esce un numero pari".

Alcune considerazioni sulle operazioni insiemistiche quando si parla di eventi:
* $A \cup B$, rappresenta l'evento: si verifica almeno uno dei due
* $A \cap B$, rappresenta l'evento: si verifica entrambi
* $A^c$, rappresenta l'evento: non si verifica $A$.

> [!multi-column]
>>[!note] fenomeno aleatorio: $\Omega$
>> - **discreto**: se $\Omega$ e' numerabile o finito
>> - **continuo**: se $\Omega$ piu che numerabile
>> 
>> Esempi **discreti**:
>> * **finito**: $\{ 1,2,\dots,6 \}$, come l'esempio di prima delle palline.
>> * **numerabile**: $\{0,1,2,\dots  \}$ 
>> 
>>Esempio **continuo**: 
>> * $(0, \infty)$, per esempio mi chiedo la probabilità che una pallina si fulmini all'istante $t$
>
>> [!note]  $\sigma \text{-Algebra}$
>> Una famiglia di eventi $\mathcal A$ contente sottoinsiemi di $\Omega$ e' una $\sigma \text{-Algebra}$ se
>> * $\Omega \in \mathcal A$
>>* $\forall A \in \mathcal A \to A^C \in \mathcal A$
>>* $\forall \{A_{n}\}_{n\geq 1} \in \mathcal A$ allora **l'unione/intersezione** di tutti gli $A_{n} \text{ sta in } \mathcal A$

La **probabilità** $P$ e' una funzione che $P: A\to \mathbb R$ tale che:
* $P(\Omega) = 1$
* se gli elementi di $\mathcal A$ sono disgiunti due a due, allora: $P(\text{unione di tutti gli } A_{n}) = \sum_{n=1}^\infty P(A_{n})$

### spazi di probabilità uniformi
Si ha quando $P(\{ w \}) = p$ per ogni $w \in \Omega$. Allora di conseguenza e' vero che: $1=P(\Omega)= \sum_{w\in \Omega}P(w) = p \cdot \# \Omega$. Ossia, dato che tutti gli eventi hanno la stessa probabilità di verificarsi, allora la somma di tutti questi deve essere uguale ad 1.

Alcune proprieta generali degli spazi di probabilita (anche non uniformi):
* $P(B) = P(B \cap A) + P(B \cap A^C)$ dato che $(B \cap A) \cup(B \cap A^C) = B \cap(A \cup A^c) = B \cap \Omega = B$
* $P(A) = 1 -P(A^C)$
* $P(A \cup B) = P(A) + P(B \cap A^C)$

Dall'ultima (per vari passaggi logici) seguono le formule di De Morgan (viste a Matematica Discreta).

### Probabilita condizionale
Indica la probabilità che $A$ si verifichi sapendo che si e' verificato prima $B$.
$$
P(A|B) = \frac{P(A \cap B)}{P(B)} 
$$

#### Esempio
Gioco alla roulette i numeri 3, 13 e 22. I numeri possibili (ossia $\Omega$) e' sono i numeri da $0 \text{ a } 36$. Sappiamo anche che la roulette e' truccata in modo che esca un **numero dispari**. Quindi posso calcolare due probabilita':
* Probabilità che esca $B = \{ 3,13,22 \}$ su $\Omega$, quindi considero il caso che la roulette non sia truccata
* Probabilità che esca $B$, sapendo che usciranno solo numeri dispari. Devo calcolare $P(B|A)$ con $A=\{ \text{ numeri dispari tra 0 e 36} \}$.

#### Esempio
Popolazione: $40\%$ fumatori e $60\%$ non fumatori. Il $25\%$ dei fumatori e il $7\%$ dei non fumatori hanno una malattia respiratoria cronica. Qual'e' la probabilità che un individuo a caso sia malato?
* $F=\frac{4}{10}$: l'individuo e' fumatore,
* $N = \frac{6}{10}$: l'individuo non fuma
* $M:$ l'individuo ha una malattia cronica.
	* $P(M|F) = \frac{25}{100} \cdot \frac{4}{10} \cdot \frac{10}{4} = 0.25$
	* $P(M|N) = \frac{7}{100} \cdot \frac{6}{100} \cdot \frac{100}{6} = 0.07$
Quindi: 
$$
P(M) = P(M \cap F) + P(M \cap N) = P(M|F) P(F) + P(M|N)P(N) = \frac{25}{100} \cdot \frac{4}{10} + \frac{7}{100} \cdot \frac{6}{10} = 0.142
$$
Si usa spesso la formula della probabilità **condizionale al contrario**: $P(A \cap B) = P(A|B) \cdot P(B)$.

Posso anche chiedermi la probabilità che una persona affetta dalla malattia respiratoria sia un fumatore:
$$
P(F|M) = \frac{P(M|F)\cdot P(F)}{P(M)} = \frac{25}{100} \cdot \frac{4}{10} \cdot \frac{1000}{142} = \frac{25 \cdot 4}{142}
$$

#### Formula di Bayes
siano $A_{i}$ **disgiunti uno a uno** ($A_{k} \cap A_{h} = \emptyset$) tali che la loro unione e' $\Omega$.
$$
P(A_{i}|B) = \frac{P(A_{i}) P(B|A_{i})}{P(B)} = \frac{P(A_{i}) P(B|A_{i}))}{\sum_{k=1}^nP(A_{k})P(B|A_{k})}
$$
la formula viene dal fatto che:
* $P(A_{i} \cap B) = P(A_{i})P(B|A_{i})$
* $P(B) = \sum_{k=1}^n P(A_{k} \cap B) = \sum_{k=1}^n P(A_k)P(B|A_{k})$

La seconda viene dal fatto che l'unione degli $A_{1} \cap B, \dots , A_{k} \cap B$ e' uguale a $B$. Essendo disgiunti posso fare la somma delle loro probabilità. Infatti $(A_{1} \cap B) \cup \dots \cup (A_{k} \cap B) = (A_{1} \cup \dots \cup A_{k}) \cap B = B$. (forse owo).

### Eventi indipendenti
Due eventi sono **indipendenti** se: 
$$
P(A\cap B) = P(A) P(B)
$$ e allora con rispettivamente $P(A)>0 \text{ e P(B) > 0}$ :
$$
P(A|B) = B \text{ e } P(B|A) = A
$$

 Se $A,B,C$ sono indipendenti due a due allora:
 $$
 P(A \cap B \cap C) = P(A) P(B) P(C)
 $$
# Calcolo Combinatorio
con $N$ di cardinalità $n$, e $K$ di cardinalità $k$:
> [!multi-column] 
> > [!note] Disposizioni
>  $$\#D^n_{k} = \frac{n!}{(n-k)!}$$
>  Dove $D^k_{n}$ e' la funzione: $f: K\to N$. Associa ad una tupla di $k$ elementi una tupla di $n$ elementi.
> 
>> [!note] Combinazioni
>> $$\#C^n_{k} = \binom {n}{k} = \frac{n!}{k!(n-k)!}$$
>> Dove $C^n_{k}$ e' l'insieme dei sottoinsiemi di $N$ con cardinalita' $k$


#### Esempio
Gioco alla cinquina secca $(1,2,3,4,5)$, si vince se i numeri escono nell'ordine. Qual'e' la probabilita di vincere?
L'insieme $\Omega$ e fatto di tutte le cinquine possibili: $w = (w_{1},\dots,w_{5})$ dove ogni $w_{i}$ e' un numero da 1 a 90.
L'insieme di estrazioni possibili, ossia delle disposizioni di 5 elementi su 90 numeri e': $\frac{90!}{85!} = 90\cdot 89 \cdot \dots \cdot 86$. Ho usato la formula delle **disposizioni**. Dato che ogni combinazione ha la stessa probabilità di uscire, allora mi basta calcolare: $\frac{85!}{90!}$.

Se invece giochiamo alla cinquina normale, ovvero mi interessa pescare $(1,2,3,4,5)$ in un qualsiasi ordine, allora uso la formula delle **combinazioni**: $\#C_5^{90} = \frac{90!}{5! 85!}$. Allora la probabilità richiesta e' $\frac{1}{\binom{90}{5}}$, con $\Omega = C^{90}_{5}$

#### Esempio
Probabilità che tra $n$ persone a caso almeno due festeggino il compleanno lo stesso giorno. Con $\Omega = S \cdot \dots \cdot S$ per $n$ volte e $S={1,\dots,365}$. Quindi un generico $w \in \Omega$ e' $w=(w_{1},\dots,w_{n})$.  Ora voglio sapere (evento $A$) quanti $w$ hanno almeno due componenti uguali, ma e' più semplice calcolare quanti $w$ hanno componenti diverse (evento $A^C$). $\# A^C$ non e' altro che il numero di disposizioni di $n$ elementi presi da $A^x$ quindi devo calcolare $\# A^c = \frac{365!}{(365-n)!}$, $\#\Omega = 365^n$. 
Quindi:
$$
P(A) = 1- \frac{\#A^c}{\# \Omega} - \frac{365!}{365^n(365-n)!}
$$

generalmente vale che: $P(A) = \frac{\#A}{\#\Omega}$


# Variabili Aleatorie
#### Esempio Introduttivo
Gioco alla roulette per tre volte 1 euro, sull'uscita del numero 29 e vado a considerare $X=\text{quanto ho guadagnato dopo 3 partite}$. Posso chiedermi:
* Qual'e' la probabilità che il mio guadagno salga?
* In media il capitale $X$ cresce o diminuisce?

>[!note] Variabile Aleatoria
> Si dice Variabile Aleatoria una funzione $X: \Omega \to \mathbb R$, tale che $\forall t \in \mathbb R \;\;\;\;\; \{  w \in \Omega: X(w)\leq t \}$. Ossia una funzione di $w$ per cui abbia senso calcolare $P(w; X(w)\leq t)$, in altre parole: **la probabilita che $X$ prenda valori piu piccoli di $t$**
> Piu in generale dovremo calcolare: $P(w; X(w) \in A)$ con $A \subset \mathbb R$, definendo $A \to P(w; X(w) \in A)$




