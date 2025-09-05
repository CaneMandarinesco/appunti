# grammatiche tipo 1 e decidibilita

> secondo quanto detto in precedenza... se $L(G)$ **ha infinite parole**, allora $NT_{G}$ del teorema $G5$ non rigetta le parole contenenti non terminali. **La computazione di $NT_G$ contiene computazioni deterministiche che non terminano**.

Infatti, ogni computazione di $NT_G$ e' una sequenza di operazioni del tipo: $\text{applica produzione} \to \text{verifica se la parola coincide con l'input}$. Ma se $x \notin L(G)$ allora devo applicare tutte le produzioni all'infinito!

> **OSS**: con $G$ grammatica di tipo 1, allora $\forall y: S \to _{G}y$, da $y$ non posso derivare parole di lunghezza minore. Nel tipo 1 ogni produzione aumenta la lunghezza della parola.

Per l'osservazione, allora posso progettare $NT'_G$ che **decide** se $x$ e' generata da $G$ di tipo 1, modificando la macchina del teorema $G5$: *basta rigettare la computazione quando viene generata una parola piu lunga di $x$*

Potrebbero nascere dei **loop di produzioni**: la lunghezza non aumenta mai, quindi $NT'_G$ *non e' in grado di rigettare*. 

*Ma come riconosco di essere in un loop?*
> **Soluzione**: se ho generato $|V_{T}\cup V_{N}|^{k} +1$ parole della **medesima** lunghezza $k$, allora le ho generate tutte, se vado in loop sicuramente ne genero una più volte.

Allora costruiamo $NT'_G$:
* ha un nastro che ricorda il valore: $|V_{T}\cup V_{N}^{|x|}|$ 
* il quinto nastro contiene il numero di parole della stessa lunghezza che sono state generate

E nel dettaglio:
* $x$ viene scritta sul primo nastro
* **Inizializzazione**: $NT_{G}^1$ scrive $S$ sul secondo nastro, 1 sul terzo e poi calcola.
* **Iterazione**: Generazione e Verifica.

**Generazione**: $NT_{G}^1$ simula tutte le produzioni non deterministicamente a partire dalla posizione indicata sul terzo nastro

> Se la produzione non aumenta il numero di caratteri allora somma in binario 1 al quinto nastro, altrimenti ri-inizializza a 1.
> Oppure: se leggo blank non applicare alcuna produzione.

**Verifica**, quando modifico la parola sul secondo nastro:
* se coincide col primo nastro **accetta**.
* se non contiene alcun non terminale, se non posso applicare produzioni, se e' piu lunga di $x$, o se il valore sul quinto nastro e' maggiore di quello sul quarto (ossia verifica il loop), **allora rigetta** (fondamentale fratelli).
* esegui un nuovo passo non deterministico.

### $NT^1 _G$ decide $L(G)$
> Dobbiamo dimostrare che $NT^{1}_G$ decide $L(G)$, quindi che viene **rigettata** ogni parola non in $L(G)$.

Con $x$ non in $L(G)$ e $y$ la parola sul nastro ad un qualsiasi istante
della computazione allora:
1. se $y$ contiene **soli terminali**, allora $y \in L(G)$ e poiché $x \notin L(G)$, allora $y \neq x$ e rigetto.
2. se $y$ contiene almeno un carattere non terminale e $|y| > |x|$ allora rigetta.
3. se $y$ ha un non terminale e $|y| \leq |x|$ allora $NT_{G}^1$ sostituisce $y$ con $z$ con $|z| \geq |y|$ fino a che non cado nel caso 1 o 2, o fino a quando non mi accorgo di essere in loop (vedi prima).

**Tutte le computazioni rigettano, non ci sono altri casi.**

Abbiamo dimostrato, $G6$: per ogni grammatica di tipo 1, esiste $T$ che decide $L(G_{t=1})$.

> Corollario: l'insieme dei linguaggi di tipo 1 e' sottoinsieme dei linguaggi decidibili.

