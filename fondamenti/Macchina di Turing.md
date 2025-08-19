**Problema**: ordinamento dei numeri. Si puo' risolvere grazie a bubble sort: alla passata $i$-esima, il numero contenuto in $c[n-i]$ e' il più grande dei precedenti.

Una persona, banalmente un essere umano, per poter eseguire l'algoritmo deve poter: ricordare un numero, leggere e scrivere sulle celle di memoria.

Quindi si puo' risolvere il problema usando: un insieme limitato di simboli (una squenza di numeri in input da $0$ a $9$), poche istruzioni (per riconoscere se $x$ e' piu grande di $y$) una memoria volatile limitata (mi basta ricordare un solo numero) e un meccanismo di lettura e scrittura sui dati in input, per scrivere l'output corretto!

>[!note] Soluzione ad un Problema
> La soluzione ad un problema e' una *sequenza di operazioni molto semplici* che:
> * dipende da una **porzione piccola** dei dati del problema  
> * l'esito dipende dallo stato dell'**esecutore**

* **Problema Somma**: dati due numeri naturali, $n$ e $k$, calcolare $n+k$
* **Problema Area**: dato un rettangolo $A$ di base $b$ e altezza $h$, calcolare l'area di $A$

Non sono problemi, **ma istanze di problemi**:
* "Quanto fa 5+2?"
* "Quanto misura l'area di un rettangolo la cui base e' 28 e la lunghezza e' 12?"

*Più formalmente: la soluzione e' un'algoritmo che per qualsiasi istanza del problema, lo risolve.*

E vorremo trovare una soluzione **semplice**, ossia composta di istruzioni elementari in un numero finito.

Bisogna anche che l'algoritmo riconosca quando l'istanza non ha **soluzione**.

> [!note] Macchina di Turing
> E' un **modello di calcolo** costituito da:
> * **memoria limitata**, ricorda un simbolo per volta.
> * **memoria di lavoro** ad accesso sequenziale.

**Macchina di Turing, Definizione 1.3**: con $\Sigma$ alfabeto finito, $\mathcal Q$ insieme finito di stati con $q_{0}\in \mathcal Q$ lo stato iniziale, e $\mathcal Q_{F}\subset \mathcal Q$ un insieme di stati finali.
Una *Macchina di Turing* $T$ su $\Sigma$ e $\mathcal Q$ e' un dispositivo dotato di:
* unita di controllo: ha uno stato in $\mathcal Q$
* nastro di lunghezza infinita, dove ogni cella $x \in \Sigma \cup \square$. 
* unta testina di lettura/scrittura, posizionata su una cella.
* $P$ insieme **limitato** di quintuple $<q_{1,}a_{1}, a_{2}, q_{2},m>$: se la testina e' nello stato $q_1$ e legge $a_1$, allora scrive $a_2$ e passa allo stato $q_2$, e si sposta usando l'azione $m \in \{\text{sinistra, destra, ferma}\}$.

**L'insieme delle parole finite costruibili** su $\Sigma$ e' $\Sigma^*$.

> Nota: $q_1$ non appartiene ad uno stato finale.

> Nota: $P$ e' l'imitato nel senso che la sua grandezza non dipende dall'istanza del problema. Nel caso della somma di due numeri implementata con la tabella, avrei un*'insieme infinito di quintuple e di simboli* (ad ogni coppia di numeri corrisponde la somma).

$T(x)$ e' la **computazione** di $T$ su input $x$, ossia l'applicazione delle quintuple $P$, partendo dallo stato $q_0$.

> [!note] macchina di Turing
> E' il metodo che risolve il problema, l'insieme delle quintuple.

>[!note] Passo elementare
>E' l'applicazione di una quintupla, un **ordine**, un'azione imperativa. Un'algoritmo e' composto sempre di passi elementari e non da istruzioni del tipo: calcola la primitiva di $F(x)$.

> Nota: le istruzioni della Macchina di Turing seguono il paradigma del tipo, se e' vera una certa condizione allora fai queste azion, ossia e' un **linguaggio imperativo**

# Ancora sulla macchina di turing
Eseguire una quintupla vuol dire:
* sovrascrivere il simbolo su cui si trova la testina (o riscrive il medesimo simbolo)
* cambiare stato interno (o non cambia)
* muovere la testina (o sta ferma)
Eseguita la prima quintupla, si cerca la prossima da eseguire.

