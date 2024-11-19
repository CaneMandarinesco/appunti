# formule
* $i^2 = -1$
* $z = x+iy$ 
* $\mathrm{Re}(z) = x$
* $Im(z) = y$
* $|z| = \sqrt{ x^2+y^2 }$
* $\overline{z}= x-iy$
* $z \, \overline{z} = |z|^2 = x^2+y^2$
* forma cartesiana -> esponenziale: 
	* $|z|e^{i\sigma +2\pi k}$ 
	* $\sigma = -\arccos\left( \frac{x}{|z|} \right) \text{ se } y < 0$
	* $\sigma = \arccos\left( \frac{x}{|z|} \right) \text{ se } y \geq 0$
	* $2k\pi$ dove $k \in [0,n$ dove $n$ e' il numero di soluzioni (negli esercizi, corrisponde al valore della radice)
	* l'esponente va diviso per n, tutti i membri compreso $2k\pi$
* esponenziale -> formula di eulero(trigonometrica):
	* $z=|z||(\cos(\sigma) + i\sin(\sigma))$
	* $x = |z|\cos(\sigma)$
	* $y= |z|\sin(\sigma)$
## oss.
le frazioni con parte immaginaria al denominatore vanno moltiplicate per il loro coniugato in modo da far sparire la parte immaginaria al numeratore. l'obiettivo e' quello di portare la frazione alla forma del numero complesso: $x+iy$

## oss.
durante lo svolgimento se compaiono radici strane, tipo: $\sqrt{ 1+\sqrt{ 3i } }$
 e' meglio portarle a forma esponenziale

## forma trigonometrica
esempio:
$$
\left( \frac{3-i}{1-2i} \right)^2 -2\sqrt{ 3}
$$

## radice n-sime di numeri complessi
$$
z^n = w
$$
> la domanda e' per quali z, vale l'uguaglianza?

tenendo presente che:
* $z = |z|e^{i\sigma}$ -> $z^n = |z|^ne^{i\sigma n}$
* $w = |w|e^{i\phi}$

riscrivo l'uguaglianza sopra: 
$$
z^n = w \to \sqrt[n]{ z^n } = \sqrt[n]{ w } \to z = \sqrt[n]{ w }
$$
ora dobbiamo riscrivere in termini di esponenziale quindi:
$$|z|e^{i\sigma} = \sqrt[n]{ |w| }\,e^{\frac{i\phi+2\pi k}{n}}

$$
dunque abbiamo che (separando modulo e l'argomento esponenziale):
* $|z| = |\sqrt[n]{ w }|$
* $\color{red}n\color{white}i\sigma = \frac{i\phi+2\pi k}{\color{red}n\color{white}}$ 

dove $k = 0,1,\dots,n-1$ dove $n$ e' l'argomento della radice, determina quindi il numero di soluzioni.

per risolvere quindi una equazione di questo tipo bisogna portare $w$ (o entrambe le parti) ad esponenziale.

> [!warning] portare termini fuori dalla radice
> nelle operazioni coi complessi sara' facile incontrare varie radici da semplificare, quando si porta fuori un termine del tipo $x^2$ dalla radice, questo diventa: $|x|$!!!


