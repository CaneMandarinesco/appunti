> La Macchina di Turing e' un linguaggio che descrive algoritmi.

**Ma con la Macchina di Turing possiamo risolvere tutti i problemi?** E se c'e' un problema irrisolvibile, *magari esiste un modello di calcolo alternativo che invece lo risolve*.

Un macchina di Turing riconosce un insieme di parola, ma che vuol dire riconoscere?

>[!note] $L$ deciso da $T$
>$L \subseteq \Sigma^*$ e' **deciso** da $T$ se
>* $\forall x \in L: o_{T(x)}= q_A$
>* $\forall x \notin L: o_{T(x)}= q_R$
>
>Ossia: $\forall x \in \Sigma^{*}[o_{T(x)}= q_{A} \iff x \in L]$ e termina sempre.
>
> Quindi: $o_{T(x)}= {q_{A} \text { se } x \in L \; \lor \; q_{R} \text { se } x \in L^C}$

> **Nota**: T e' sempre in  grado di distinguere fra le parole di $L$ e le parole non in $L$, ossia la macchina $T$ gestisce scorrettamente tutte le parole in $\Sigma^*$

> [!note] $L$ accettato da $T$
> $L \subseteq \Sigma^*$ e' accettato da $T$ se:
> * $\forall x \in L: o_T(x)= q_A$
>* $\forall x \notin L: o_T(x) \neq q_A$, oppure **non termina**.
> 
> Ossia: $\forall x \in \Sigma^{*}[o_{T(x)}= q_{A} \iff x \in L]$ ma potrebbe non terminare.

$T$ e' in grado di dirci solo se $x$ appartiene ad $L$, ma non ci puo' dire con certezza se **non appartiene** ad $L$.

Allora:
* $L$ e' **decidibile** se $\exists T$ che lo decide.
* $L$ e' **accettabile** se $\exists T$ che lo accetta.

> Nota: ogni linguaggio **decidibile** e' anche **accettabile**.

Invece, sia $L \subseteq \Sigma^*$ e $L^{C}= \Sigma^{*}- L$, dunque la differenza tra linguaggio accettabile e decidibile sta in come $T$ gestisce $L^C$.


## Teorema 3.1
> $L \subseteq \Sigma^*$ decidibile se e solo se $L$ **accettabile** e $L^C$ **accettabile**.

**Dimostrazione**, **se $L$ decidibile** dobbiamo costruire $T_1$ che accetta $L$ e $T_2$ che accetta $L^C$:
* $T_1$ e' proprio $T$ che decide $L$, e per definizione va bene.
* $T_{2}$ invece e' $T$ ma con stati $q_{A}$ e $q_R$ invertiti, se vogliamo accettare $L^C$, dobbiamo invertire il significato dell'output di $T$.

**Dimostrazione**, **se** $L$ **accettabile** e $L^C$ **accettabile**, allora abbiamo le macchine $T_1$ e $T_2$ che accettano $L$ e $L^C$. Possiamo creare $T$ che simula $T_1$ e $T_2$ rispettivamente su primo e secondo nastro: ad ogni iterazione eseguo un istruzione di $T_1$ e poi una di $T_2$, finché una delle due **non accetta**.

> Prima o poi: $T_1$ o $T_2$ accettano. Se $T_2$ accetta allora $T$ rigetta.


## Esercizi da esame
* dimostrare che se $L_1$ e $L_2$ sono accettabili, allora $L_{1}\cup L_2$ e' accettabile
* dimostrare che se $L_1$ e $L_2$ e' accettabile, allora $L_{1} \cap L_{2}$ e' accettaabile.

## Funzioni calcolabili
Una macchina di Turing di tipo trasduttore, quando calcola una funzione deve calcolare solo quando sono definite, e prendendo il dominio da un'alfabeto finito.

> le funzioni **totali** sono quelle definite sull'intero dominio.

> [!note] $f$ calcolabile
> $f: \Sigma_{1}^{*} \to \Sigma_{2}^{*}$ e' calcolabile se esiste $T$ che per ogni $x \in \Sigma_1^*$ e' definita e per cui $T(x) = f(x)$

> E se $x$ non e' definita? $T(x)$ potrebbe non terminare, oppure scrivere un valore errato.
> Quindi, cosa succede quando $x$ non appartiene al linguaggio?

