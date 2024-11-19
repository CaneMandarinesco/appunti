## es. 1
riscrivere:
$$
(1056)_{10} 
$$

in base 2, 8, 16.  

per convertire in base 2 basta dividere per 2 e controllare il resto.  
per la base ottale prendo i bit a gruppi di 3 (e li riscrivo in base 8) e per la base 16 prendo i bit a gruppi di 4 (e li riscrivo in base 16).

quindi ho che:
* in base 2: $10000100000$
* in base 8: $2040$
* in base 16: $420$

## es. 2
riscrivere:
$$
(1F 7)_{16}
$$
in base: 2,8,10.

* in base 2: $0001 1111 0110$ (convertendo cifra per cifra)
* in base 8, raggruppo le cifre della base 2 a gruppi di 3: $0767$
* in base 10, $7 \cdot 16^0 + 15 \cdot 16^1 + 1 \cdot 16^2$ ovvero trasformo ogni singola cifra in base decimale e moltiplico per il **peso** della cifra.

## es. 3
$$
\overline A \cdot \overline B \cdot \overline C = \overline{A+B+C}
$$
* $A+B = Z$ -> $\overline{Z+C}$ ...
## es. 4
$$
A+ \overline A \cdot B = A + B
$$

* $A + \overline A \cdot B = A+B(\overline A +A)$
* $= A+ B \overline A + BA$ ma $A+ BA = A$ quindi:
* $= A + B \overline A$

oppure con le mappe di karnaough.  

## es. 4
rappresentare il circuito digitale minimo:
$$
\overline B + \overline {C \cdot (\overline A +B)}
$$
* $Z = \overline A + B$ e quindi: $\overline B + \overline {C \cdot Z}$
* usando de morgan: $= \overline B + \overline C + \overline Z =  \overline B + \overline C + (A + \overline B)$
* poi non lo so porcodio

...

## es. 5 (hamming)

trasmettere $F 5$ usando il codice di hamming.  
* $F 5 = 1111 0001$
* devo posizionare i bit di controllo: $b_{0} b_{1}\,x \, b_{2} \, x x x \, b_{3} \, x x x x$
* $b_{0}$ controlla le posizioni dispari, nota: le posizioni si contano a partire da 1
* $b_{1}$ ecc...


b3 -> 8 = 1000
1 -> 0110