>[!note] Riformuliamo la gerarchia delle grammatiche
> * $G_0$: coincide con i **linguaggi accettabili**.
> * $\mathcal D$: ossia i **linguaggi decidibili**, e' contenuto nei **linguaggi accettabili**
> * $G_1$: e' contenuto in $\mathcal D$.
> * ecc...

# grammatiche tipo 2
Non e' che per caso... $G_{2}= G_1$? O sono classi distinte?

## `pumping lemma` per linguaggi context-free.

**Enunciato**: Per ogni linguaggio `context-free` $L$ esiste $p_{L}>0$ (che dipende da $L$) tale che per ogni parola $z \in L$ se $|z| \geq p_L$ allora esistono cinque parole $u,v,w,x,y$ tali che.
1. $z=uvwxy$
2. $|vwx| \leq p_L$
3. $|vx| \geq 1$
4. $uv^{h}wx^{h}y$ e' in $L$ per ogni $h \geq 0$.

> Ossia una condizione necessaria **ma non sufficiente** perché un linguaggio sia di tipo 2.
> Ogni linguaggio context free soddisfa i punti 1, 2, 3, 4.

> Il lemma di *Bar-Hiller* non e' sufficiente, nel senso che esistono linguaggi context-free che non soddisfano i punti 1,2,3,4.

Importante.
> *Di conseguenza: si usa il lemma per dimostrare che un linguaggio **non e' context-free**.*
> Ossia, un linguaggio context-free che non soddisfa il lemma non e' context free.


## esempio di applicazione di pumping lemma
Si vuole dimostrare che $L_{a=b=c} = \{a^{n}b^{n}c^{n}: n \geq 1\}$ non e' di tipo 2.

Per assurdo: $L$ context-free, e $p_L>0$, e consideriamo $a^{p_L}b^{p_L}c^{p_L}$, poiché $|z|= 3p_{L}> p_L$ allora può essere scritta come nel **punto 2**.

Inoltre, $uv^{h}wx^{h}y$ per ogni $h \geq 0$ e' in $L_{a=b=c}$, in particolare: $uwy \in L_{a=b=c}$.

Poiché $|vwx| \leq p_L$, il carattere $a$ più a destra e la $c$ più a sinistra distano $p_{L}+1$ posizioni e quindi $vwx$ contiene al massimo 2 simboli distinti, ossia una tra $v,w,x$ e' parola vuota.

## chiusura
**Unione**: Con $L_1$ e $L_2$ `context-free` generate da $G_1$ e $G_{2}$ allora $L= L_{1}\cup L_{2}$ e' `context-free`: poiché la grammatica ha $P = P_{1} \cup P_{2} \cup \{S \to S_{1}, S \to S_{2}\}$ e' context free e genera $L$.

**Intersezione**: come prima, ma $L = L_{1} \cap L_{2}$, con $L_{1}, L_{2}$ `context-free`, allora non e' detto che anche $L$ lo sia, **per controesempio**, $L_{a=b=c}$ e' l'intersezione dei linguaggi $L_{a=b,c}$ e $L_{a,b=c}$ `context-free`, ma non e' context-free.

> [!note] chiusura
> * (**Teo G.7**) i context-free **sono chiusi** rispetto all'unione. 
> * (**Teo G.8**) i context-free **non sono chiusi** rispetto all'intersezione.
> * (**Teo G.9**) i context-free **non sono chiusi** rispetto al complemento.

> OSS: i context-free sono decidibili, essendo contenuti nelle grammatiche di tipo 1.


## PDA

> [!note]
> $PDA = <\Sigma, \Gamma, Q, Q_{F}, q_{0}, Z_{0}, \delta>$ e' un modello di calcolo costituito da:
> * unita di controllo
> * **2 nastri**: $n_1$ opera su alfabeto $\Sigma$ e $n_2$ su $\Gamma$
> * **2 testine**: la testina su $n_{1}$ e' posizionato sul *carattere piu a sinistra*, mentre $n_{2}$ e' posizionato sul *carattere piu a destra*. **Sempre!**
> * $\delta: Q \times (\Sigma \cup \{\epsilon\}) \times \Gamma \to \mathcal P(Q \times \Gamma^*)$ che server per decidere quale quintupla eseguire.
> * Nota: il 2o nastro e' gestito con politica **FIFO**

