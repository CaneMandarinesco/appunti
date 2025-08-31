> [!note] $\mathfrak T$
> $\mathfrak T$ insieme di istanze di un problema. Es: $\mathfrak T = \mathbb N$. Ossia dato un insieme di oggetti conosciuti che costituisce l'**istanza**, vogliamo ricavare l'insieme delle soluzioni possibili, ossia oggetti che soddisfano dei vincoli.

Tipi di problemi:
* ottimizzazione: ottenere l'oggetto migliore.
* ricerca: elencare un'oggetto preso da una soluzione.
* enumerazione: elencare tutti gli oggetti che hanno la stessa proprieta.
* decisione: si o no?

> [!note] Problema: e' una tripla!
> 
> E' dunque una quintupla: $<I, R, S, \mathcal n, \rho>$ dove:
> * $S$: insieme delle **soluzioni possibili**
> * $\mathcal n: S(I) \to 2^{S(I)}$ ossia insieme delle **soluzioni effettive**
> * $\rho: I \times \mathcal n(S(I)) \to R$ e' la **richiesta** del problema.
> 
> Di conseguenza, la **risposta** ad un'istanza $x \in I$ e' $\rho(x, \mathcal n(S(x)))$.

### es. 7.1
se abbiamo un certo insieme di contenitori e una bilancia, e di questi contenitori conosciamo il peso di acqua che puo' tenere quando pieno fino all'orlo.

E vogliamo trovare magari il contenitore che porta esattamente un litro.

**Formalizziamo**: l'istanza del problema $C \in I$ e' un'insieme $C = \{c_{1}, \dots, c_{n}\}$ tale che $c_{i} \in \mathbb R^+$. $S(C)$ *coincide* con $C$, dunque $S(C) = C$.

Allora $n(S(C)) = \{c \in S(C) : c \geq 1\}$ ed $\rho$ e' definita come segue:
* voglio trovare un contenitore che contiene almeno un litro d'acqua: $R = C$ e $\rho(C, \mathcal n(S(C)))$ e' un elemento di $\mathcal n(S(C))$
* Se **esiste** un contenitore che può contenere un litro di acqua? $R = \{\text{vero, falso}\}$ e $p(C, \mathcal n(S(C))) = [\mathcal n(S(C)) \neq 0]$
* Quanti sono i contenitori che possono contenere almeno un litro di acqua? $R = \mathbb N$ e $p(C, \mathcal n(S(C)))=|n(S(C))|$.

## Problemi decisionali
La funzione $\rho$ in questi casi e' booleana, e la definizione di problema puo' essere semplificata nella tripla $<I,S,\pi>:$ $I$ insieme di istanze, $S$ soluzioni possibili per istanza $I$, $\pi$ predicato e chiamiamo la tripla con $\Gamma$. 

Possiamo partizionare le istanze di $\Gamma$ in:
* $Y_{\Gamma} = \{x \in I_{\Gamma} : \pi_{\Gamma}(x) = \text{ vero }\}$
* $N_{\Gamma} = \{x \in I_{\Gamma} : \pi_{\Gamma}(x) = \text{ falso }\}$
### Es: `SODDISFACIBILITA'` 
$$f: \{\text{vero, falso}\}^{n}\to \{\text{vero, falso}\}$$
Sia $f$ la funzione che combina $n$ variabili booleane mediante gli operatori logici: $\land, \lor, \lnot$. $f$ e' in forma congiunta normale se e' una congiunzione di clausole di al piu $n$ variabili distinte, dove queste sono congiunte da $\land$.

Si puo' formulare nel seguente modo:
* $I_{SAT} = \{<f,X> \text{ tale che } f \text{ in forma congiuntiva normale }\}$, insieme di funzioni e di assegnazioni.
* $S_{SAT} = \{a: X \to \{\text{vero, falso}\}\}$, ossia insieme di assegnazioni di variabili
* $\pi_{SAT}(f, S_{SAT}(f,X)) = \exists a \in S_{SAT}(f,X) : f(a(x_1), ..., a(x_{n))}= \text{vero}$. Ne esiste una che la rende vera?

**Vediamo la codifica $\mathcal X_{1}$ per il problema**: sia $\Sigma = \{0,1\}$, allora ogni $x_i$ e' codificato da una sequenza di $n$ simboli in $\Sigma$, dove l'unico simbolo $1$ e' in $i\text{-esima}$ tale che $i$ rappresenta l'indice $x_i$ del simbolo rappresentato.

