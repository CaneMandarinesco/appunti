### def. Problema e Algoritmo
Un Problema e' un insieme di parole $X$ prese da $\Sigma^*$. Risolvere un'istanza di un problema vuol dire, capire $\forall x \in \Sigma^*$, quali $x \in X$, oppure calcolare una funzione $f(x)=y$ per $x \in X$ con $X$ insieme di parole per cui $f(x)$ e' calcolabile.

Un'algoritmo e' una sequenza di ordini, semplici, limitati:
* semplici: una sequenza di operazioni semplici definite dal modello di calcolo
* limitate: il numero di istruzioni che compone l'algoritmo e' costante rispetto all'input del problema 

### def. Macchina di Turing
E' un modello di calcolo, mette a disposizione una memoria ad accesso sequenziale, un'unita di controllo che permette di ricordare uno stato e delle istruzioni di tipo: se la condizione $x$ e' vera, allora fai questo.

O meglio, con $\Sigma$ insieme finito di simboli (alfabeto), $\mathcal Q$ insieme finito di stati:
* $k$ nastri, costituiti da infinite celle, dove ogni cella contiene un simbolo $x \in \Sigma \cup \{ \square \}$. Uno di questi nastri e' usato come input per la macchina, ossia vi viene scritta sopra una parola $w \in \Sigma^*$ su cui eseguire l'algoritmo.
* Unita di controllo che ricorda uno stato in $\mathcal Q$.
* Una testina di lettura e scrittura per ogni nastro: le testine possono spostarsi a destra o a sinistra a seguito di un'istruzione
* $\mathcal P$ insieme di istruzioni, o meglio ordini, dove per macchine a 1 nastro sono definite come quintuple: $<q_{i}, \sigma_{j}, \sigma_{k}, q_{l}, m>$ dove $q_{i}, q_{l} \in \mathcal Q$ e $\sigma_{j}, \sigma_{k} \in \Sigma$ e $m \in \{\text{sinistra, destra, fermo}\}$.
La quintupla e' un ordine da interpretare come: se sono nello stato $q_{i}$ e leggo sul nastro il simbolo $\sigma_j$, allora scrivo $\sigma_k$ e passo allo stato $q_l$ e muovo la testina con movimento $m$.
### 
$\Sigma^* = \{ \sigma_{1}\sigma_{2}... \sigma_{n}: n \in \mathbb N, \forall i \in \mathbb N, 1 \leq i \leq n [\sigma_{i} \in \Sigma]\}$
### def. macchina di Turing
Una macchina di Turing e' rappresentabile come una quintupla: $<\Sigma, Q, q_0, Q_{F}, P>$:
* $\Sigma$: simboli che la macchina comprende e che compaiono nelle quintuple
* $Q$: insieme di stati
* $q_0$: stato iniziale della macchina
* $Q_{F}\subseteq Q$: stati finali della macchina.
* $P$: insieme di quintuple che la macchina di Turing usa

Una macchina di Turing e' quindi un'algoritmo che risolve un problema specifico!
Ma cosa vuol dire eseguire un'algoritmo, o meglio computare?

### def. Transizione
$SG_{i}$ e' uno stato globale, ossia una fotografia della macchina di turing nel corso dell'esecuzione. E' rappresentata da una parola che descrive la porzione non `blank` del nastro (dato che e' infinito), dove viene aggiunto il carattere che rappresenta lo stato della macchina alla sinistra del carattere su cui e' posizionata la testina nel nastro.

Una transizione $SG_1$ a $SG_2$ avviene solo se c'e' una quintupla la cui esecuzione ha come stato quello di $SG_1$ e legge lo stesso carattere letto da $SG_1$ e mi porta a scrivere il carattere letto da $SG_2$ nel suo stato!

### def. Computazione 
Una computazione e' una successione, eventualmente infinita di $SG_i$: $SG_{0}SG_{1}... SG_{k} ...$ dove $\forall i > k, \in \mathbb N$:
* $SG_{i}$ non e' uno stato finale
* $SG_k$ 
* $SG_{0}$ e' in stato iniziale $q_0$, con la testina posizionata sul carattere piu a sinistra della parola in input.

### def. Trasduttore e Riconscitore
Un trasduttore e' una macchina che calcola un valore a partire dalla parola $x$ data in input: lo stato finale di un trasduttore non ci dice molto sul suo risultato, bisogna leggere cosa e' stato scritto sul nastro di output.

Un riconscitore e' una macchian che data una parola $x$ in input termina in $q_A$ se e solo se $x$ appartiene al problema definito, e se possibile, termina in $q_R$ se e solo se $x$ non appartiene al problema.

Quindi, un esempio di riconoscitore e' la macchina $T_{PPAL}$ che riconsce le parole palindrome di lunghezza pari.