In caso di una macchina a piu nastri, una quintupla e' definita come: $<q_{1},(a_1,...,a_{k}),(b_{1}, ..., b_{k}), q_{2},(m_{1}..., m_{k})>$

> [!nota] macchina di Turing
> Una macchina di Turing (m piccola) e' una quintupla $<\Sigma, Q, q_{0}, Q_{F,}P>$. Ossia un'algoritmo descritto secondo le regole che abbiamo introdotto di Alan Turing. 

> Nota: per capire quanti nastri ha la macchina, basta vedere un'istruzione di $P$. 

> Nota: invece Macchina di Turing e' il modello di calcolo.

Una macchina *con alfabeto infinito, e stati infiniti non e' realizzabile* fisicamente.

Inoltre, la notazione $\forall n \in \mathbb N <q_{i}, (n, \square), (n, \square), q_{n,}(d,f)>$ non e' valida, i numeri naturali sono infiniti: quindi alfabeto infinito e stati infiniti, di conseguenza quintuple infinite.

> **Quindi**: gli stati, simboli e quintuple devono essere **costanti**, ossia indipendenti dall'input.

### definizioni importanti
Lo **Stato globale** $SG$ e' una *fotografia* della macchina ad un certo istante, contiene una **descrizione della porzione non blank** del nastro, della posizione della testina e dello stato interno.

Una **Transizione** da $SG_1$ a $SG_2$ esiste quando c'e' una quintupla: $<q_{1}, \sigma_{1}, \sigma_{2}, q_{2}, m> \in P$. dove $q_1$ e $\sigma_1$ sono lo stato interno e il simbolo letto in $SG_1$, e $q_2$ e $\sigma_2$ sono relativi a $SG_2$ in accordo al movimento $m$ della testina.

Una **Computazione** $T(x)$ della macchina $T$ su input $x = <x_{1}, x_{2}, ..., x_{n>} \in \Sigma^*$ e' una successione di $SG_{0}(x), SG_{1}(x), ..., SG_{k}(x), ...$  dove:
* $SG_0$ stato globale iniziale di $T(x)$, dove la testina e' posizionata su $x_1$.
*  Se per $k \geq 0$, $SG_k$ e' uno stato finale, allora per $i > k$ non ci sono altri stati globali.
* Per ogni $i \geq 0$, per cui $SG_i$ non e' stato finale, allora esiste una transizione.

Se la computazione $T(x)$ **termina**, allora contiene uno stato finale.

> Nota: nello stato iniziale si assume di solito che la testina sia posizionata sul **primo carattere a sinistra della parola in input**.

Un **Trasduttore** e' una macchina di Turing che calcola un valore, o meglio, il valore di una funzione, quindi il risultato e' una sequenza di caratteri e lo stato finale della macchina non restituisce in formazione sull'esisto della computazione.

> Nota: si assume che i trasduttori abbiano **un solo stato finale** $q_F$ e almeno **due nastri**, uno dedicato all'output. 
> La testina sul nastro di output si muove solo da sinistra a destra e non puo' cancellare cosa e' stato scritto.
> Il nastro di output non e' conteggiato nel totale dei nastri usati dalla macchina di turing.

Un **Riconoscitore** e' una macchina di Turing che decide se la parola data in input *appartiene o non ad un insieme* che definisce il problema da risolvere. Queste macchine hanno almeno due stati finali: $q_{A}$ e $q_{R}$, ossia stato di **accettazione** o **rifiuto**.

Con $o_T(x)$ si indica l'esito della computazione $T(x)$:
* se $T$ trasduttore: allora $o_T(x)$ e' una funzione: $\Sigma^* \to \Sigma^*$
* se $T$ riconoscitore: allora $o_T(x)$ e' una funzione: $\Sigma^{*} \to \{q_{A}, q_R\}$

> Nota: la funzione e' sempre **e solo** definita per le computazioni che **terminano**.

Inoltre le macchine possono basarsi su uno di questi due modelli:
* tanti nastri a **testine indipendenti**
* tanti nastri a **testine solidali**
* **alfabeto binario** o non.
* ad **un solo nastro**

> E' dimostrabile che tutto cio che si puo' fare con un modello si puo fare con un'altro.

