
> [!note] misura di complessita
> $c(T,x)$ che rispetta assiomi di blum:
> * definita solo per $T(x)$ che termina
> * calcolabile

Studio delle risorse richieste da un'algoritmo per eseguire.

>[!note] $dtime$
> $dtime(T,x) = \text { numero di operazioni che impiega una macchina deterministica per terminare }$

> [!note] $dspace$
> $dspace(T,x) = \text{ numero di celle che usa una macchina T deterministica per terminare}$

Le due misure sono indefinite per computazioni che non terminano.
Verifichiamo gli assiomi di blum per $dtime$:
* per definizione e' definita solo su computazioni che terminano
* e' calcolabile usando la macchina $U_{dtime}$, ossia la macchina universale che ad ogni istruzione di $T$ simulata, aggiunge un 1 sul 5o nastro, scrivendo il valore in unario.

per $dspace$ si dimostra similmente.

## Macchine Non Deterministiche
Per macchine non deterministiche abbiamo le misure:
$ntime$ e $nspace$ ossia, misurano la quantità di risorse usate da macchine non deterministiche.

Per computazioni che accettano $ntime$ e $nspace$ misurano il numero minimo di risorse richieste per ottenere una computazione accettante.

Invece per computazioni che rigettano, si ottiene il numero massimo per una computazione che rigetta.

## $dspace(T,x) \leq dtime \leq dspace(T,x)|Q| |\Sigma + 1|^{dspace(T,x)}$

Ossia che $dtime$ e' compreso tra il numero di celle utilizzate e le combinazioni possibili di stati della macchina, ossia:
* $dspace$ sono celle usate
* $|Q|$ per ogni stato su ogni cella
* $|\Sigma + 1|$ per ogni possibile carattere su ogni cella e ogni stato
* elevato alla $dspace$ per tenere conto di tutte le parole

La parte a sinistra e' verificata perche' in condizioni standard, se uso $dspace$ celle, allora ho almeno $dspace$ operazioni di lettura su queste.

La parte a destra e' verificata perche' se ci fossero piu di $k(T,x)$ operazioni, allora vuol dire che nella computazione $T(x)$ ci sono due stati $G_i$ e $G_k$ con $i < k$ per cui $G_{i}= G_k$. Ma essendo $T$ deterministica questo vuol dire che per qualche $l$, allora $G_{i+l} = G_{k+l}$, e quindi non esiste uno stato finale, poiche' l'esecuzione va in loop!

### Teorema 6.2
> Con $f: \mathbb N \to \mathbb N$, se $L$ accettato da $NT$ tale che per ogni $x \in L$ $ntime(NT,x) \leq f(|x|)$, allora $L$ decidibile.

**Dimostrazione**: Dato che $f$ calcolabile e totale, allora esiste un trasduttore $T_f$ che prende in input $|x|$ in unario e scrive in output il valore $f(|x|)$ in unario.

Quindi posso usare $NT'$ che usa due nastri per simulare $T_f$, e poi per ogni computazione non deterministica di $NT$, ad ogni istruzione viene decrementato il valore sul 3o nastro, semplicemente cancellando gli uni.

Se $NT$ non acetta o rigetta entro $f(|x|)$ istruzioni, allora rigetto.

> NOTA: grazie ad $f(|x|)$ so quando rigettare, perche' una computazione accettante impiega al massimo $f(|x|)$, e quella computazione esiste sempre.

## accelerazione lineare
Con $L \subseteq \Sigma ^*$ deciso da $T$ deterministica ad un nastro, con $dtime(T,x) = t(|x|)$, allora per ogni $k > 0$ e $\forall x \in \Sigma^*$:
* $\exists T_{1}$ per cui $dtime(T_{1}, x) \leq \frac{|x|}{k} + O(|x|^2)$.
* $\exists T_2$ a DUE NASTRI per cui $dtime(T_{2},x) \leq \frac{|x|}{k} + O(|x|)$.

