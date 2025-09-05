 > Quanti sono gli algoritmi? Proviamo a contarli usando i numeri transfiniti di cantor, o meglio, che cardinalita ha l'insieme degli algoritmi?

Dato che ad ogni algoritmo corrisponde una `macchina di Turing`, allora proviamo a enumerare l'insieme $\mathcal T$ definite sul alfabeto $\{0,1\}$ e a singolo nastro, più uno di output.

> [!note] $\mathcal T$
> considereremo "sempre???" $\mathcal T$ definita su $\{0,1\}$ con un nastro e eventualmente uno di output.


## Teo 5.1
Con $\Sigma$ insieme finito, allora $\Sigma^*$ costituito da parole di lunghezza finita e' enumerabile.

> nota, si considera sempre la parte intera **superiore** di $log n$

**Dimostrazione**: consideriamo $|\Sigma| = n$ e di conseguenza la codifica $c: \Sigma \to \{0,1\}^{log n}$.

Per costruzione di una funzione di **codifica**, allora $\forall s,t \in \Sigma ^{*} \text{ t.c. } s \neq t [c(s) \neq c(t)]$.

Con $p \in \Sigma^*$, e $k$ la sua lunghezza, allora $p=s_{1}s_{2}...s_{k}$ con $s_{i} \in \Sigma$.

$C(p)$ e' la concatenazione di: $c(s_{1}) c(s_{2)}... c(s_{k})$ e di conseguenza consideriamo $\micro(p) = 1 C(p)$ (ossia la codifica della parola preceduta da un 1, ma perche????).

> **Fino a qui abbiamo formalizzato la costruzione di $\micro$, ossia la codifica di una parola $p \in \Sigma^*$ preceduta dal carattere $1$.**

> $\micro(p)$ e' a tutti gli effetti un **Numero Naturale**, senza il carattere $1$ all'inizio potremmo avere codifiche con numero differente di zeri all'inizio, ma che rappresentano lo stesso numero naturale.