#### Da Riconoscitore a Trasduttore
Per trasformare un riconoscitore in un trasduttore basta includere $q_{A}$ e $q_R$ come simboli nel linguaggio del trasduttore e aggiungo le seguenti quintuple:
* $<q_{A}, (x_{1},...,x_{n}, \square), (x_{1}, ..., x_{n}, q_{A}), q_{F}, \text{fermo}>$
* $<q_{R}, (x_{1},...,x_{n}, \square), (x_{1}, ..., x_{n}, q_{R}), q_{F}, \text{fermo}>$
ossia se sono in stato di accettazione nel trasduttore, allora scrivo $q_A$ nel nastro di output, altrimenti scrivo $q_R$, ovviamente mi aspetto sempre di leggere blank sul nastro di output.

## Struttura di $P$
Possiamo che $P$ e' una corrispondenza: $\mathcal Q \times \Sigma \to \mathcal Q \times \Sigma \times \{\text{sinistra , destra, ferma}\}$, ma di che tipo?
### Totalita
> [!note] Totalita
> E' vero che per ogni  $(q_{1},s_{1}) \in (\mathcal Q - \mathcal Q_{F}) \times \Sigma$ allora esiste la tripla $(s_{2}, q_{2}, m) \in \Sigma$ per cui $<q_{1}, s_{1}, s_{2}, q_{2}, m> \in P$? Non e' detto, per esempio consideriamo il caso in cui si legge un input non valido, allora la macchina non fa nulla.

Una macchina di Turing, sia riconoscitore che trasduttore potrebbe non aver previsto certe condizioni (errori, input errato), per cui la macchina si potrebbe bloccare, ossia non trovare quintuple da eseguire e non essere in uno stato finale.

In questo caso, $P$ **non e' totale**. Possiamo pero' trasformare $P$ in modo tale da essere totale aggiungendo le istruzioni $<q,s,s,q_{R},\text{fermo}>$, per le coppie $(q,s)$ che non sono gestite da $P$, e quindi in questi casi, la **macchina non si arresta**!

Dopo aver modificato $P$, $o_T(x)$ rimane invariato.

Abbiamo imparato che per evitare casini, l'input deve rispettare le specifiche della macchina!

**Quindi**: la corrispondenza totale e' verificata.

> **Nota**: si assume che la computazione $T(x)$ non termini mai per input che non rispetta le specifiche della macchina.

> **Nota**: se non le specifichiamo. si assume che nello stato iniziale ci siano tutte le quintuple: $<q, \sigma, \sigma, q, \text{fermo}>$, secondo quanto detto prima

> **Nota**: il perche' la macchina si e' interrotta lo capisce l'utente guardando le specifiche della machina. Probabilmente cio' avviene per un input errato.

### E' una corrispondenza o una funzione?
Se $P$ e' una funzione, allora la macchina di Turing e' **deterministica**, altrimenti, se ricade nelle **corrispondenze** e' una macchina **non deterministica**.

Deterministica, nel senso che il comportamento della macchina e' predeterminato, nelle non deterministiche ci possono essere piu quintuple che iniziano con la stessa coppia $(q,s)$.

Il grado di non determinismo di una macchina e' $k = 3 |\mathcal Q| |\Sigma|$.

Una computazione non deterministica e' rappresentabile come un'albero, dove la radice e' $SG_0$ dove $k$ e' il numero massimo di figli di ogni nodo.

Una **computazione deterministica** di $NT$ e' il percorso da albero ad una foglia.

Nella macchina $NT$, $o_{T}(x)$ e':
* $q_A$ se **almeno** una computazione di $NT$ accetta.
* $q_R$ se **tutte** le computazioni non accettano
* non definito se altrimenti.

> La **formulazione** del `genio burlone pasticcione`, vuole l'intervento di questo genio che scelga la quintupla di eseguire in caso di piu di queste.
> La computazione e' non deterministica e il risultato dipende dalle scelte del `genio`, la computazione rigetta dunque, se e solo se tutte le scelte possibili rigettano.

**Teorema 2.1**: per ogni macchina $NT$, esiste $T$, tale per cui l'esito della computazione di $NT(x)$ coincide con $T(x)$.