> Ossia, dato un'algoritmo, ne esiste sempre uno piu veloce di un fattore costante.


E' sempre possibile comprimere l'input:
* con un nastro ci vuole $O(|x|^2)$ passi
* con due nastri ne bastanof $O(|x|)$

## classi
Per l'accelerazione lineare e' possibile avere algoritmi sempre piu veloci, ma limitati dalla classe $O(f(x))$ di appartenenza. piuttosto che individuare il migliore algoritmo si preferisce discriminarli in classi.

* $DTIME[f(n)] = \{L \subseteq \{0,1\}^{*} \text{ per cui } \exists T \text { che lo decide }: \forall x \in L dtime(T,x) \in O(f(|x|))\}.$
* $DSPACE[f(n)] = \{L \subseteq \{0,1\}^{*} \text{ per cui } \exists T \text { che lo decide }: dspace(T,x) \in O(f(|x|))\}$.

> $DTIME$ e $DSPACE$ sono insiemi di linguaggi.

* con $T$ non deterministica: $NTIME[f(n)] = \{L \subseteq \{0,1\}^{*}\text { per cui } \exists T \text { che lo ACCETTA }: ntime(T,x) \in O(f(|x|))\}$
* * $NSPACE[f(n)] = \{L \subseteq \{0,1\}^{*}\text { per cui } \exists T \text { che lo ACCETTA }: nspace(T,x) \in O(f(|x|))\}$
Di conseguenza si definiscono $coDTIME$, $coDSPACE$, ecc... :
* $coDTIME[f(n)] = \{ L \subseteq \{0,1\}^{*} \text {per cui } L^{C} \in DTIME[f(n)]\}$

> [!note] funzione limite
> $f(n)$ che definisce una classe di complessita e' la **funzione limite**. Deve essere calcolabile e totale, altrimenti a che cazzo serve se non la posso calcolare?


### Teorema 6.8
per ogni $f: \mathbb N \to \mathbb N$ allora
1. $DTIME[f(n)] \subseteq NTIME[f(n)]$
2. $DSPACE[f(n)] \subseteq NSPACE[f(n)]$

Effettivamente, una macchina deterministica e' una macchina non deterministica con grado di non determinismo pari a 1.


### Teorema 6.9
per ogni $f: \mathbb N \to \mathbb N$ allora:
1. $DTIME[f(n)] \subseteq DSPACE[f(n)]$
2. $NTIME[f(n)] \subseteq NSPACE[f(n)]$

Sappiamo che $dtime(T,x) \leq f(n)$ e quindi $dtime(T,x) \in O(f(n))$. Inoltre per il primo teorema, $dspace(T,x) \leq dtime(T,x)$, dunque $dspace(T,x) \in O(f(n))$. Quindi $L \in DPSACE[f(n)]$.

### Teorema 6.10
per ogni $f: \mathbb N \to \mathbb N$ allora:
1. $DSPACE[f(n)] \subseteq DTIME[f(n)]$
2. $NSPACE[f(n)] \subseteq NTIME[f(n)]$

Per il primo teorema: $dtime(T, |x|) \leq dspace(T, |x|) |Q| (|\Sigma| + 1)^{dspace(T, |x|)}$, ossia $2^{log(dspace(T, |x|))} |Q|2 ^{log(\Sigma + 1) dspace(T, |x|)} = |Q|...$.

### Teorema 6.11
* $DTIME[f(n)] = coDTIME[f(n)]$
* $DSPACE[f(n)] = coDSPACE[f(n)]$.

allora: con $L \in DTIME[f(n)]$, allora esiste $T$ per cui $dtime(T, |x|) \leq f(|x|) \forall x \in L$ e dunque $dtime(T, |x|) \in O(f(n))$.

Dato che $T$ decide $L$, posso creare $T'$ che decide $L^C$. $dtime(T', x) = dtime(T, x) \in O(f(n))$ dato che basta invertire $q_{A}$ e $q_R$ in $T$.

