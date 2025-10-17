Oltre a decidere un linguaggio, perché non proviamo a **generarlo**?

> [!note] Grammatica
> Modello di calcolo generativo che genera tutte e sole le parole di un linguaggio.
> $G = (V_{T}, V_{N}, P, S)$
> * $V_T$ insieme dei simboli terminali  ($\Sigma$)
> * $V_N$ non terminali.
> * $S \in V_N$ e' un simbolo non terminale detto **assioma**.
> * $P$ insieme delle produzioni $(\alpha, \beta) \in (V_{T}\cup V_{N})^* \times V_{T}\cup V_{N}^*$
> c'e' almeno la produzione $S \to \beta$.  O meglio: $P⊆(V_{NT}​∪V_T​)^∗×(V_{NT}​∪V_T​)^*$
> 
> **Si vuole generare parole** di $V_T^*$

> Nota: $S$ e' un unico simbolo!

> Nota: ogni produzione $(\alpha, \beta)$ e' tale che $\alpha$ contiene un simbolo in $V_N$.

* $\alpha \beta$ e' la concatenazione di due parole
* $x \to_{G}y$ vuol dire che $x \in (V_{T}\cup V_N)^*$ contiene $\alpha$ e $y \in \backslash \backslash$ contiene $\beta$ e $\alpha \to \beta$ e' una **produzione** in $P$. 
* $x \to_{G}^* y$ vuol dire che $y$ deriva in $G$ da $x$ dopo una serie di produzioni (una sequenza di $x_{1}, ...,x_n$ dove da $x_n$ deriva $y$).
* $y$ **generata** da $G$ se: $S \to_{G}^{*}y$. 
* $L(G) = \{y \in V_{T}: S \to_{G}^{*}y\}$, ossia il linguaggio **generato** da $G$ a partire dall'**assioma**.
* $S \to a$ e $S \to B$ possono essere scritte come $S \to a | b$. 
* $\epsilon$ e' la parola vuota ed $a \to \epsilon$ e' una `epsilon-produzione`.


## Gerarchia di chomsky
Ha identificato 4 tipi di grammatiche con restrizioni sempre piu **crescenti** in modo che: $G_{0}$ include $G_1$ che include ... che include $G_3$.

$G_0$: **grammatiche formali**, o illimitate.
* $(\alpha,\beta)$ tali che $\alpha \neq \epsilon$ e  contiene almeno un non terminale.
* e' l'unica che puo' generare $\epsilon$
* **Importante!** $\alpha \neq \epsilon$.

