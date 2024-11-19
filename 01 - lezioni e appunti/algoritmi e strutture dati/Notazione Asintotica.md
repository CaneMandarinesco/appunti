# modelli di calcolo
Per poter fare delle stime sulla complessità temporale bisogna introdurre un modello di calcolo appropriato. Noi useremo la macchina a registri, che viene storicamente dopo il modello della macchina di Turing.
## macchina di Turing
Il primo modello storico introdotto e' la macchina di **Turing**:
* un nastro infinito bidirezionale rappresenta la **memoria**
* il nastro e' diviso in **celle**
* ciascuna cella puo' contenere un simbolo, preso da una alfabeto finito.
* l'accesso al nastro avviene tramite una **testina**
* ogni passo di calcolo permette alla testina di: 
	* spostarsi di una cellla
	* cambiare stato di controllos
	* scrivere o leggere
## macchina a registri (astrazione di von Neumann)
Questo modello e' di livello troppo basso, usiamo un metodo piu adeguato che si ispira alla macchina di **von neumann**, o **macchina a registri**. e' costituita da  
* **programma** finito
* **nastro** in entrata e in uscita
* **memoria** strutturata come **array** di `n` parole, con un indirizzo da 1 a `n`
* le parole possono essere un qualsiasi **intero** o un qualsiasi numero **reale**.
* **accumulatore**: contiene l'operando 
* **program counter**
* ogni istruzione permette una di queste **operazioni elementari**
	* leggere o stampare
	* una operazione aritmetico/logica
	* accesso o modifica di un elemento in memoria
## Criteri di costo uniforme e logaritmico
A seconda del calcolatore, e' ragionevole pensare che il costo computazionale di una lettura/scrittura in memoria sia proporzionale alla grandezza del dato che stiamo manipolando: magari il mio calcolatore ha bisogno di 2 operazioni "atomiche" di scrittura per modificare una variabile di 128 bit. 
Introduciamo quindi due criteri di costo:
* **uniforme**: il costo computazionale di un'operazione e' sempre costante. anche se devo computare 1,2,100 o miliardi di cifre
* **logaritmico**: il costo computazionale dipende dalla grandezza dei dati. la grandezza dei dati influenza il tempo di esecuzione in `log(n)`
tuttavia, useremo quasi sempre il costo **uniforme**.