### Teorema 6.12
$f, g$ funzioni totali, calcolabili tali che $f(n) \leq g(n)$ definitivamente per $n \geq n_0$ per qualche $n_{0}\in \mathbb N$, allora $DSPACE[f(n)] \subseteq DSPACE[g(n)]$ ecc..

Banalmente, se $L \in DSPACE[f(n)]$, allora esiste $T$ che decide $L$ in $dtime(T,x) \in O(f(n))$. ma $f(n) \in O(g(n))$, dunque $L \in DSPACE[g(n)]$.

### Teorema 6.13
Esiste $f(n)$ per cui $DTIME[2^{f(n)}] \subseteq DTIME[f(n)]$

### `time-constructible` e `space-constructible`
> [!note] `time-constructible`
> $f: \mathbb N \to \mathbb N$  e' `time-constructible` se esiste $T$ trasduttore che calcola $f(n)$ in unario ed ha $dtime(T,x) \in O(f(n))$.
> Nota: $f(n)$ e $n$ sono in **unario**!!!!

Similmente per `space-constructible`.

> Nota: $1^n$ indica una sequenza di $n$ simboli $1$.
> Nota: il valore dell'input e' la lunghezza dell'output in una macchina che testimonia il `time-constructible`.


> [!warning] tempo di rigetto
> Pur conoscendo il tempo massimo per cui il ramo di una macchina non deterministica rigetta, questa potrebbe avere infiniti rami! Dunque, il tempo di rigetto potrebbe essere infinitamente piu grande di $ntime$.


> [!note] decidibilita da $NT$ a $T$
> Tutto cio che e' deciso da $NT$ non deterministica, puo' essere deciso da $T$ deterministica.

### Teorema 6.16
> Con $f: \mathbb N \to \mathbb N$ una funzione `time-constructible` se $L \in NTIME[f(n)]$ allora $L$ decidibile in tempo non deterministico $O(f(n))$.

> Con $f: \mathbb N \to \mathbb N$ una funzione `space-constructible`, se $L \in NSPACE[f(n)]$ allora $L$ e' decidibile in spazio non deterministico in $O(f(n))$.

**Dimostrazione**: se $L \in NTIME[f(n)]$ e $NT$ la macchina che accetta $L$, allora $ntime(NT,x) \leq cf(|x|)$ per qualche costante $c > 0$. 

Essendo $f$ `time-constructible` allora esiste $T_f$ che scrive in unario $c f(n)$.

$NT'$ decide $L$, infatti:
* se $x \in L$ allora $NT(x)$ accetta in $c f(|x|)$ passi
* se $x \notin L$ allora $NT(x)$ rigetta in $cf(|x|)$ passi, oppure supera $cf(|x|)$ passi e rigetta ugualmente.

Ci vuole $O(c f(|x|))$ per calcolare $c f(|x|)$ essendo `time-constructible` e $cf(|x|)$ passi di $NT(|x|)$, dunque $L$ decidibile in $O(f(|n|))$.

> NOTA: A differenza del primo teorema, siamo in grado di dire che l'esecuzione termina in $O(f(|n|))$

### Teorema 6.17
Per ogni funzione `time-constructible` $f: \mathbb N \to \mathbb N, NTIME[f(n)] \subseteq DTIME[2^{O(f(n))}]$.

**Dimostrazione**: con $L \in NTIME[f(n)]$ allora esistono, $NT$ non deterministica che accetta $L$ e $h$ costante tali che:
$\forall x \in L: ntime(NT,x) \leq h f(|x|)$.

E' simile al teorema precedente, solo che la macchina deterministica $T$ deve simulare una per una le computazioni, e puo' farlo dato che conosce $h f(x)$. 

Allora basta contare quante computazioni ci sono, ossia, con $k$ grado di non determinismo di $NT$, $T$ esegue $h f(|x|) k^{h (f|x|)}$ dove semplificando le costanti e i termini, possiamo dire che $dtime(T,x) \in O(2^{f(|x|)})$