Ad ogni linguaggio posso associare: $\mathcal X_{L}: \Sigma^{*}\to \{0,1\}$, ossia la **funzione caratteristica** per cui:
* $\mathcal X_{L(x)}= 1 \text{ se } x \in L$
* $\mathcal X_{L(x)}= 0 \text{ se } x \notin L$ 

> [!nota] $\mathcal X_L$
> E' una **funzione totale** per qualsiasi $L$. Nota che $X_{L}(x)$ e' definita per ogni $x \in \Sigma^*$.


## Teorema 3.2
$\mathcal X_L$ calcolabile se e solo se $L$ e' decidibile. Quindi ogni linguaggio e' calcolabile tramite $\mathcal X_L$

**Dimostrazione, se $L$ decidibile** allora esiste $T$ tale che: $o_{T(x)}= {q_{A} \text { se } x \in L \; \lor \; q_{R} \text { se } x \notin L}$.

A partire da $T$ definiamo $T'$ a due nastri dove sul primo viene eseguita la computazione di $T(x)$, mentre sul secondo nastro viene scritto 1 se $T$ termina in $q_A$.

**Dimostrazione, se $\mathcal X_L$ calcolabile**  allora, essendo totale, esiste $T$ che calcola $\mathcal X_L$. Allora posso costruire $T'$ a partire da $T$ in modo che se scrive 1 sul secondo nastro, allora $T'$ accetta, altrimenti rigetta.

## Teorema 3.3
> Nota, ad $f: \Sigma^{*}_1 \to \Sigma^{*}_2$ possiamo associare $L_{f}= \{(x,y) \in {\Sigma^{*}}_{1} \times {\Sigma^{*}}_{2} \text { t.c. } y=f(x)\}$

Se $f: \Sigma^{*}_1 \to \Sigma^{*}_2$ e' **totale e calcolabile**, allora $L_{f} \subseteq \Sigma^{*}_1 \to \Sigma^{*}_2$ e' decidibile.

**Dimostrazione**: Con $T$ trasduttore che calcola $f$, definiamo $T'$ riconoscitore che con input $<x,y>$ con $x \in \Sigma^*_{1}$ e $y \in \Sigma^*_2$:
1. sul **primo nastro** contiene $<x,y>$
2. sul **secondo nastro** eseguo $T(x)$ con risultato $z$
3. se $y=z$ **accetta**

Quindi $L_f$ decidibile, essendo $f$ calcolabile posso decidere quali parole sono in $L$.


## Teorema 3.4
Con $f: \Sigma^{*} \to \Sigma^*_1$ una funzione, con $L_f$ decidibile allora $f$ e' calcolabile. 

**Dimostrazione**: Con $T$ riconoscitore che accetta per $y=f(x)$ e altrimenti rigetta ($o_T(x)=...$). A partire da $T$ definisco $T'$ che usa 4 nastri con $x$ sul 1o nastro:
1. scrive i=0 sul 1o nastro
2. enumera tutte le stringhe $y \in \Sigma^*$ che hanno lunghezza pari a $i$
	1. con $y$ una stringa di lunghezza $i$, scrivo $y$ sul secondo nastro
	2. sul terzo eseguo $T(x,y)$ se termina in $q_A$ scrivo sul nastro di output, altrimenti provo con altre stringhe.

> Voglia trovare in pratica $y$ per cui $y=f(x)$, ossia vogliamo calcolare $f$ conoscendo $L_f$. 


# per l'esame
### Se $L_{1}$ e $L_{2}$ accettabile allora $L_{1} \cup L_{2}$ accettabile
Basta che $T_1$ o $T_2$ accettano.

### Se $L_{1}$ e $L_{2}$ accettabile allora $L_{1} \cap L_{2}$ accettabile
Bisogna tenere in mente che se $x \in L_{1} \cap L_{2}$ allora $T_1$ e $T_2$ accettano sempre, ma se $x \notin L_{1} \cap L_{2}$ allora puo' essere che $T_1$ rifiuta mentre $T_2$ accetta o viceversa, quindi bisogna simulare entrambe le macchine e accettare se e solo se $T_{1}$ e $T_{2}$ accettano, altrimenti tocca rifiutare. 