Ci sono due casi per l'esecuzione delle quintuple:
* se $(q_{1}, y) \in \delta(q_{0}, a, Z)$ allora viene sovrascritto il primo carattere sul secondo nastro con $Z$, su $n_1$ la testina si sposta a destra
* se $a=\epsilon$, allora la testina sul primo nastro sta **ferma**.
* se $y = \epsilon$, allora la testina sul secondo nastro **cancella** il *simbolo in cima alla pila*.

> Le $PDA$ accettano le grammatiche di tipo 2.


## grammatica tipo 3

### Teo G.14
$\forall ASFD \; A = <\Sigma, Q, q_{0,}Q_{F,}\delta>$ allora esiste una grammatica $G_{A}= <V_{T},V_{N},A_{0}, P>$ tale che $L(A) = L(G_A)$.

Infatti, posso costruire a partire da $A$ la grammatica $G_A$ tale che:
* $\forall q_{k} \in Q, a \in Sigma [\forall q_{i} \in \delta(q_{k}, a) \text { allora } A_{k} \to aA_{i} \in P]$.
* e similmente per $q_i$ stato finale, ecc...

Con $V_{T}=\Sigma$ e con $Q = \{q_{0}, q_{1}, ..., q_{k}\}$ allora $V_{N}=\{A_{0}, A_{1}, ..., A_{k}\}$ dove $A_0$ e' l'assioma.

### Teo G.15
Per ogni grammatica $G=<V_{T},V_{N},P,S>$ esiste un $ASFND$ $A_{G}= <\Sigma, Q, q_{0}, Q_{F}, \delta>$ tale che $L(G)=L(A_{G})$.

Con $\Sigma = V_{T}$ allora per ogni produzione in $G$ del tipo $A_{k} \to aA_{i}$, allora $q_{i} \in \delta(q_{k},a)$ dove.

Se ho $A_{k}\to a$ allora $q_{i} \in \delta(q_{k}, a)$ dove $q_i$ e' stato finale.
Se ho $A_{0}\to \epsilon$ allora $q_{F}\in \delta(q_{0}, \square)$.

Dunque per costruzione, ad ogni grammatica che genera $x$, allora esiste una computazione deterministica di $A_G$ che accetta quella parola, ossia $A_G$ riconosce determinate sequenze di parole che appartengono a $G$.

### Teo G.16
Per ogni $ASFND \;NA = <\Sigma, Q, q_{0}, Q_{F}, \delta>$ esiste un $ASFD A = <\Sigma_{D}, Q_{D}, q_{0D}, Q_{FD}, \delta_{D}>$ tale che $L(NA)=L(A)$

Con $Q_{D}= 2^Q$, ossia ogni stato di $Q_D$ e' un sottoinsieme degli stati di $NA$, e con $q_{0D} = \{q_0\}$ e con $Q_{FD} = \{Q' \subseteq Q: Q' \cap Q_{F}\neq \emptyset\}$ e $\forall a \in \Sigma \text{ e }\omega \in Q_D$ allora $\delta_{D}(\omega, a) = \bigcup_{q\in Q}\delta(q,a)$

### chiusura del tipo 3
E' chiusa rispetto all'**unione**, infatti $L_{1} \cup L_{2}$ puo' essere deciso da $ASFND$ che usa $A_{1}$e $A_{2}$.

E' chiusa rispetto al complemento, infatti $L^C$ puo' essere deciso da $ASFND$ $NA$, ma allora esita $ASFD$ $A$ che decide $L$, dunque posso invertire gli stati finale di $ASFD$ per accetta

E' chiusa rispetto all'intersezione, infatti $L_{1} \cap L_{2} = (L_{1} \cup L_2)^C$.