# classi di complessita
* $P = \cup_{k \in \mathbb N} DTIME[n^k]$. Tutti i linguaggi decisi in tempo polinomiale da macchine deterministiche.
* $NP = \cup_{k \in \mathbb N} NTIME[n^k]$. Tutti i linguaggi accettati in tempo polinomiale da macchine non deterministiche.
* $EXPTIME = \cup_{k \in \mathbb N} DTIME[2^{p(n,k)}]$
* $NEXPTIME = \cup_{k \in \mathbb N} NTIME[2^{p(n,k)}]$
* $FP = \cup_{k \in mathbb N} \{f: \Sigma_{1}^{*}\to \Sigma_{2}^{*}: \exists T \text{ che calcola f, per cui } \forall x \in \Sigma_{1}^{*}, dtime(x,T) \in O(n^k) \}$
> [!warning] $FP$
> Insieme di funzioni calcolabili da una macchina di turing deterministica in tempo polinomiale.

Possiamo dire che:
* $P \subseteq NP$, $PSPACE \subseteq NPSPACE$, $EXPTIME \subseteq NEXPTIME$ 
* $P \subseteq PSPACE$, $NSPACE \subseteq NPSPACE$
* ecc...

Ma si tratta di relazioni deboli:
* non sappiamo se $P = NP$ o $P \subset NP$.

Sappiamo pero' che:
* $P \subset EXPTIME$.
# riducibilita polinomiale
A parte $P \subset EXPTIME$ e $PSPACE = NPSPACE$ le altre sono tutte **inclusioni improprie**.

E di fatto non sappiamo se:
* esiste un linguaggio in $NP$ che non puo' essere deciso in tempo polinomiale? ossia che $P \subset NP$?
* non e' che tutti i linguaggi $EXPTIME$ sono in $PSPACE$?

> tutti i linguaggi in $P$ sono in $NP$, ma non sappiamo se vale il contrario.

> [!note] $L_{1} \leq L_{2}$
> $L_1$ e' riducibile a $L_2$ se esiste una funzione $f: \Sigma_{1}\to \Sigma_{2}$ $f$ e' totale e calcolabile e per cui $\forall x \in \Sigma_{1}^{*}[x \in L_{1} \iff f(x) \in L_2]$

Ora, sia $\pi$ un predicato ossia una proprietà che deve soddisfare $f$ che dimostra la riducibilità tra due linguaggi, ad esempio:
* $|f(x)| = |x|$

> [!note] $L_{1} \leq_{\pi} L_{2}$
> Ossia, $L_1$ $\pi \text{-riducibile}$ a $L_2$ se:
> * definita $\forall x \in \Sigma_{1}^*$
> * $\exists T_{f}$ tale che per ogni parola $x \in \Sigma_{1}^*$ la computazione di $T_f(x)$ termina con $f(x) \in \Sigma_{2}^*$ scritto in output
> * $f$ soddisfa $\pi$

