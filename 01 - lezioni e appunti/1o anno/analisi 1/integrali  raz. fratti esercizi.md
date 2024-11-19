a mio parere gli esercizi piu' brutti e noiosi di analisi 1 
## es 1
$$
\int \frac{3x-1}{(x+1)(x-1)(x-3)} \, dx 
$$
riscriviamo la frazione e la scomponiamo:
$$
\frac{3x-1}{(x+1)(x-1)(x-3)} = \frac{A}{x+1} + \frac{B}{x-1} + \frac{C}{x-3}
$$
calcoliamo A, B e C:
* $A(x-1)(x-3)+B(x+1)(x-3)+C(x^2-1) = 3x-1$
* $A(x^2-4x+3) + B(x^2-2x-3) + C(x^2-1) = 3x-1$
* $Ax^2 + Bx^2 + Cx^2 -A4x - B 2x + A 3 - B 3 - C = 3x -1$  
* $x^2(A+B+C) + x(-4A -2B) + 3A - 3B - C$
quindi:
1. $A+B+C = 0$ 
2. $-4A-2B = 3$
3. $3A-3B-C = -1$

dalla (1): $C = -A-B$ e usando la (3) $3A-3B +A + B = -1$  
ottengo: $4A-2B = -1$ (4) che posso sommare alla (2).  
quindi ho: $-4B = 2$ e quindi $B=-\frac{1}{2}$  
ora che conosco il valore di B, sostituisco nella 4: $4A-2\left( -\frac{1}{2} \right) = -1 \to 4A+1=-1 \to A=-\frac{1}{2}$  
infine, dalla (1)  ho che $C = \frac{1}{2}+\frac{1}{2}$ e quindi $C=1$

infine dopo tutto sto gran casino ho che:
$$
\frac{A}{x+1} + \frac{B}{x-1} + \frac{C}{x-3} = -\frac{\frac{1}{2}}{x+1} -\frac{\frac{1}{2}}{x-1}+\frac{1}{x-3}
$$

quindi sostituendo dentro l'integrale ho che:
$$
-\frac{1}{2}\int \frac{1}{x+1}  \, dx  -\frac{1}{2}\int \frac{1}{x-1} \, dx + \int \frac{1}{x-3} \, dx =-\frac{1}{2} (\log(x+1) + \log(x-1)) + \log(x-3)
$$

> nota: i logaritmi hanno valore assoluto dell'argomento
