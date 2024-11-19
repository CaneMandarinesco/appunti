## teorema di talete
dato un fascio di rette parallele tagliate da due trasversali, i segmenti su una trasversale sono **proporzionali** ai segmenti corrispondenti sull'altra.

## somma di vettori
$$
\vec{\mathrm{OP}''} = \vec{\mathrm{OP}} + \vec{\mathrm{OP}'}
$$
>[!note] somma di 3 vettori
> la somma di 3 vettori e' il vertice opposto a $O$ del parallelepipedo costruito sui 3 vettori (???)
### gruppo commutativo
$V^2_{O}$ e' un **gruppo commutativo**:
$$
\vec{\mathrm{OP'}} + \vec{\mathrm{OP''}} = \vec{\mathrm{OP''}} + \vec{\mathrm{OP'}}
$$

per la dimostrazione dobbiamo verificare che:
1. esiste un elemento neutro (si dimostra applicando il **vettore nullo** $\vec{\mathrm{OO}}$ alla somma)
2. esiste **l'opposto** (si dimostra prendendo il vettore $\vec{\mathrm{OA}'}$ rispetto ad $\vec{\mathrm{OA}}$ dove $A'$ e' simmetrico ad $A$ sulla retta passante per $O$ ed $A$)
![[Pasted image 20240308214925.png]]
3. vale la proprietà commutativa
4. vale la proprietà associativa

### proprietà 3. commutativa
$$
\vec{\mathrm{OA}} + \vec{\mathrm{OB}} = \vec{\mathrm{OB}} + \vec{\mathrm{OA}} 
$$
#### dim.
se $O, A, B$ **non sono allineati**, allora se proviamo a costruire il parallelogramma su questi, abbiamo che $O,A,B$ e $O, B,A$ *sono lo stesso parallelogramma*.  
se sono **allineati** allora la dimostrazione segue dalla congruenza fra segmenti.
### proprietà 4. associativa
$$
\vec{\mathrm{OP}} + (\vec{\mathrm{OP'}} + \vec{\mathrm{OP''}}) = (\vec{OP} + \vec{\mathrm{OP'}}) + \vec{\mathrm{OP''}}
$$
usando i punti in figura, come sul libro:
$$
\vec{\mathrm{OA_{1}}}  + (\vec{\mathrm{OA_{2}}} + \vec{\mathrm{OA_{3}}}) = (\vec{\mathrm{OA_{1}}}  + \vec{\mathrm{OA_{2}}}) + \vec{\mathrm{OA_{3}}}
$$
graficamente:
![[Pasted image 20240308215714.png]]

#### dim.
*nel caso i punti non siano allineati*, poniamo:
$$
\vec{\mathrm{OB_{1}}} = \vec{\mathrm{OA_{1}}} + \vec{\mathrm{OA_{2}}} \,\, \text{ e } \,\, \vec{\mathrm{OB_{2}}} = \vec{\mathrm{OA_{2}}} + \vec{\mathrm{OA_{3}}}
$$
e poniamo infine:
$$
\vec{\mathrm{OC}} = \vec{\mathrm{OB_{1}}} + \vec{\mathrm{OA_{3}}} = (\vec{\mathrm{OA_{1}}} + \vec{\mathrm{OA_{2}}}) + \vec{\mathrm{OA_{3}}}
$$
> [!note] oss.
> scrivendo in termini di $OB_{1}$ e $OB_{2}$ mettiamo in evidenza il fatto che per eseguire la somma usando la proprietà associativa possiamo seguire due strade diverse, ovvero prima calcolo $OB_{1}$ oppure calcolo per primo $OB_{2}$, e poi sommo con il vettore rimasto.
> ovviamente noi sappiamo che qualsiasi strada scegliamo otteniamo lo stesso risultato.

dobbiamo dimostrare che:
$$
\vec{\mathrm{OA_{1}}} + (\vec{\mathrm{OA_{2}}} + \vec{\mathrm{OA_{3}}}) = \vec{\mathrm{OA_{1}}} + \vec{\mathrm{OB_{2}}} = \vec{\mathrm{OC}}
$$
dato che abbiamo costruito dei parallelogrammi notiamo che:
* $\overline{B_{1}C}$ e' parallelo a $\overline{OA_{3}}$