$\micro(p) \neq \micro(p') \text{ se } p \neq p'$, questo perche:
* se $k > k'$, sapendo che $\micro(p)$ ha $1 + k log n$ cifre allora $k'$ ha $1 + k' log n \leq 1 + k log n$ cifre.
* se $k=k'$ allora esiste almeno un simbolo in $p$ e in $p'$ nella stessa posizione ma differenti, per cui la codifica e' differente.

> Quindi $\micro$ e' una biezione fra $\Sigma^*$ e un sottoinsieme infinito dei numeri naturali, ed e' numerabile per cantor.
## Teo 5.2 
$\mathcal T$ e' numerabile.

**Dimostrazione**: per dimostrarlo bisogna trovare una biezione tra $\mathcal T$ e $\mathbb N$, ossia etichettare ogni $T$ con un numero naturale, una **numerazione**.

Con $T$ ad un nastro, con $Q$ insieme finito di stati, di cui $q_0$ stato iniziale, $q_1$ accettazione oppure stato finale se $T$ trasduttore, con $b^{Q}:  \mathcal Q \to \{0,1\}^m$ la codifica per gli $m=log|Q|$ stati allora, come abbiamo fatto per la macchina universale, rappresentiamo $T$ come $\beta_{T}= b^{Q} (q_0)-b^{Q}(q_{1}) \times ...$, **ossia la codifica in linguaggio** $\{1,0, + \times, -, f,s,d\}$ di $T$.

> $T$ e' univocamente caratterizzata una volta fissate: quintuple, stato iniziale e stato finale, e quindi **univocamente rappresentata** dalla parola $\beta _{T} \in \Sigma^*$, ossia:

$$
\forall T,T' \in \mathcal T: T \neq T' \iff \beta_{T'} \neq \beta_{T}
$$
Come conseguenza del Teorema 5.1, $\mathcal T$ e' **numerabile** e possiamo trasformare la codifica $\beta_T$ in un **numero naturale** $v(T)$ tali che $v(T) \neq v(T')$ semplicemente sostituendo ai simboli $+, \times, -, ...$ dei numeri ad una cifra.

## Quanti sono i linguaggi?
### Teo 5.3
Con $\Sigma$ alfabeto finito e $\mathcal L_\Sigma$ insieme dei linguaggi su $\Sigma$, se $\Sigma$ finito, allora $\mathcal L_\Sigma$ non numerabile per Teorema 4.6, dato che $\mathcal L_{\Sigma}= 2^{\Sigma^*}$.

### Def 5.1
Con $T \in \mathcal T_\Sigma$ di tipo riconoscitore,$L_{A(T)}\in L_\Sigma$ e' il sottoinsieme di $\Sigma^*$ di parole accettate da $T$. 

#### Corollario importante
> per ogni $T \in \mathcal T_{\Sigma}$ esiste un unico $L \in L_\Sigma$ tale che $L=L_A(T)$.
> Dato che $\mathcal L_\Sigma$ non numerabile e poiché $\mathcal T_\Sigma$ numerabile **esiste un linguaggio non accettabile**.

# Halting Problem
> Siamo nel contesto della ricerca di un Linguaggio accettabile ma non decidibile

Domanda, se $L_{1}$ non decidibile allora:
* e' sempre non accettabile?
* oppure può essere accettabile ma non decidibile?

Allora, con:
$$
L_{H}= \{(i,x): i \text{ contiene le cifre da 0 a 7 } \land i \text{ e' la codifica di Turing } \land T_{i(x)}\text{ termina} \} \subseteq \mathbb N \times \mathbb N
$$
si dimostra che $L_H$ accettabile, ma non lo e' $L_{H}^C$, dunque, non e' **decidibile**.

$L_H$ e' l'**Halting Problem**, ovvero mi dice se un programma con input $x$ termina, oppure no.

### Teo 5.4
$L_H$ e' accettabile.
**Dimostrazione**, creiamo $U'$ a partire da $U$, in modo che:
* $U'$ verifica che $i$ e' la codifica della macchina $T_i$, controlla che inizia con $2$ e che non contenga 8 e 9.
* accetta se e solo se $T_i(x)$ termina: sia se $T_i$ accetta che rigetta.

Ma se $(i,x) \notin L_H$ non posso concludere niente per come abbiamo costruito $U'$.

### Teo 5.5
$L_H$ **non e' decidibile**.
**Dimostrazione**, per **assurdo**: se $L_H$ fosse decidibile, allora esiste $T$ che accetta se $(i,x) \in L_H$ e rigetta se $(i,x) \notin L_H$.

1. Possiamo derivare $T'$ che accetta $L^C$ chiamando in black-box $T$.
2. Da $T'$ deriviamo $T^*$ che accetta se $T'(i,i)$ accetta, e **non termina se $T'(i,i)$ rigetta**.

> Possiamo ottenere $T^*$ semplicemente aggiungendo una quintupla per il loop, quando si raggiunge lo stato $q_R'$ (il nuovo stato finale) e sostituendo $q_R$ con $q_R'$ nelle quintuple di $T$.

> **Cercando di riassumere**: 
> * $T'$ accetta $L^C$
> * $T^*$ accetta se $T'(i,i)$ accetta, ossia se $T$ rigetta, **altrimenti non termina**

Dato che $T^*$ e' una macchina di Turing e puo' essere codificata, allora $\exists k$ per cui $T^{*}= T_k$. 

> Cosa succede a $T_k(k)$? Ossia la macchina che accetta se $T(k)$ rigetta, con input se stessa?

1. Se $T_k(k)$ accetta, allora $T'(k,k)$ deve accettare, ma allora $(k,k) \notin L_H$, quindi $T_k(k)$ non termina.
2. Ma per il ragionamento inverso, se $T_k(k)$ non termina, allora dovrebbe rigettare, quindi $(k,k) \in L_H$

entrambe le ipotesi portano ad una contraddizione, quindi $T^*$ non puo' esistere, ma essendo $T^*$ stata creata a partire da $T$ con semplici modifiche: $L_H$ non decidibile.

>[!note] Significato
> vuol dire che **non esiste un algoritmo** che avendo in input la descrizione di $A_i$ ed un input di $A_i$, possa verificare in un numero finito di passi se l'istanza terminerà in un numero finito di passi.
> Se esistesse, sarebbe in grado di operare su se stesso, quasi un paradosso.

# operazioni fra linguaggi
Definiamo $L^{C}= \{0,1\}^{*}- L$. e lavoriamo con linguaggi in $\{0,1\}^*$

### teo 5.6
$L$ e' decidibile se e solo se $L$ accettabile e $L^C$ accettabile.

==simile ai teoremi visti in precedenza!==

## riduzioni fra linguaggi
Vogliamo poter individuare relazioni tra due linguaggi del tipo: se un linguaggio e' accettabile/decidibile allora per rid
uzione altri linguaggi lo sono.

### def. `many to one`
$L_{1} \subseteq \{0,1\}^*$ e' many to one **riducibile** a $L_2$ se **esiste** $f: \{0,1\} \to \{0,1\}$ tale che:
$$
\forall x \in \{0,1\}^* [x \in L_{1}\iff f(x) \in L_2]
$$

> $L_1$ riducibile ad $L_2$: $L_{1}\leq_{m} L_2$.

Gode delle seguenti proprieta:
* $L \leq_{m}L$: riflessiva
* $L_{1}\leq_{m}L_{2} \land L_{2}\leq _{m} L_{3}\to L_{1} \leq_m L_{3}$.
### es
Per esempio, considerando ${L_{H}}_{\square}= \{i \in \mathbb N: T_i(\square) \text{ termina } \}$, ossia tutte le macchine che terminano con input $\square$ che e' riducibile a ${L_H}$, ossia: ${L_{H}}_{\square}$.

Dobbiamo definire funzione totale e calcolabile che per ogni $x \in \{0,1\}^*$  e ogni macchina $i \in \mathbb N$ allora associa $k \in \mathbb N$ tale che $(i,x) \in L_{H}\iff k \in {L_{H}}_\square$.  

svolgimento che non ho capito....

### e quindi come si usa la riduzione???
> 1: Con $L_1$ non decidibile e $L_2$ linguaggio tale che $L_{1} \leq_{m}L_{2}$, allora $L_{2}$ **non decidibile**.

**Dimostrazione**, con $f_{1,2}$ funzione che riduce da 1 a 2 potremmo decidere $L_1$ semplicemente guardando se $f(x) \in L_2$, ma se $L_1$ non decidibile, allora $f(x)$ non puo' decidere $L_1$!

> 2: Con $L_1$ non accettabile e $L_2$ tale che $L_{1} \leq_{m} L_2$ allora $L_2$ **non accettabile**.

**Dimostrazione**, simile  a prima.

> 3: Con $L_3$ decidibile e $L_4$ tale che $L_{4} \leq_m L_3$, allora $L_4$ **decidibile**.

Dimostrazione, similmente a prima, con $f_{3,4}$ posso decidere quando $f(x) \in L_4$ guardando se $x \in L_3$.

> 4: la stessa ma per **accettabilita**