> se il simbolo $x_i$ e' preceduto da $\lnot$ allora ci sara $1$ a precedere il simbolo, altrimenti $0$.
* 
Per sapere di quante variabili e' costituita l'istanza possiamo fare cosi: la codifica inizia con tanti 1 quante sono le variabili, poi uno 0.

**Vediamo la codifica $\mathcal X_{2}$ per il problema**: sempre con $\Sigma = \{0,1\}$, sappiamo che $f$ e' rappresentata dalla tabella di verita!

Ora l'algoritmo che risolve e' tipo cosi: 
* $n = |X|$
* per ogni assegnazione di verita $a$ delle $n$ variabili, verifica se $f(a(X)) = \text{ vero }$. 

> Con $\mathcal X_{1}$ risolvere il problema e' difficile: devo generare tutte le combinazioni di $a$ possibili, quindi $dtime = O(n)$.

> Con $\mathcal X_{2}$ invece, devo solo scandire una volta l'input, che e' la tabella di verità stessa: $dtime = |\mathcal X_2(X,f)|$

Allora il problema e' risolvibile in tempo polinomiale?
* $\mathcal X_1$ rappresenta solo l'informazione esplicita, e strettamente necessaria, la struttura di $f$
* $\mathcal X_{2}$ e' una rappresentazione piu estesa, e' la tavola della verita che descrive $f$ per intera! Raccoglie in se, implicitamente l'informazione che voglio ottenere dal problema.

> $\mathcal X_{2}$ e' irragionevolmente lunga, poiche $\mathcal X_1$ rappresenta la stessa informazione con meno caratteri.