dato che $O,A_{3},B_{2},A_{2}$ e' un parallelogramma:
* $\overline{OA_{3}}$ e' parallelo e congruente a $\overline{A_{2}B_{2}}$: quindi (per teorema 1.4 del libro) $O,A_{2},B_{2},A_{3}$ *e' un parallelogramma*
* per cui $B_{2}C$ e' parallelo e congruente a $A_{2}B_{1}$

dato che $O, A_{1},B_{1},A_{2}$ e' un parallelogramma:
* $A_{2}B_{1}$ e' parallelo e congruente a $OA_{1}$
* quindi: $B_{2}C$ e $OA_{1}$ sono paralleli e congruenti

da i due punti sopra possiamo affermare che $O,A_{1},C,B_{2}$ e' un parallelogramma e che: 
$$
\vec{OC} = \vec{OA_{1}} + \vec{OB_{2}}
$$
nel caso sono allineati: dimostrare come esercizio.

## moltiplicazione scalare
#formula_errata:
$$
\forall v = \vec{\mathrm{OP}} \in V_{0}^n, \forall t \in \mathbb{R} \; \exists! t \cdot v = t \cdot \vec{\mathrm{OP}} \in V_{0}^n
$$
giusto:
dove: $t \cdot \vec{\mathrm{OP}} = \vec{\mathrm{OP}_{t}}$ e $P_{t} \in A^n$  
ovvero: *per ogni vettore esiste un solo numero* $t$ che moltiplicato a $\vec{OP}$ ci da $\vec{OP_{t}}$  
abbiamo vari casi, le cui proprietà valgono se $\vec{\mathrm{OP}} \neq  \vec{\mathrm{OO}}$:  
### $t=0$
$$
P_{t=0} := 0 \to t \cdot \vec{\mathrm{OP}} = \vec{\mathrm{OO}}
$$
### $t>0$
il punto $P_{t}$ si trova nella retta di $\vec{\mathrm{OP}}$, dal lato di $P$, e $t$ si calcola nel seguente modo:
$$
t = \frac{len(\vec{\mathrm{OP_{t}}})}{len(\vec{\mathrm{OP})}}
$$

### t < 0 (simile a prima)
il punto $P_{t}$ si trova nella retta di $\vec{\mathrm{OP}}$, *dal lato opposto* a $P$, e $t$ si calcola nel seguente modo:
$$
-t = \frac{len(\vec{\mathrm{OP_{t}}})}{len(\vec{\mathrm{OP})}}
$$
---
quindi possiamo dire che esiste la funzione:
$$
\mathbb R \times V_{0}^n \to V_{0}^n
$$
detta: **moltiplicazioni scalari e vettori**

![[Pasted image 20240309140003.png]]

* $0 \cdot \vec{\mathrm{OA}} = \vec{\mathrm{OO}}$
* $2 \cdot  \vec{\mathrm{OA}} = \vec{\mathrm{OA}}+\vec{\mathrm{OA}}$
* $-3 \cdot  \vec{\mathrm{OA}} = -\vec{\mathrm{OA}}-\vec{\mathrm{OA}} -\vec{\mathrm{OA}}$
* ecc...
* $\emptyset_{\mathbb R} \cdot v = \emptyset_{V_{0}}$
* $t \cdot \emptyset_{V_{0}} = \emptyset_{V_{o}}$

## proprietà della moltiplicazione per scalare
 
1. $t(\vec{\mathrm{OA}} + \vec{\mathrm{OB}}) = t\cdot \vec{\mathrm{OA}} + t \cdot \vec{\mathrm{OB}}$
2. $(t+g)\vec{\mathrm{OA}} = t \cdot \vec{\mathrm{OA}} + g \cdot \vec{\mathrm{OA}}$
3. $(t \cdot g)\vec{\mathrm{OA}} = t(g\cdot \vec{\mathrm{OA}})$
4. $1\cdot\vec{\mathrm{OA}} = \vec{\mathrm{OA}}$ e $0 \cdot \vec{\mathrm{OA}} = \vec{\mathrm{OO}}$