Vogliamo usare sta robba per individuare i linguaggi separatori tra due classi di complessità, cosi possiamo dimostrare che siano differenti! (esiste un linguaggio in una classe che non sta nell'altra?).

> [!note] chiusura
> Una complessità $C$ e' chiusa ad una generica $\pi \text{-riduzione}$ se **per ogni coppia** di linguaggi $L_{1}$ ed $L_{2}$ tali che per $L_{1} \leq _{\pi} L_{2}$ ed $L_{2}\in C$ allora si ha che $L_{1} \in C$.


Dunque, una $\pi$ riduzione può' essere usata per dimostrare l'appartenenza di un linguaggio $L$ a $C$, quindi se $L \leq_{pi} L_0$ ed $L_{0}\in C$ allora anche $L \in C$.

> [!note] completezza di $L$ in $C$ rispetto a $\pi$ 
> $L$ e' $C$ **completo** rispetto alla riduzione $\pi$ se:
> * $L \in C$
> * $\forall L' \in C$ allora $L' \leq _{\pi} L$.

## supercazzola!

> [!note] linguaggio separatore
> $L$ e' linguaggio separatore se $L \in C_{2}- C_1$ (assumendo $C_{1} \subseteq C_{2}$), ossia dobbiamo dimostrare che $L \in C_1$ ma $\notin C_2$ o viceversa

E' difficile dimostrare che $L \notin C_{1}$: ossia che nessuna macchina di turing che rispetta la complessita di $C_1$ sia in grado di accettare o decidere $L$.

Possiamo trovare i linguaggi **candidati** ad essere separatori!

### Teorema 6.20
Siano $C$ e $C'$ due classi tali che $C' \subseteq C$, se $C'$ e' chiusa rispetto ad una $\pi\text{-produzione}$ allora, per ogni linguaggio $L$ che sia $C\text{-completo}$ rispetto a $\pi$, $L \in C' \iff C=C'$.

Se $C=C'$, allora $L\in C \iff L \in C'$.
Se $L \in C'$ essendo $L, C\text{-completo}$, per ogni $L'\in C, L' \leq_{\pi} L$. Essendo $C'$ chiuso 

boooo

## Riducibilità polinomiale

>[!note] Riducibilità polinomiale $L_{1} \leq_{p} L_2$.
> e' vero se $\exists f: \Sigma_{1}^{*}\to \Sigma_{2}^{*}$ tale che:
> * $f$ e' totale e calcolabile in tempo polinomiale ($f \in FP$) ossia: $\exists c: \forall x\in \Sigma_{1}^{*}, dtime(T,x) \in O(|x|^c)$.
> 
> * per ogni $x \in \Sigma_{1}^*$ vale che: $x \in L_{1}$ se e solo se $f(x) \in L_2$, $\forall x \in \Sigma_{1}^{*}[x \in L_{1}\iff f(x) \in L_{2}]$

> [!warning] attenzione!
> $L_{1} \leq L_{2}$ vuol dire che $L_1$ riducibile polinomialmente a $L_{2}$. 

### Teorema 6.21: $P$ chiuso rispetto a riducibilità polinomiale.
Se, con $L_{1}\subseteq \Sigma_{1}^{*}$ e $L_{2}\subseteq \Sigma_{2}^{*}$  ed $L_{1}\leq _{p}L_{2}$, e dimostriamo che esistono $T_r$ e $c$ costante che per ogni $x \in \Sigma_{1}^{*:}x \in L_{1} \iff f(x) \in L_{2}$ e $dtime(T_{r}, x) \in O(|x|^c)$ .

Se sappiamo che $L_{2}\in DTIME[f(n)]$, possiamo costruire $T_1$ che:
* **fase 1:** simula $T_r$
* **fase 2**: simula $T_{2}$ usano come input, l'output di $T_r$, se accetta allora $T_1$ accetta, altrimenti rigetta.

Dunque: $T_{1}$ decide $L_{1}$, ma in quanto tempo? $L_{1} \in DTIME[n^{c}+ f(n^c)]$ perché' $T_1$ termina in $O(|x|^{c}+ f(|x|^{c}))$ passi.

**Quindi**: se $L_{2}\in P$, allora anche $L_{1}\in P$. Abbiamo dimostrato che se $L_{1}\leq L_{2}$ e $L_{2}\in P$, allora $L_{1} \in P$ e in altre parole, la chiusura di $P$ rispetto alla riduzione polinomiale.

> Stessa cosa per $EXPTIME$

## NP-completezza
$L \subseteq \Sigma^*$ e' NP-completo se:
* $L \in NP$
* per ogni altro $L_{0}\in NP$ $L0 \leq L$.

Ossia sono i possibili linguaggi separatori tra $P$ e $NP$.

### corollario 6.4
se $P \neq NP$, allora ogni linguaggio $NP\text{-completo}$ $L \notin P$.

**dim.** Se fosse $L$ NP-completo in $P$ allora ogni $L_{0} \in NP$ e' riducibile ad $L$ ($L_{0}\leq L$). Ma se $L \in P$ poiché $P$ chiuso rispetto alla riduzione polinomiale allora, per ogni $L_{0}\in NP$, $L_{0} \in P$ e quindi $P = NP$.

> Vuol dire che, e' improbabile che un problema $NP$ completo sia anche in $P$, in quanto si sospetta che $P \neq NP$.


> Ricorda:
> * $L_{1}\leq L_{2}$ con $L_{2}$ accettabile/decidibile, allora lo e' anche $L_1$
> * $L_{1}\leq L_{2}$ con $L_{1}$ non accettabile o decidibile, allora non lo e' anche $L_2$.

# classi complemento
Nella definizione delle classi complemento non si dice nulla circa l'accettabilita o la decidibilita di questi, ci importa che il complemento di $L$ sia accettato o deciso.

### Teorema 6.11
Per ogni $f: \mathbb N \to \mathbb N$, $DTIME[f(n)] = coDTIME[f(n)]$ e $DSPACE[f(n)] = coDSPACE[f(n)]$. 

Infatti: con $T: dtime(T,x) \leq f(|x|)$, posso costruire $T'$ invertendo gli stati di accettazione di $T$, in modo che $T'$ decide $L^c$.

> **Importante**: da qui, segue che $P=coP$, ma e' vero anche per $NP$ e $coNP$?

Possiamo applicare lo stesso procedimento per $NTIME$ e $coNTIME$?

## $NP = coNP$?
Ricordiamo che: $NT$ accetta se esiste una computazione deterministica che termina in $q_A$, mentre rigetta se tutte terminano in $q_R$.

con $L^{C}= \{0,1\}* - L$ e che:
* se $x \in L$, allora $x \notin L^c$
* se $x \notin L$, allora $x \in L^c$.

Allora $NT^C$ accetta $L^C$ se, per ogni $x \in \{0,1\}^*$:
* se $x \in L$, allora $NT^C(x)$ non accetta, quindi ogni computazione deterministica di $NT^C$ non termina in $q_A$
* se $x \notin L$, allora accetta ossia, esiste una computazione che termina in $q_A$

Ora, scegliamo $L \subseteq \{0,1\}^*$ accettato da $NT$, e costruiamo $NT_1$ che accetta $L$ ma con la quintupla $<q_{0}, s, s, q_{R}, F>$ per ogni $s \in \{0,1,\square\}$: *in questo modo, esiste sempre una computazione che rigetta*!

Dunque, $NT_1$ accetta $L$, dato che se $x \in L$, in $NT$ esiste una computazione che accetta e questa computazione a un certo punto compare in $NT_1$.

Stesso discorso se $x \notin L$, nessuna computazione deterministica termina in $q_A$.

Ora costruiamo $NT_{1}^C$ che accetta $L^C$... 
* se $x \in L^C$, allora in $NT_{1}$ esiste la computazione che termina in $q_R$ e quindi accetto.
* se $x \notin L^C$, in $NT_1$ troviamo la computazione che termina in $q_R$, e quindi $NT_{1}^C$ accetta. diocane!!!!
Quindi, $NT_{1}^C$ accetta tutti gli input, questo per via dell'asimmetria tra accettazione e rigetto nel modello delle macchine non deterministiche.

> Non possiamo concludere che $coNP = NP$, ma nemmeno che $coNP \neq NP$. 

Si pensa che:
* $P \neq NP$
* $coNP \neq NP$

> **Nota**: Se $P =NP$, allora $NP=coNP$. Infatti $P = coP = NP = coNP$ (per definizione).


> [!note] $L$ coNP completo.
> $L$ e' tale se:
> * $L \in coNP$
> * $\forall L' \in coNP, L' \leq L$.

> i linguaggi $NP\text{-completi}$ sono in un certo senso quelli piu difficili da accettare in $NP$ e si usano per cercare di dimostrare che $P \neq NP$.

> Si puo' fare la stessa cosa per provare che $coNP \neq NP$.

### Teorema 6.25

### Teorema 6.26
























