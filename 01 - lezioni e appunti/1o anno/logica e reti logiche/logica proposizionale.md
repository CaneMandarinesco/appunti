> nota: date due variabili esistono in totale $2^4 = 16$ funzioni diverse. 

### formule ben formate
Le lettere proposizionali sono formule ben formate (f.b.f)
* se $\mathcal F$ e' una $\text{ f.b.f. }$ allora anche $\mathcal F$ lo e'.
* se $\mathcal F \text{ e } \mathcal G$ sono $\text{f.b.f.}$ allora anche $\mathcal F \circ \mathcal G$ lo e' (dove con $\circ$ indico uno dei qualsiasi connettivi)
* ***Nient'altro*** e' una $\text{f.b.f.}$

###  tautologie, contraddizioni, contingenze
Data una formula $\mathcal F$ le si puo assegnare un interpretazione: $\tau$, ovvero l'assegnazione di valori ($T \text{ o } F$) alle variabili della formula.
$\mathcal F$ puo' essere quindi: 
* **tautologia** se per ogni interpretazione, $\mathcal F$ e' $T$
* **contraddizioni** se $\mathcal F$ e' sempre $T$
* **contingenza** se e' un po e un po.
### costanti
$t \text{ e } f$ sono due variabili costanti che valgono rispettivamente $T \text{ e } F$.
* $p \land t$ equivale a $p$
* $p \land f$ equivale a $f$
* $f \lor t$ equivale a $f$

### indipendenza dei connettivi
* $p \to q$ equivale a $\neg p \lor q$
* $p \land q$ equivale a $\neg(\neg p \lor \neg q)$
* $p \equiv q$ equivale a $(p\to q) \land (q \to p)$ oppure a $(p \land q) \lor (\neg p \land \neg q)$


> [!note] joint denial (NOR)
> $p \downarrow q$ e' $T$ se e solo se $p = F \text{ e } q=F$
> * $p \downarrow p$ e' equivalente a $\neg p$


### Metodo dei tableaux
Sia $\mathcal F$ una proposizione:
* consideriamo $\neg \mathcal F$, possiamo dire che $\mathcal F$ **non e' una tautologia** se e solo se esiste un'interpretazione di $\mathcal F$ che rende $T$ la formula $\neg \mathcal F$
* vado a considerare in $\neg \mathcal F$ le sue parti necessarie a rendere $T$ la formula, preservando la soddisfacibilita' di $\neg \mathcal F$ .
* man mano che procedo si costruisce un albero.
* ogni formula corrisponde si puo semplificare in una $\alpha \text{-formula}$ (AND) o in una $\beta \text{-formula}$ (OR).

![[Pasted image 20240812154417.png]]

### insiemi di hintikka
$S$ insieme di formule, e' detto insieme di Hintikka se:
* $H_{0}:$ $S$ non contiene ne $p$, ne $\neg p$
* $H_{1}:$ se $S$ contiene  una $\alpha \text{-formula}$ allora $S$ contiene anche **entrambe** le sue componenti $\alpha_{1} \text{ e } \alpha_{2}$
* $H_{2}:$ se $S$ contiene una $\beta \text{-formula}$ allora $S$ contiene anche **almeno una** delle sue componenti $\beta_{1} \text{ o } \beta_{2}$.

### teorema completezza
Se $\mathcal F$ e' tautologia allora e' dimostrabile con il teorema dei tableaux.