> [!note] codifica ragionevole
> $\mathcal X$ e' ragionevole se per ogni altra codifica $\mathcal X'$ per le istanze $I_{\Gamma}$ esiste $k \in \mathbb N$ tale che: $$ |\mathcal X(x)| \in O(|\mathcal X'(x)|^k)$$ con $x\in I$.

Analogamente, $\mathcal X$ non e' ragionevole se esiste un'altra codifica con notazione asintotica migliore.

## complessita dei problemi decisionali
Con $\Gamma = <\mathfrak T, S, \pi>$ un problema decisionale allora l'insieme delle istanze $\mathfrak T$ e' partizionato in:
* istanze si: verificano $\pi$
* istanze no: non verificano $\pi$.
Con $\mathcal X: \mathfrak T \to \Sigma^*$ una codifica ragionevole per il problema $\Gamma$, allora questa e' partizionata in:
* $Y_{\Gamma}$: parole che codificano istanze si
* $N_{\Gamma}$: parole che codificano istanze no
* parole che non codificano istanze di $\Gamma$.

> [!note] Linguaggio associato a $\mathcal X$ in $\Gamma$
> $$
> L_{\Gamma}(\mathcal X) = \{x \in \Sigma^{*}\exists y \in \mathfrak [x = \mathcal X(y) \land \pi(y, S(y))]\}$$
> Ossia, l'insieme delle parole codificate accettate, oppure.
> $$
> L_{\Gamma}(\mathcal X) = \{x \in \Sigma^{*}: x \in \mathcal X(I_{\Gamma}) \land \pi_{\Gamma}(\mathcal X^{-1}(I_{\Gamma}), S(\mathcal X^{-1}(I_{\Gamma})))\}
> $$

Per vedere se $y$ e' una istanza si, occorre verificare se esiste $x$ codifica di $y$ che appartiene a $L_{\Gamma}(\mathcal X)$.

Per vedere se $x$ e' codifica di un'istanza si, bisogna vedere se esiste $y$ che codificato mi da $x$ che verifica il predicato $\pi(y, S(y))$.

> [!note] Codifica Ragionevole per $\Gamma$
> Con $\Gamma$ problema e $C \in \{DTIME, DSPACE, NTIME, NSPACE\}$ (di $f$ funzione totale e calcolabile) allora $\Gamma \in C$ se esiste una **codifica ragionevole** $\mathcal X: \mathfrak T \to \Sigma^*$ per $\Gamma$ tale che $L_{\Gamma}(\mathcal X) \in C$

> Dunque, *un problema sta in una certa classe di complessita se esiste una codifica per quel problema, per cui il suo linguaggio sta in quella classe.*
### es: 7.6
Usando la codifica $\mathcal X_{1}$ per $3SAT$, allora una parola $x \in \{0,1,2,3,4\}^*$ (ossia i simboli usati per la codifica!) e' in $L_{3SAT}(X_{1})$ se:
* $x$ e' la codifica secondo $\mathcal X_{1}$ di qualche $<X, f>$
* $x$ che codifica $f$ e' soddisfacibile.

### decidere le istanze

> **Nota**: Bisogna verificare che $x$ codifichi correttamente un'istanza! ossia che $\mathcal X (\mathfrak T)$ sia **decidibile**!

> [!note] Linguaggio delle istanze di $\Gamma$
> $$
> \mathcal X(\mathfrak T) = \{x \in \Sigma^{*} : \exists y \in \mathfrak [x = \mathcal X(y)\}
> $$

> **osservazione**: $\mathcal X$ e' invertibile.

### es: 7.7 $PHC$
Con $PHC$ il problema del percorso in ciclo Hamiltoniano, vogliamo sapere se dato un ciclo Hamiltoniano in $G$ e due nodi $u,v$ questi due sono connessi da un percorso.
* $\mathfrak \{<G =(V,E), u, v>: G \text{ e' un grafo non orientato } \land \exists \text{c ciclo che passa per una e una sola volta in ogni nodo del grafo } \land u,v \in V\}$
* $S_{PHC} = \{p: \text{ p e' un percorso in G} \}$
* $\pi = \exists p \in S_{PHC} \text{ che connette } u \text { a } v$.
Ma ogni istanza di $PHC$ e' un'istanza **si**! Ma bisogna quantomeno decidere se scegliendo la codifica ragionevole $\mathcal X$, se $x \in \mathcal X(\mathfrak T)$.

> Per $PHC$, decidere se $x \in \mathcal X(\mathfrak T)$ e' un problema $\text{NP-completo}$. Dunque, nonostante sia un problema stupido, e' $\text{NP-completo}$.

> [!note] **Asimmetria** del linguaggio complemento
> Con $\Gamma ^{c}= <I_{\Gamma}, S_{\Gamma}, \lnot \pi_{\Gamma}>$
> $$
> L_{\Gamma^{c}}(\mathcal X)= \{x \in \Sigma^{*}: \exists y \in I_{\Gamma}[x = \mathcal X(y) \land \lnot \pi (y_\Gamma, S_{\Gamma}(y))]\}
> $$
> che e' differente da
> $$
> L^{c}_{\gamma}(\mathcal X) = \{x \in \Sigma^{*}: x \notin \mathcal X(I_{\Gamma}) \lor \lnot \pi_{\Gamma}(\dots)\}
> $$

> [!warning] attenzione all'asimmetria
> la prima definizione riguarda le parole codificate che non verificano $\pi$, la *seconda riguarda anche le parole che non appartengono a quella codifica*!

Possiamo quindi ricavare il complemento di $PHC$, ossia: decidere se dati due nodi $u,v$, se questi non sono connessi da un path in $G$, sapendo che in $G$ c'e' un ciclo  hamiltoniano.

> **Nota**: codifcare $PHC^C$ e' un problema NP-completo.

### Teorema 7.1
con $\Gamma = <...>$ un problema decisionale e $\mathcal X: \mathfrak T \to \Sigma^*$ una codifica ragionevole, se $\mathcal X(\mathfrak T) \in P$ allora:
1. se $L(\mathcal X) \in NP$ allora il linguaggio complemento sta in $coNP$.
2. se $L(\mathcal X) \in NEXPTIME$ allora il linguaggio complemento sta in $coNEXPTIME$.

#### dim. 1
Dato che $\mathcal X(\mathfrak T) \in P$, allora esiste $T$ deterministica e $h$ costante per cui $T$ decide se $x \in X(I)$ con $dtime(T,x) \in O(|x|^h)$.

Se $L(\mathcal X)$ sta in $NP$, allora esiste $NT$ e un intero $k$ per cui $ntime(NT, x) \in O(|x|^k)$. Combinando $T$ e $NT$ possiamo derivare $NT'$ che:
* simula $T(x)$ che decide la codifica.
* simula $NT(x)$

Dunque $NT'$ accetta se $x$ non e' una codifica oppure se 

BOH????

# classe NP
Allora, se un problema sta in $P$ e' perche esiste un'algoritmo veloce che lo risolve. 

In $NP$ si trovano tanti problemi che hanno rilevanza pratica, che non si riescono a risolvere con *algoritmi deterministici polinomiali*.

Perche la classe $NP$ e' definita come i linguaggi accettati da $NT$? Perche' se una computazione di $NT$ consigliata dal genio rigetta, potrebbe essere perche' o una computazione accettante non esiste, o perche' il genio si e' sbagliato.

> Il genio e' **affidabile** quando ci consiglia una computazione accetta, ma non lo e' quando rigetta!


### $3SAT$
Con:
* $\mathfrak T = \{<X,f>: X \text{ insieme di variabili booleane } \land f \text{ e' predicato su X in 3CNF}\}$
* $S(X,f) = \{a: X \to \{vero, falso\}\}$
* $\pi(X,f, S(X,f)) = \exists a \in S: f(a(x)) = \text{vero}$.
Un'algoritmo che accetta 3SAT potrebbe essere:
```
i = 1
while (i <= n) do begin
	scegli a(x_i) nell'insieme {vero, falso};
	i+=1
end

while (i <= n) do begin
	sostituisci in f ogni occorrenza di x_i con a(x_i) e not x_i con not a(x_i)
end
if (f(a(X)) = vero) allora q = q_A;
else q = q_R
```

* La prima parte: sceglie un'assegnazione di verità $a$ per le $n$ variabili. *E' la parte non deterministica, dove chiedo al genio di scegliere la giusta assegnazione di verita*.
* La seconda parte: sostituisce gli $x_i$ con $a(x_i)$.
* L'ultima parte: verifica $f(a(X))$

E l'esecuzione richiede tempo non determinstico: $O(|X| |f|)$, quindi il problema e' in $NP$

> **Nota**: l'operazione `scegli` in `PascalMinimo` chiede l'intervento del genio per effettuare una scelta.

### `CLIQUE`
Il problema consiste nel decidere, dato $G(V,E)$ ed $k \in \mathbb N$, trovare se esiste un sotto-grafo completo di almeno $k$ nodi.

* $\mathfrak T = {<G(V,E), k> : k \in \mathbb N}$
* $S(G(V,E), k) = V' \subseteq V$
* $\pi(G(V,E), k, S) = V' \in S t.c. \text{ completo } \land |V'| \geq k$, dove completo vuol dire che **ogni coppia** di nodi ha un'arco.

L'algoritmo che lo risolve e' simile a `3-SAT`:
* scegli un'insieme di nodi da verificare, chiesti al genio burlone. (parte non deterministica dell'algoritmo)
* verifica che questi siano tutti collegati tra di loro: ossia $V$ scelto e' completo. **verifica il predicato**

### `LONG PATH`
Dato un grafo $G$ e due nodi, e un $k \in \mathbb N$ voglio sapere se esiste un percorso fra $a$ e $b$ di esattamente $k$ archi.

Come potrei strutturare la parte non deterministica? Chiedo al genio di scegliere un nodo tra $V$?

> [!warning] attenzione fra
> Non va bene l'algoritmo come formulato sopra! $|V|$ non e' costante e quindi **non e' non deterministico**


## osservazione incredibile
Il predicato nei 3 problemi visti ha sempre una struttura simile: ci chiediamo se esiste un'oggetto che soddisfa certe proprieta, e l'algoritmo e' diviso in due molto simili tra i 3 problemi.

> [!note] Problema in NP
> Un problema e' in $NP$ se:
> * $\pi(x, S(x)) = \exists y \in S(x) \text{ t.c. } n(x,y)$
> * $n(x,y): \text{ verifica se una certa proprieta e' soddisfatta}$
> * $y$ viene scelto in $|x|$ tempo polinomiale
> * $n(x,y)$ viene verificato in $|x|$ tempo polinomiale.

> Ma non e' che esistono problemi in $NP$ **che non soddisfano queste 3 osservazioni fatte**? Allora: **no**! diocane!

### Teorema 9.1
Con $L \subseteq \Sigma^*$, appartiene a $NP$ **se e soltanto se**:
* esiste $T$, $h,k \in \mathbb N$ tali che, $\forall x \in \Sigma^*$

$$x \in L \iff \exists y_{x}\in \{0,1\}^{*}: |y_{x}| \leq |x|^{k} \land T(x, y_{x}) \text{ accetta } \land dtime(T,x,y_{x})\in O(|x|^n)$$

**Che cazzo vuol dire**? 
* vogliamo trovare una parola $y_x$, da **associare** a $x$, che non sia troppo lunga: $|y_{x|}\leq |x|^h$.
* La macchina $T$ accetta, ed in tempo breve, ossia $O(|x|^h)$.

Ossia, il teorema ci dice che se do in input a $T$ le parole $x,y_x$ con $y_X$ **da me scelta** e non troppo lunga, $T(x,y)$ accetta in tempo polinomiale in $|x|$ e accetta se e soltanto se $x \in L$ e $y$ e' **giusta**.

Pero' devo avere qualcuno che mi suggerisce la $y_x$ giusta da dare a $T$ per farla accettare, se $y_x$ e' sbagliata, allora rigetta.

$y_x$ potrebbe essere una sequenza di quintuple che permette a $NT(x)$ di accettare, ma dato che $y_{x}$ mi viene suggerita dal genio di cui non mi fido, la devo **verificare**! 

> **Nota**: praticamente $y_x$ e' un **certificato** che viene suggerito da un genio.E questo certificato puo' essere verificato in tempo polinomiale, ossia se $y_x$ e' una computazione  accettante di $NT$.

Quindi, devo verificare che:
* devo verificare che $y_x$ e' una sequenza di quintuple in $NT$.
* che $y_x$ e' una sequenza di quintuple valide per $x$, o meglio che rappresenti una computazione di $NT(x)$.
* $y_x$ e' una computazione accettante?

Con $T(x, y_{x})$ la macchina **verificatore**, quanto cazzo impiega?
* $L \in NP$, e quindi se $x \in L$ esiste una computazione deterministica che termina in $|x|^k$ passi, quindi $ntime(NT,z) \leq |z|^k$. 
* $|y_{x}|\leq |x|^k$
* per verificare che $y_x$ e' una sequenza di quintuple in $NT$, impiego $O(|y_x|)$ passi, ossia $O(|x|^{k} |y_{x| )}\in O(|x|^{2k})$ 

#### ricapitolando: dim $\to$
Se $L \in NP$ e' accettato da $NT$ che impiega al massimo $|x|^k$ istruzioni, allora posso costruire $T$ verificatore che accetta in tempo polinomiale in $|x|$, e può verificarlo solo se $x \in L$.

> *Quindi abbiamo dimostrato che se $L \in NP$, allora esiste la macchina che certifica in tempo polinomiale.*

> Il genio ci comunica $x, y_x$, ossia la parola che crede sia accettata e la computazione che dovrebbe accettare.

### dim $\leftarrow$.
Dobbiamo costruire $NT$ usando $L$ e $T$ **verificatore**.

* FASE 1: $NT$, con input $x$, sceglie non deterministicamente una parola $y$ di lunghezza $|y| \leq |x|^k$
* FASE 2: $NT$ invoca $T(x,y)$ che accetta in $O(|x|^h)$ passi.

>**Per la fase 1**: Conoscendo $|x|^k$, calcolato in unario, posso chiedere al genio di scegliere $y[i] \in \{0,1\}$ (gli chiedo di generare la parola $y$ in binario).

> **Per la fase 2**,  con input $x$ e $y$ posso calcolare la lunghezza della computazione usando $T_g(|x|)$ e vedo se accetto entro quel numero di istruzioni.

$NT$ accetta $L$, se $x \in L$, allora $\exists y$ che certifica in tempo $O(|x|^h)$, ossia una sequenza di scelte, consigliate dal genio nella fase 1 che permette alla fase 2 di accettare.

Per quanto riguarda il tempo:
* fase 1: $O(|x|^k)$ operazioni per calcolare il certificatore.
* la fase 2: $O(|x|^h)$ operazioni calcolate da $T_g$, per ogni operazione devo eseguire un numero costante di istruzioni.

### quindi
> [!note] $\Gamma \in NP$
> $\Gamma \in NP$ se e solo se le istanze si ammettono certificati **verificabili in tempo polinomiale**.


> **Nota**: Possiamo usare come certificato per l'istanza $x$ di $\Gamma$: $y \in S(x)$