Con $o_T(x)$ si indica l'esito della computazione:
* riconoscitore: $o_T(x)$ puo' essere $q_A$ o $q_R$, ossia calcola la funzione $\Sigma^{*} \to \{q_{A}, q_{R}\}$ per opportune parole in input.
* trasduttore: $o_T(x)$ e' la parola in output sul nastro, ossia calcola la funzione $\Sigma^{*}\to \Sigma^{*}$.

### da riconoscitore a trasduttore
Trasformare un riconscitore in un trasduttore e' semplice:
* $q_A$ e $q_R$ non sono piu stati finali, usiamo $q_F$ come stato finale per esempio
* aggiungiamo un nastro per l'output
* aggiungiamo il simbolo $q_{A}, q_R$ a $\Sigma$.
* aggiungiamo le quintuple: $<q_{A}, (x, \square), (x, q_{A}), q_{F}, f)> \forall x \in \Sigma$.
### dim. $P$ e' Totale

$P$ e' una corrispondenza $Q \times \Sigma \to Q \times \Sigma \times m$, ma e' **totale**?
Non sempre, per esempio, una macchina di Turing ha delle specifiche che devono essere rispettate. In caso contrario la macchina di Turing potrebbe leggere inaspettatamente un simbolo in uno stato $q$ e non trovare una quintupla da eseguire.

Ma sia trasduttori che riconoscitori con $P$ non totale possono essere trasformata in totale, semplicemente aggiungendo le istruzioni $<q,\sigma_{1}, \sigma_{1}, q, f> \forall (q, \sigma_{1}) \notin P$, ossia la macchina rimane ferma, all'infinito. 

Aggiungendo queste istruzioni, $o_T(x)$ rimane invariato!

Osserviamo quindi che una macchina di turing con $P$ totale, in caso di input errato potrebbe non terminare mai, oppure con $P$ non totale potrebbe arrestarsi in uno stato finale e in caso di un trasduttore potrebbe esserci scritto qualcosa sul nastro di output ma naturalmente non e' un risultato corretto.

### $P$ corrispondenza e $P$ funzione
* Se $P$ e' una funzione, allora $T$ e' deterministica, ossia ad ogni coppia $(q,\sigma)$ corrisponde una ed una sola azione.
* Se $P$ non e' funzione, allora e' corrispondenza e vuol dire che ad almeno una coppia $(q,\sigma)$ corrispondono piu quintuple.

Come rappresentiamo l'esecuzione di una macchina non determinstica (P corrispondenza)? Con un'albero, la radice e' $SG_0$, un'arco tra due nodi indica una transizione, quando un nodo ha piu di un figlio e' perche' ci sono piu quintuple da poter eseguire e un percorso da radice a foglia indica una computazione che ha terminato.

E' come se avessimo piu computazioni in parallelo, ma come decido quando una maccchina non deterministica di tipo riconoscitore accetta una parola? $NT$ accetta quando almeno una computazione tra le tante accetta, invece rifiuta quando tutte le computazioni rifiutano, altrimenti $NT$ e' indefinita (in caso tutte le computazioni non terminino mai)

### Teorema 2.1: per ogni $NT$ esiste $T$ tale che esisto uguale.

Usiamo $T$ che simula $NT$, ma come? Dato che un ramo di $T$ potrebbe non terminare mai mentre potrebbe essercene uno che termina come faccio ad non incappare in computazioni infinite?

Uso la simulazione a coda di rondine, in un nastro tengo un valore $k=1$ che viene incrementato ad ogni iterazione e $\forall k \in \mathbb N$ simulo  tutte le computazioni non deterministiche di lunghezza $k$.

Quando una di queste comptuazioni termina ain $q_A$ allora, posso terminare la simulazione e terminare in $q_A$, altrimenti termino in $q_R$ se termino.

### da $T$ testine indipendenti a testine solidali
Senza perdita di generalita considerando macchine che non scrivono $\square$, usando la tecnica dello shifting possiamo simulare le testine indipendenti su una macchina a testine solidali.

Ho bisogno di un nastro che contiene un simbolo, per esempio $*$, indica dove sono posizionate le testine, inizialmente sul carattere piu a sinistra.


```
----ax----
----ax----
-----x----
```

$<q_{1}, (a,b,c), (a',b',c'), q_{2}, (s,s,f)>$.
Per simulare questa istruzione ho bisogno di fare lo shift verso destra dei primi due nastri:
* mi sposto fino al blank a destra sul primo nastro: 
* comincio a spostare di un carattere
* 

### $T$ un nastro puo' simulare $k$ nastri

* l'input viene scritto come $x_{1,1} x_{1,2} ... x_{1,k}, x_{2,1} x_{2,2} ... x_{2,k}...$
* ad una istruzione di $T_k$ corrispondono $k$ operazioni per leggere k simboli consecutivi, e $k$ per scrivere.

### $\Sigma$ puo' essere ridotto a $\Sigma = \{0,1\}$

### $L$ decidibile se e solo se $L$ accettabile e $L^C$ accettabile