$G_1$: `context-sensitive.
* $(\alpha, \beta)$ tali che $|\alpha| \leq |\beta|$. 
* Si puo' avere $S \to \epsilon$ **se e solo se** $S$ non compare mai a sinistra.
* Se $G$ contiene $aBA \to c$ allora e' una grammatica di tipo 0.

 $G_2$ sono le `context-free`
 * $(\alpha, \beta)$ tali che $\alpha \in V_N$. 
 * Se compare $aB \to cd$ allora $G$ e' necessariamente di tipo 1.

$G_3$ sono le `regolari`.
* $(\alpha, \beta)$ tali che $\alpha \in V_N$ e $\beta$ contiene un terminale preceduto o succeduto da un **non terminale opzionale**.

## Teorema $G1$
Sia $G$ una grammatica di tipo $t>0$ e $G'$ la grammatica ottenuta aggiungendo a $G$: il non terminale $S'$, che diventa anche **assioma** e le produzioni: $S' \to S$ e $S' \to \epsilon$.

**Allora**: $L(G') = L(G) \cup \{\epsilon \}$, che equivale ad aggiungere la produzione $S \to \epsilon$ in $G$, a patto che $S$ non compaia come termine destro in alcuna produzione.
## Teorema $G2$
> Con le grammatiche di tipo $t>1$ posso rimuovere la limitazione riguardo le $\epsilon \text{-produzioni}$.

Con $G$ grammatica tipo $t>1$ e $G'$ costruita aggiungendo un numero qualsiasi di $\epsilon$ produzioni.

**Allora:** $L(G') = L(G) \cup \{\epsilon\}$, quindi sto generando $L(G)$ di tipo $t>1$ aumentato della sola parola vuota.

Quindi al contrario, se a una grammatica $G$ gli rimuovo le $\epsilon \text{-produzioni}$ e ottengo una grammatica di $t>1$ allora ho lo stesso linguaggio ma senza $\epsilon$

> Vale solo per $t=2,3$. Se $t=1$ e applico $G2$ potrebbe variare $L(G')$, ossia comparirebbero altre parole nuove, oltre a $\epsilon$.

## Teorema $G3$
Per ogni $G_{t=0}$ esiste $G'$ ottenuta aggiungendo a una grammatica di tipo $t=1$ un numero arbitrario di $\epsilon \text{-produzioni}$ in modo che $L(G_{t=0}) = L(G')$.

## esempio
Progettare $G :=  <V_{T}, V_{N}, P, S>$ che genera $L = \{xx: x \in \{a,b\}^+\}$.

**Svolgimento**, $P$ contiene:
* $S \to a U_{a}S | b U_{b}S | X$. Ossia genero $x$, e mi ricordo che dovrò posizionare poi $U_x$ correttamente. $X$ e' il simbolo da usare per capire quando la prima parola $x$ termina.
* $U_{a}a \to a U_{a}$ ed $U_{a} b\to b U_{a}$, per far avanzare il simbolo $U_a$
* similmente a sopra ma invertendo $a$ con $b$, faccio avanzare il simbolo $U_b$ nella stringa
* $U_{a}X \to Xa, U_{b}X \to Xb$, per convertire il carattere
* $U_{a}X \to a, U_{b}X \to b$, per convertire l'**ultimo** carattere!

L'idea e' quella di far scorrere gli $U_x$ lungo la stringa, quando sono adiacenti a $X$ posso convertire $U_x$ in $x$. A sinistra di $X$ comparira la stringa $x$ nell'ordine corretto!

> **Oss**: e' grammatica di tipo 0!


#### dim. $L(G) \subseteq \{xx : x \in \{a,b\}^+\}$
Ogni parola generata da $G$ e' della forma $xx$.
Intanto, quando applico: $S \to X$, *non posso aggiungere più caratteri*.

ipotizziamo di aver applicato $n$ produzioni in modo da avere $x_{1}U_{x_{1}} x_{2}U_{x_{2}} ... x_{n}U_{x_{n}} X$. Ora il nostro scopo e' di eliminare i non terminali:
* $... x_{n}U_{x_{n}} X \to _{G} ... X x_{n}$ 
* oppure posso avvicinare il non terminale $U_{x_{i}}$ al non terminale $X$ per poi applicare $U_{x_{i}}X \to X x_{i}...$.
* Per ultima, applico la produzione per cui $... U_{x_{1}}X... \to_G x_{n-1} x_{n} x_{1} x_{2}... x_{n}$ 

E si puo' argomentare ancora...

# Grammatiche e MT
* La grammatica e' un modello di calcolo: genera parole appartenenti ad un linguaggio $L$. 
* La macchina di turing e' un modello di calcolo che riconosce parole che appartengono ad un linguaggio $L$. 

In che modo sono collegate?

> 1. Se $L$ accettato da $T$, allora esiste $G_{t=0}$ che la genera.
> 2. Se $L$ generato da $G_{t=0}$ allora esiste $T$ che la accetta.

### Teorema
Per ogni macchina $T$ ad un nastro, alfabeto $\{0,1\}$ esiste $T'$ con nastro semi-infinito (tutte le altre celle stanno a destra) che non scrive mai $\square$ che $\forall x \{0,1\}^{*}, o_{T}(x)= o_{T'}(x)$.

Ossia, posso simulare un nastro infinito, con uno semi-infinito.
## Teorema $G4$
Per ogni linguaggio accettabile $L$ esiste $G$ tale che $L = L(G)$.

**Dimostrazione**: con $L \subseteq \{0,1\}^*$ accettabile, e $T$ macchina che accetta $L$ con $Q = \{q_{0}, q_{1}, ..., q_{k}, q_{A}, q_{R}\}$.
Posso definire $G$ con: 
* $V_{T}= \{0,1, a, \square\}$
* $V_{N} = \{S,A,C,D,X,U_{0}, U_{1}\} \cup \{Q_{i}: i = 0, ..., k\} \cup \{Q_{A}, Q_{R}\}$.

in modo da costruire la parola $x$ in tre fasi:
* **costruisci** $x \; a \; Q_{0}\; x \; \square$ con $x \in \{0,1\}^*$.
* **simula** $T$ con input $x$.
* se $T(x)$ **termina** in $q_a$ allora **cancella** i caratteri di destra di `a` ed `a` stessa viene cancellata per lasciare $x$ sul nastro.

**Le produzioni in $P_G$ della fase 1** sono simili all'esercizio visto prima, si vuole generare una parola del tipo $xx$:
* $S \to 0 U_{0}A \square | 1 U_{1}A \square | a Q_{0}\square$ ed: $A \to 0 U_{0}A | 1 U_{1}A | X$.
* Per traslare gli $U_{0/1}$: $U_{0}0 \to 0 U_{0}$ ecc...
* Per trasformare $U_{0/1}$: $U_{0} X \to X 0$ ecc...
* E per l'**ultima trasformazione**: $U_{0}X \to a Q_{0}0 \;\; U_{1}X \to a Q_{0}1$.

> Si genera $x$ e il separatore $X$. Poi si traslano tutti gli $U_{i}$ e l'ultima traslazione genera i simboli $a Q_{0} i$.

> **OSS1**: per ogni $x \in \{0,1\}^*$ le produzioni in fase 1 generano $x a Q_{0}x \square$ e anche $aQ_{0}\square$.

Per simulare $<{q_{i}}_{1}, h_{1}, h_{2}, {q_i}_{2}, sx> \; \; \forall h_{1}, h_{2} \in \{0,1\}$ (ricordando che $T$ non legge e scrive blank) bisogna includere le produzioni $b {Q_i}_{1} h_{1} \to {Q_{i}}_{2}b h_2$ per ogni $b \in \{0,1\}$ e per $<{q_{i}}_{1}, \square, h_{2}, {q_i}_{2}, sx> \; \; \forall h_{2} \in \{0,1\}$ si aggiunge ${Q_{i}}_{1}h_{1} \to {Q_{i}}_{2}bh_{2}\square$

> Scrivo il nuovo stato a sinistra del precedente per simulare il movimento della testina.

Per simulare $<{q_{i}}_{1}, h_{1}, h_{2}, {q_i}_{2}, f> \;  \forall h_{1}, h_{2} \in \{0,1\}$  si aggiunge ${Q_{i}}_{1} h_{1}\to {Q_i}_{2}h_{2}$. e similmente a prima per $h_{1}=\square$.

Analogamente a quanto fatto per $sx$ si implementa il movimento $dx$. 

> **OSS2+3**: le produzioni in fase 2 si applica *solo alle parole che hanno il non terminale* $Q_{0}$ e ogni parola in fase 2 *contiene un e un solo **non terminale*** $Q_{i} \cup \{Q_{A}, Q_{R}\}$

> **OSS4:** in fase 2 le parole generate a destra del carattere `a` rappresentano gli stati globali della computazione. Allora verrà generato il non terminale $Q_A$ se $T(x)$ termina in $q_A$. 

> **OSS5**: se $T(x)$ non termina in $q_A$ allora qualunque parola generata in fase 2 contiene un non terminale, ossia uno stato $Q_{i} \cup \{Q_R\}$.

> **OSS6**: ogni parola generata in fase 2 termina con il carattere $\square$. 

Ora, dobbiamo cancellare i caratteri a destra di $a$:
* $Q_{A}b \to b Q_{A}$: fai scorrere $Q_{A}$ fino alla fine.
* $Q_{A} \square \to C$: se $Q_{A}$ e' alla fine, allora trasformalo in $C$
* $bC \to C$: mangiati tutti i caratteri prima, $\forall b \in \{0,1\}$
* $aC \to \epsilon$. elimina $x$ fratello!

> **OSS7**: la fase 3 si applica solo se viene scritto il non terminale $Q_A$.

> **OSS8**: come risultato di queste quintuple, viene cancellata la parola a destra di $a$, rimane solo $x$.

Concludiamo che, $\forall x \in L$:
* per **oss1** allora $S \to _{G}^{*}xaQ_{0}x$ 
* per **oss2**: posso applicare le produzioni della fase 2

E che, con $xay: xaQ_{0}x \to_{G}^{*}xay$, fino a che non e' possibile applicare altre produzioni, allora:
* $x\in L$ e $T$ accetta $L$ ed $y$ contiene il non terminale $Q_A$
* Posso applicare la fase 3
* e quindi, posso derivare la sola parola $x$ da $xay$.

Quindi $S \to_{G ^*}x$, ossia, che $x \in L(G)$.

## Teorema $G5$
Per ogni grammatica $G$ esiste $T$ che accetta $L(G)$.

Con $G=<V_{T}, V_{N}, P_{G}, S>$ definisco $NT$ con input $x \in V_{T}^*$ **che accetta solo se** $S \to _{G}^{*}x$.

step:
* scrivi $x$ sul primo nastro, il secondo deve essere vuoto.
* scrivi $S$ sul secondo nastro.
* *applica le produzioni in modo non deterministico*.

Ogni computazione deterministica verifica se la parola coincide con quella scritta sul primo nastro, se si, **accetta**.

Problema: il grado di determinismo di $NT$ potrebbe non essere **costante**: le produzioni potrebbero essere applicabili in "molti" modi diversi in un modo che non dipende da $P$.

$NT_G$ utilizza un terzo nastro sul quale e' memorizzato un intero $i$ e ad ogni passo applico ciascuna produzione a partire dalla posizione $i$ della parola generata fino a quel momento.

**Inizializzazione** di $NT_G$: scrive $S$ sul secondo nastro, e 1 sul terzo.

**Iterazione** di $NT_G$: simula tuttel e produzioni possibili sulla parola sul secondo nastro a partire dalla posizione $i$, aggiungi la quintupla (ramo non deterministico) che incrementa $i$ senza applicare altre produzioni.

Quando un ramo modifica la parola sul secondo nastro verifica se coincide col primo nastro e di conseguenza accetta. Se non posso applicare produzioni rigetta.

> Ora, il grado di non determinismo e' $|P_{G}|+ 1$ e non dipende da $x$ in input.

### $NT_G$ accetta $L(G)$?
Per costruzione, la parola $x$ viene accettata se generata da $G$.

### corollario
> Le grammatiche di tipo 0, sono **accettabili**. 

## altro
> **OSS**: dato che $L(G)$ e' infinito, ci sono rami deterministici che non terminano mai, ossia posso sempre applicare quintuple senza avere stringhe con soli terminali.

Questo e' vero perche' $NT_G$ e' una sequenza di operazioni di tipo: applica una produzione, se $x \notin L(G)$ per poter rigettare $NT_G(x)$ deve applicare  produzioni fino a quando non ha confrontato $x$ con tutte le parole in $L(G)$.

allora quando $x \notin L(G)$, allora $NT_G(x)$ **non rigetta**.