*Dimostrazione*: con la tecnica della **simulazione**, possiamo usare $T$ che simula ogni ramificazione di $NT$, fino a che non trovo una foglia che accetta! Usiamo la simulazione a **coda di rondine**: simulo l'albero per $k=0$ stati, e al passo successivo incremento $k$ di 1. (ossia, simulo tutte le computazioni di lunghezza $k$)

> Nota: bisogna simulare in ampienza (e non in profondita) perche' potrebbero esserci computazioni che non terminano!

## $TM$ a un nastro e $k$ nastri, solidali e non.
Una macchina $T_k$ a $k$ nastri può essere simulata da $T_1$ a 1 nastro? O meglio, *posso fare in modo che entrambe calcolino la stessa funzione?*

Spoiler: **si**, quindi possiamo considerare macchine ad un nastro (piu 1 di output).

> $T_2$ **a testine indipendenti e' equivalente a** $T_3$ **a testine solidali**: *Possiamo simulare il comportamento di una macchina a testine indipendenti tramite una a testine solidali*.

**Dimostrazione** usando lo **shifting**, assumendo:
* $T_2$ a testine **indipendenti** con due nastri
* $T_3$ a testine **solidali** a tre nastri.
* L'input delle macchine non contiene $\square$, e le macchine non scrivono $\square$
Allora $T_3$ simula $T_2$ facendo uno **shift** del contenuto dei nastri, in modo che la testina di $T_3$ coincide con il carattere $*$ scritto sul terzo nastro.

Un movimento a $(\text{destra, destra})$ di $T_2$ si simula cosi:
* $<q_{1}, ({s_{1}}_{1}, {s_{2}}_{1}, *), ({s_{1}}_{2}, {s_{2}}_{2}, \square), q_{2}^*, \text{destra}>$: devo cancellare $*$ sul 3o nastro.
* $<q_{2}^{*}, (x,y,\square), (x,y,*), q_{2}, fermo>$: riposiziono correttamente il simbolo
Similmente per $(\text{sinistra, sinistra})$, invece quando $mov_{1} \neq mov_{2}$ devo fare lo shift dei due nastri, in modo che il carattere da leggere su ogni nastro sia allineato su $*$. Per il movimento $(\text{destra, sinistra})$
1. mi sposto verso l'ultimo carattere non $\square$ sul primo nastro
2. comincio a spostare a sinistra i caratteri sul primo nastro.
3. 1. e 2. ma al contrario sul secondo nastro
4. vai nello stato successivo secondo la quintupla originale.\

> Una macchina di Turing a 1 nastro può simulare una qualsiasi macchina a $k$ nastri.

* $T_1$: macchina di Turing che usa lo stesso alfabeto $\Sigma$ di $T_k$.
* Input di $T_k$ viene scritto su $T_1$ come: ${x_1}_{1}, ..., {x_{k}}_{1}, {x_1}_{2}, ..., {x_{k}}_{2}, ...$

Notiamo che ad un comando di $T_k$ corrispondono $k$ comandi consecutivi su $T_1$, quindi $T_1$ deve leggere uno dopo l'altro i simboli sui vari nastri (posti su un'unico nastro). Per farlo ho bisogno di eseguire $k$ **letture**:
* $<q_{1}, s_{1}, s_{1}, q(q_{1}, s_{1}), \text{destra}>$. Se leggo $s1$, allora entro nello stato $q(q_1,s_1)$, ossia ricorda che ero in $q_1$ e che ho letto $s_1$.
* ...
* $<q(q_1,{s_{1}}_{1}, {s_{1}}_{2}, ..., {s_{1}}_{k-1}), ..., ..., q(..., {s_{1}}_{k}), \text{sinistra}>$

A questo punto la quintupla e' verificata e puo' essere eseguita, **bisogna portare il nastro indietro** di $k$ celle.

Ora bisogna eseguire **sovrascrivendo** i caratteri
* passo a $q^{write}$
* per ogni $i=2,..,k$ sostituisco ${s_{1}}_{i}$ con ${s_{2}}_{i}$

Nell'ultimo passaggio vediamo come passare allo stato $q'$ e come fare il movimento $m'$.

