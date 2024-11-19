## regola del prodotto (o formula inversa)
$$
P(A|B) = \frac{P(A \cap B)}{P(B) } \to P(A \cap B) =P(A|B) P(B)
$$
$$
P(A \cap B \cap C) = P(A| B \cap C
) \cdot P(B|C) \cdot P(C)
$$
si applica nei casi del reinserimento delle palline. Un evento cambia le sorti di un'altro.
### esempio
Un'urna contiene 2 palline bianche, 3 rosse e 4 nere. Si estraggono 3 palline a caso una alla volta **senza reinserimento**.

1. Calcolare la probabilità di ottenere la sequenza **rosso, non rosso** sulle prime due estrazioni
2. Calcolare la probabilità di ottenere le sequenze (rosso, bianco, rosso)

Risposte: 
1. $P(R_{1} \cap R_{2}^C) = P(R_{2}^C | R_{1}) P(R_{1}) = \frac{6}{8} \cdot \frac{3}{9} = \frac{1}{4}$. Ovvero il complementare delle palline rosse per la probabilità di pescare una rossa
2. $P(R_{1} \cap B_{2} \cap R_{3}) = P(R_{3}|R_{1}\cap B_{2}) \cdot  P(B_{2}|R_{1}) \cdot P(R_{1}) = \frac{2}{7} \cdot \frac{2}{8} \cdot \frac{3}{9} = \frac{1}{42}$

> [!error] attenzione!
> la formula $P(A \cap B \cap C)$ va sviluppata correttamente, o il risultato potrebbe non essere corretto, in base a chi influenza cosa
> $$
> P(A \cap B \cap C) = P(A|B \cap C) \cdot P(B|C) \cdot P(C) 
>$$

## formula delle probabilita totali
partizione di eventi finita o numerabile: $\{  E_{i}: i \in I \}$ con $I = \{1,\dots,n\}$.
Allora la unione dei vari $E_{k}$ mi da $\Omega$ e inoltre sono **disgiunti due a due**.

Sia $A$ un'altro evento (contenuti in $\Omega$), come posso calcolare $P(A)$ se conosco $\{ P(E_{i})_{i \in I} \}$ e ${P(A|E_{i})}_{i\in I}$ (con $P(E_{i}) \neq 0$):
$$
P(A) = \sum_{i \in I} P(A|E_{i}) P(Ei)
$$
Se ho due eventi: $E_{1}=E$ e $E_{2} = E^C$ allora $P(A) = P(A|E)P(E) + P(A|E^C)P(E^C)$


### esempio
Un'urna ha 2 palline bianche e 1 nera e lancio un dado:
* esce il numero 1 metto 2 palline bianche
* escono i numeri 2 e 3 metto nell'urna 1 bianca e una nera
* escono 4,5,6 metto 2 palline nere
Poi si estrae una pallina a caso. Calcolare probabilità di estrarre una pallina bianca. 

$E_{1} = \{ \text{ esce 1} \}$, $E_{2} = \{\text{ escono 2 o 3}\}$, $E_{3} = \{ \text{ escono 4 o 5 o 6} \}$.
Ho che $P(E_{1}) = \frac{1}{6}$, $P(E_{2}) = \frac{2}{6}$, $P(E_{3}) = \frac{3}{6}$.
E ora:  $P(B|E_{1}) = \frac{4}{5}$, $P(B|E_{2})=\frac{3}{5}$, $P(B|E_{3})=\frac{2}{5}$.

Per concludere: $P(B) = P(B|E_{1})P(E_{1}) + P(B|E_{2})P(E_{2}) + P(B|E_{3})P(E_{3})$
ovvero: $\frac{4}{5} \cdot \frac{1}{6} + \frac{3}{5} \cdot \frac{2}{6} + \frac{2}{5} \cdot \frac{3}{6} = \frac{8}{15}$

> infatti in questo esercizio conosco le probabilità di $E_{1}, E_{2}, E_{3}$ (disgiunti due a due e la loro unione mi da tutti i numeri possibili del dado!) che influenzano a loro volta l'evento $B$: possibilità di pescare una pallina bianca
### formula di bayes
sappiamo che:
 * $P(A|B) = \frac{P(B\cap A)P(A)}{P(B)}$
 * inoltre $A \cap B$ e $B \cap A$ sono uguali quindi $P(A \cap B) = P(B \cap A) = P(B|A)P(A)$.  

> [!note] formula di bayes
> $$
P(A|B) = \frac{P(B|A)P(A)}{P(B)}
$$

>[!note] bayes + probabilita totali
> $$
P(E_{n}|B) = \frac{P(B|E_{n})P(E_{n})}{\sum_{i \in I} P(B|E_{i})P(E_{i})}
$$

#### esempio!
Lancio un dado:
* esce 1: considero un'urna di 4 B e 1 N
* esce 2 o 3: considero un'urna di 3 B e 2 N
* esce 4 o 5 o 6: considero un'urna di 2 B e 3 N

> calcolare probabilità  che sia uscito 2 o 3 sapendo di aver estratto una bianca

so che:
* $P(E_{1}) = \frac{1}{6}, P(E_{2})=\frac{2}{6}, P(E_{3})=\frac{2}{5}$
* $P(B|E_{1}) = \frac{4}{5}, P(B|E_{2})=\frac{3}{5}, P(B|E_{3})=\frac{2}{5}$

Quindi si chiede di calcolare: $P(E_{2}|B)$, ma conosciamo sappiamo $P(B|E_{2})$.
$$
P(E_{2}|B) = \frac{P(B|E_{2})P(E_{2})}{P(B)} = \frac{P(B|E_{2})P(E_{2})}{P(B|E_{1})P(E_{1}) + P(B|E_{2})P(E_{2}) + P(B|E_{3})P(E_{3})}
$$
ovvero:
$$
\frac{\frac{3}{5} \cdot \frac{2}{6}}{\frac{4}{5}\cdot \frac{1}{6} + \frac{3}{5} \cdot \frac{2}{6} + \frac{2}{5} \cdot \frac{3}{6}} = \frac{3}{8}
$$
Altre domande possibili:
> calcolare la prob. che sia uscito 1, sapendo di aver estratto una pallina bianca.

>calcolare la prob. che sia uscito 4 o 5 o 6 sapendo di aver estratto una pallina bianca.

> calcolare la prob. che si-a uscito 2 o 3, pescando una pallina nera

> calcolare la prob. che esca un numero dispari sapendo di aver estratto una pallina bianca

### indipendenza tra eventi
> [!note] indipendenza tra eventi
> $A$ e' indipendente da $B$ se: $$
P(A \cap B) = P(A) P(B)
$$

 > [!note] indipendenza tra eventi
 > $A$ e $B$ sono indipendenti se e solo se $$ P(A|B)=P(A)$$

dimostrazione: $$
P(A \cap B) = P(A)P(B) \iff \frac{P(A \cap B)}{P(B)} = P(A)
$$
dall'ultima formula segue la tesi, per definizione di $P(A|B)$

* un $A \text{ t.c } P(A)=0 \text{ oppure } P(A) =1$ e' **indipendente** da tutti gli altri
* se $A,B$ sono indipendenti, allora lo sono anche a coppie: $(A^c,B), (A,B^c) \text{ e } (A^c, B^c)$
* $A,B$ sono indipendenti se lo sono $A \text{ e } B^c$ oppure se lo sono $A^c \text{ e } B^c$