## Caso peggiore e caso medio
A parità di dimensione dell'istanza: $n$, il tempo di esecuzione dell'algoritmo potrebbe variare. (es: se su $n$ monete devo cercare quella falsa, potrei trovarla subito o potrei trovarla all'`n-esimo` passo). 

Introduco quindi una formula che rappresenta il tempo peggiore di esecuzione.

 > [!note] caso peggiore
 > $$T_{\text{worst}}(n) = \max \limits_{\text{ istanze I di dimensione n}} \{  tempo(I) \}$$
 > Ossia il tempo di esecuzione dell'istanza peggiore, mi garantisce che tutte le altre vengano eseguite in tempo $\leq T_{worst}(n)$, rispetto a $n$!

Abbiamo anche una formula per il tempo medio di esecuzione.

 >[!note] caso medio
 >$$T_{\text{avg}}(n) = \sum \limits_{\text{ istanze di I di dimensione n}} \{  P\{ I \} \cdot {tempo}(I) \}$$
 > per conoscerlo potrei  dover fare delle assunzioni, dato che non sempre si può calcolare.
 
 
# notazione asintotica
Abbiamo già detto che e' un metodo che ci semplifica l'analisi del costo computazionale di un'algoritmo, ci permette di fare un'analisi **qualitativa** di $T(n)$. Quando usiamo questo tipo di notazione, si assume che vogliamo studiare $T(n)$ per valori **molto grandi** (miliardi, bilioni, trilioni, ecc...).

## definizioni
Usiamo 5 simboli: $O, \Omega, \Theta$ (rispettivamente, O-grande, Omega e Theta) e $o \text{ e } \omega$ (o-piccolo, omega-piccolo).
* $O(f(n)) = \{  g(n): \exists c>0 \text{ e } n_{0}\geq 0\text{ t.c. } g(n) \leq cf(n) \text{ per ogni } n\geq n_{0}  \}$
* $\Omega(f(n)) = \{ g(n): \exists c>0 \text{ e } n_{0} \geq 0 \text{ t.c. } g(n)\geq c f(n) \text{ per ogni } n\geq n_{0} \}$
* $\Theta(f (n)) = \{ g(n): \exists c_{1},c_{2} > 0 \text{ e } n_{0} \geq 0 \text{ t.c, per ogni } n\geq n_{0}, c_{1}f(n)\leq g(n) \leq c_{2} f(n) \}$
	* se $\lim_{ n \to \infty } \frac{f(n)}{g(n)} = c>0$
* $o(g(n)) = \{ f(n): \forall c > 0, \exists n_{0} \text{ t.c } \forall n \geq n_{0}, \,\,0 \leq f(n) < c \cdot g(n) \}$
	* oss: $o(g(n)) \subset O(g(n))$
	* $f(n) = o(g(n)) \iff \lim_{ n \to \infty } \frac{f(n)}{g(n)} = 0$

* $\omega (g(n)) = \{ f(n): \forall c>0, \exists n_{0} \text{ t.c. } \forall n \geq n_{0}, 0 \leq c \cdot g(n) < f(n)\}$
	* oss.: $\omega (g(n)) \subset \Omega(g(n))$
	* * $f(n) = \omega(g(n)) \to \lim_{ n \to \infty } \frac{f(n)}{g(n)} = \infty$

In italiano vuol dire che, ogni simbolo ha un significato logico:
* se $g(n) \in O(f(n))$ allora, $g(n)$ cresce **al più** come $f(n)$, **upper bound**
* se $g(n) \in \Omega(f(n))$ allora, $g(n)$ cresce **almeno** come $f(n)$, **lower bound**
* se $g(n) \in \Theta(f(n))$ allora, $g(n)$ cresce **esattamente** come $f(n)$

* se $g(n) \in o(f(n))$ allora, $g(n)$ cresce **strettamente** meno di $f(n)$
* se $g(n) \in \omega(f(n))$ allora, $g(n)$ cresce **strettamente** piu' di $f(n)$

o ancora più brevemente:

| notaz.                | simbolo logico   |
| --------------------- | ---------------- |
| $g(n) = O(f(n))$      | $g(n) \leq f(n)$ |
| $g(n) = \Omega(f(n))$ | $g(n)\geq f(n)$  |
| $g(n) = \Theta(f(n))$ | $g(n)= f(n)$     |
| $g(n) = o(f(n))$      | $g(n)< f(n)$     |
| $g(n) = \omega(f(n))$ | $g(n)> f(n)$     |
$O$ e $\Omega$ esprimono rispettivamente una **delimitazione superiore o inferiore** del nostro algoritmo. La prima ($O$) ci assicura che il nostro algoritmo esegue al **massimo** in tempo proporzionale a $f(n)$, la seconda ($\Omega$) che il nostro algoritmo esegue **almeno** in tempo proporzionale a $f(n)$.

 > notazione appropriata:
> - $2n^2 + 4 = O(n^3)$ non e' matematicamente corretta, anche se tutta via scriveremo cosi!
> - $2n^2 + 4 \in O(n^3)$ e' corretto

## analisi di Fibonacci 3
```
algoritmo fib3(n)
1   sia Fib un array di n interi
2  	Fib[1] <- Fib[2] <- 1
3	for i = 3 to n do:
4		Fib[i] <- ...
5	return Fib[n]
```

* eseguo 3 linee, una sola volta
* le linee 3 e 4 vengono eseguite al più $n$ volte
* quindi: $T(n) = c_{1}+c_{2}+c_{5} + (c_{3}+c_{4})n = \Theta(n)$
	* ossia $T(n)$ e' esattamente proporzionale ad $n$
	* il termine $c_{i}$ indica il numero di passi elementari eseguiti sulla RAM alla $i$-esima riga.

## perche' e' figa la notazione asintotica?
* nasconde i dettagli irrilevanti del codice: ossia le istruzioni poco rilevanti, quando $n$ e' enorme.
* e' il metodo piu' realistico da applicare: come puo' un programmatore conoscere nel dettaglio quante istruzioni impiega una determinata procedura? e' meglio ignorare questi dettagli.
* ci da un valore che e' indipendente dalla macchina che usiamo.

