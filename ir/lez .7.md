**context sensitive error spelling fixing**: genero un *candidate set* che contiene
* la parola stessa
* **edit dist = 1**parole con edit distance pari a 1
* il set viene calato nel **contesto**

**Insiemi candidati**: sia la sentence costituita da $x_{1},\dots,x_{n}$, per ogni $x_{i}$ genero $\text{Candidate}(x_{i}) = \{ x_i, w_{1}^i, w_{2}^i, \dots , w_{k}^i \}$

**Come scelgo la sequenza $W$ che massimizza** $P(W|x_{1},\dots,x_{n})$? uso un **language model**.
* **bagof**: voglio semplificare questo calcolo cambiando lo stimatore.
* **stimatore**: prendire la prossima parola e vedere se e' corretta.
* **stimatore markoviano**: metto un limite alla storia per guardare le parole da utilizzare per stimare la parola successiva.
* $P(w_{1}, \dots, w_{n})=P(w_{1})P(w_{2}|w_{1})\dots P(w_{n}|w_{n-1})$.

**smoothing**: devo includere fenomeni che potrei non aver visto
![[Pasted image 20260327120249.png]]
* $\lambda$ va scelto tramite esperimenti.
* **log**: vogliamo lavorare con i logaritmi, dato che abbiamo tanti prodotti di probabilità nella formula dello smoothing.
 ![[Pasted image 20260327120750.png]]

**hidden markov model**: serve a scoprire informazioni nascoste
* **osservazioni**: i simboli che vedo
* **hidden**: i simboli che devo scoprire, candidate ad essere sostituzioni corrette per lo spelling correction.
* **probabilita di osservazione**
* **probabilita di transizione**

![[Pasted image 20260327121733.png]]

**semplificazione**: do un limite a quanti errori (1 o 2) potrei aver fatto, altrimenti l'**algoritmo di viterbi** potrebbe cambiare tutte le parole.

**language model**: usa statistica di unigrammi, bigrammi e costo delle sostituzioni per tirare fuori le parole giuste.

**problema**: *se la mia parola e' corretta ma molto rara*?
* introduco la probabilita che l'utente sbagli la parola in input, a seconda del metodo di input.