inoltre abbiamo che:
$$
t \cdot  \vec{\mathrm{ OO}} = t \cdot (\vec{\mathrm{OO}} + \vec{\mathrm{OO}}) = t \cdot \vec{\mathrm{OO}} + t \cdot \vec{\mathrm{OO}}
$$

### 1. dim
per $O, A, B$ allineati e' banale, diversamente abbiamo che, ponendo $t>0$
![[Pasted image 20240309141259.png]]
poniamo:
* $\vec{\mathrm{OA_{1}}} = t \cdot \vec{\mathrm{OA}}$
*  $\vec{\mathrm{OB_{1}}} = t \cdot \vec{\mathrm{OB}}$
* $\vec{\mathrm{OC}} = \vec{\mathrm{OA}} + \vec{\mathrm{OB}}$

dobbiamo dimostrare che:
$$
t \cdot \vec{\mathrm{OC}} = \vec{\mathrm{OA_{1}}} + \vec{\mathrm{OB_{1}}}
$$
per **teorema di talete** abbiamo che i rapporti tra $OA_{1}$ e $OA$ ($= t$) e' uguale al rapporto tra $OC_{1}$ e $OC$. quindi il vettore $\vec{\mathrm{OC_{1}}}$ e' $t \cdot  \vec{\mathrm{OC}}$.  
ora $O,A,C,B$ e' un parallelogramma, e la retta passante per $C_{1}$ parallela ad $OA$ interseca la retta passante per $OB$ nel punto $B_{1}'$.  
per il **teorema di talete** abbiamo che i rapporti $OC_{1} \text{ e } OC$ ($=t$) e' uguale al rapporto fra $OB_{1}'$ e $OB$.  
quindi il vettore $OB'_{1}$ e' $t \cdot  \vec{\mathrm{OB}}$, e abbiamo che $B_{1}' = B_{1}$.  
quindi $O, A_{1},C_{1},B_{1}$ e' un parallelogramma e dunque $OC_{1} = OA_{1} + OB_{1}$

> nota: ho omesso il simbolo che identifica il vettore

## coordinate
con la formula $\vec{\mathrm{OP}} \cdot t$ posso indicare tutti i vettori possibili sulla retta di $OP$. $t$ e' la coordinata rispetto a $v = \vec{\mathrm{OP}}$.

## combinazioni lineari
![[Pasted image 20240309150029.png]]
quale' la combinazione di $x, y$ tale che: $x \cdot \vec{j} + y \cdot \vec{i} = \vec{\mathrm{OP}}$?
piu' in generale. esiste questa funzione?
$$
f: V_{0}^n \iff \mathbb R^n
$$
abbiamo vari casi a seconda di $n$.
### n=1
* $x$ si dice **coordinata** di $v=\vec{\mathrm{OP}}$
* e quindi: $V_{O}^1 \iff \mathbb R$

### n=2
* fisso $O \in A^2$ e fisso $\vec{i}, \vec{j} \in V_{O}^2$ con $\vec{i} \text{ e } \vec{j}$ **non multipli**
* la funzione: $V^2_{O} \iff \mathbb R \times \mathbb R$ e': $(x \cdot \vec{j}) + (y \cdot \vec{i}) \iff (x,y)$
posso dire che: 
$$
\forall \vec{v} = \vec{\mathrm{OP}} \in V_{0}^2 \;\; \exists!(x,y) \in R^2 \,|\, \vec{v} = \vec{\mathrm{OP}} = (x \cdot \vec{j}) + (y \cdot \vec{i})
$$
### n=3
* deriva da sopra
* $V_{O}^3 \iff R^3: x \vec{i} + y \vec{i} + z \vec{i} \iff (x,y,z)$
* $x,y,z$ sono sullo stesso piano (???)