* se $m=\text{destra}$, allora $q'=q_2$ e $m'= \text{destra}$
* Se $m = \text{fermo}$ allora devo tornare al primo carattere che avevo letto in precedenza. $q' = q^{\text{sin}}(q_{2}, k-1)$  con $m'=\text{sinistra}$.
* Se $m=\text{sinistra}$ invece devo tornare indietro di $2k-1$ (stessa cosa che succede per $\text{fermo}$).
> Sul nastro non sono forse scritte come ${x_{1}}_{1}, {x_{1}}_{2}, ..., {x_{1}}_{k}$????

## Cardinalita di $\Sigma$.
Ogni macchina di Turing definita su $|\Sigma| > 2$ puo' essere simulata da $T_{01}$, ossia una macchina che capisce solo $\{0,1\}$. 

Con $\Sigma = \{\sigma_{0}, ..., \sigma_{n-1}\}$ possiamo rappresentare ogni simbolo usando $k = log_{2}n$ cifre. Infatti per ogni $\sigma \in \Sigma$, $b(\sigma) = (b_1(\sigma), b_2(\sigma), ..., b_k(\sigma))$.

**Inoltre**: $b(\square)$ e' una sequenza di $k$ simboli vuoti.

Con $p$, la quintupla da simulare, e $P_1$ insieme delle quintuple che verificano l'esecuzione di una quintupla, allora $P_1(p)$ e' fatta cosi:
* $<q_{1}, b_{1}(\sigma), b_{1}(\sigma), q(b_1(\sigma), d)>$.
* $<q(b_1(\sigma)), b_{2}(\sigma), b_{2}(\sigma), q(b_1(\sigma), b_2(\sigma)), d)>$. e cosi via, similmente a quanto fatto prima per la riduzione da $k$ nastri ad un nastro.

Poi riavvolgo e sovrascrivo i caratteri: usando gli stati $q(\sigma, k)$ per capire quando fermarmi.

E infine vado a simulare l'insieme $P_2$ che si occupa di muovere correttamente la testina:
 * $m=f$, allora passo a $q'$, tanto sono gia tornato indietro.
 * $m=s$, allora devo spostarmi di sinistra sfruttando gli stati $q^{mv}(\sigma, k-2)$, per qualsiasi simbolo $a \in \{\square, 0, 1\}$
## simulazione a scatola chiusa
Immaginando di avere due macchine: $T_{PPAL}$ e $T_{DPAL}$, che accettano rispettivamente: l'insieme delle parole di lunghezza pari palindrome, e l'insieme delle parole di lunghezza dispari palindrome, vogliamo creare $T_{PAL}$ che data in input una qualsiasi parola, controlla che questa sia palindroma. (sono state progettate nella Lezione 3)

Possiamo fare cosi:
1. copia $x$ su secondo e terzo nastro.
2. simulo $T_{PPAL}$ sul secondo nastro, e se $T_{PPAL}$ termina con $q_{PP}^P$, allora $T_{PAL}$ puo' terminare in $q_P$.
3. simulo $T_{DPAL}$, sul terzo nastro, e se termina con $q_{DP}^D$, allora $x$ palindroma di lunghezza dispari e $T_{PAL}$ entra in $q_P$
4. altrimenti termina in $q_{NP}$

Simulare vuol dire, copiare le quintuple di $T_{PPAL}$ e $T_{DPAL}$ in modo che ognuna scriva sul proprio nastro, bisogna far diventare gli stati delle due macchine, stati interni della macchina $T_{PAL}$.

Dopo la copia dell'input sui nastri, $T_{PAL}$ deve entrare nello stato iniziale di $T_{PPAL}$ e poi se non termino, entro nello stato iniziale di $T_{DPAL}$.



---

Funzionamento:
* leggi il primo carattere, ricordalo e cancellalo con $\square$.
* vai all'ultimo carattere, se e' uguale a quello ricordato, allora scrivi $\square$ e torna al secondo carattere. Altrimenti termina in $q_{\text{non-pal}}$.
Se cancello tutti i caratteri della parola, allora e' palindroma.

Svolgimento...

## Somma di numeri naturali
Ricordandoci che la macchina di Turing ha una memoria limitata e che anche l'insieme delle quintuple lo e', non posso tenere in memoria tutte le combinazioni di numeri per fare la somma.

Facendo un'analogia con l'uomo, quando viene chiesto di fare `2134+334566`, applichiamo prima un'algoritmo poiche non siamo capaci di ricordare l'infinta tabella delle somme.

Svolgimento...

## Simulazioniiii