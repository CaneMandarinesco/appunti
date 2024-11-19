## grafi orientati
un grafo si indica orientato si indica cosi:
$$
\vec{ \Gamma } = (V,A)
$$
si può estrarre una matrice da un grafo.

## retta, piano e spazio
### retta euclidea
$$
A^1 = \text{ retta euclidea affine}
$$
* posso fissare un punto $O$, detto **origine** e svolgere varie operazioni rispetto a questo
* dopo $O$ avremo numeri positivi e prima di $O$ numeri negativi
* $\overline{\mathrm{OP}}$ distanza tra punto $P$ e punto $O$

### piano affine
$$
A^2 = \text{ piano affine}
$$
* $\pi = \text{ piano }$
* $\pi \supseteq \{ P \}$ (un punto $P$ puo' **appartenere** a un piano)
* $\pi \ni P$ (un punto $P$ puo' essere elemento di un piano, equivalente a sopra ???)
* $\pi \supseteq r$ (una retta puo' **appartenere** a un piano)

#### rette incidenti, coincidenti e parallele
proprieta' per le rette affini in $A^2$:
* $r_{1} \cap r_{2} = \{ P \}$ ovvero si incontrano in un solo punto allora sono **incidenti**.
---
* $r_{1} \cap r_{2} = \emptyset$ ovvero non hanno punti in comune: sono **parallele** #controllare 
* $r_{1} = r_{2}$ allora sono **coincidenti**

>[!nota] vettore nullo
>$$
>\vec{\mathrm{OO}}
>$$

### spazio affine
$$
A^3 = \text{ spazio affine}
$$
ha dei sottoinsiemi **notevoli** che sono: $P$ (punti), $r$ (rette), $\pi$ (piani).  
siano $\pi^1$ e $\pi^2$ due piani distinti: 
* $\pi^1 \cap \pi^2 = \text{ retta }$ (insieme **non vuoto** e non i due piano **non coincidono**), allora i due piani sono **incidenti** 
* $\pi^1 \cap \pi^2 = \emptyset$ oppure $\pi^1 = \pi^2$ allora sono **paralleli**

#### rette e piani (incidenti e paralleli)
siano $r$ una retta e $\pi$ un piano:
* sono **incidenti** se: $r \cap \pi = \{ P \}$ , quindi la retta incontra il piano in un solo punto
* sono **paralleli** se: $r \cap \pi = \emptyset$ (non si incontrano mai) oppure $r \subseteq \pi$ (ovvero $r$ e' contenuta in $\pi$)

### alcune propr. delle rette
siano $r' \text{ e } r''$ due rette:
* sono **complanari** se $\exists \pi: r'' \subseteq \pi \supseteq r'$. ovvero esiste un unico **piano** che contiene $r' \text{ e } r''$
	* implica che possano essere **incidenti** o **parallele**
* sono **sghembe** se $r' \cap r'' = \emptyset$ e $r' \nparallel r''$ (non sono parallele, se fossero parallele non sarebbero sghembe logicamente)

## vettori
fisso un punto $O$ in $A^{1/2}$ .  
>[!note] DEF.
>un **vettore orientato** in $O$ e' una "freccia" $\vec{\mathrm{OP}}$ dove $P \in A^{1/2/3}$

graficamente: una retta che va da $\mathrm{ O }  \text{ a } \mathrm{ P}$ con una freccia orientata verso $\mathrm{ P }$, un'altro esempio sono due vettori che da $\mathrm{O}$ vanno a $\mathrm{P'} \text{ e } \mathrm{P''}$. 

Sia $A^{1,2,3}$ una retta/piano/spazio contenente per definizione dei punti e $V_{O}$

> [!note] DEF.
> una funzione: $\Phi_{_{O}}: A^{1/2/3} \to V_{0}^{1,2,3}$ costruita come:
> $$
> \Phi_{O}(P) := \vec{\mathrm{OP}}
> $$
> quindi una funzione che ad ogni $P$ associa un vettore orientato che va da $\mathrm{O} \text{ a } \mathrm{ P}$, questa funzione e':
> - iniettiva
> - suriettiva

### somma di due vettori
$$
\forall v' = \vec{\mathrm{OP'}}, v'' = \vec{\mathrm{OP''}} \text{ in } V_{0} \to \exists ! v= \vec{\mathrm{OP}} \text{ in } V_{0}
$$
dove questo $\vec{\mathrm{OP}}$ e' un punto che si trova al vertice opposto ad $O$ nel parallelogramma costruito usando $O, \vec{\mathrm{OP'}} \text{ e }\vec{\mathrm{OP''}}$

# note
* in una retta $\overline{OA}$ i suoi punti sono in corrispondenza biunivoca con i numeri reali





