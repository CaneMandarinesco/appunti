## teoremi e definizioni utili
$$
f(x) = o[g(x)] \text{ allora } \lim_{ x \to 0 } \frac{f(x)}{g(x)} = 0
$$

### es 1
$$
\lim_{ x \to 0 } \frac{x-\sin x}{x \log(\cos x)} = \frac{0}{0}

$$
provo a risolvere usando:
* $\lim_{ x \to 0 }  \frac{log(1+x)}{x} = 1$
* $\lim_{ x \to 0 } \frac{\sin x}{x} = 1$
* $\lim_{ x \to 0 }\frac{cos x-1}{x^2} = -\frac{1}{2}$ limite del coseno ma al contrario
$$
x\log(\cos x) = x\left[ \log\left( x^2\frac{\cos x-1}{x^2}+1 \right) \right] \text{ per } x\to 0: x\left[\log\left(  -x^2 \frac{1}{2} +1 \right) \right] = x\left[ \frac{\log\left( -x^2 \frac{1}{2}+1 \right)}{-\frac{x^2}{2}} \frac{[-x^2]}{2}\right]
$$
$$
= \lim_{ x \to 0 } \frac{x-\sin x}{x \left( \frac{-x^2}{2} \right)} = \lim_{ x \to 0 } \frac{x-\sin x}{\frac{-x^3}{2}}  = \frac{0}{-\frac{x^3}{2}} = 0
$$
Errore, non conviene usare i limiti notevoli al numeratore in questo caso
Si potrebbe risolvere usando Hopital o Taylor
Con Taylor:
$$
\sin x = x-\frac{x^3}{6} + o(x^3)
$$
$$
\lim_{ x \to 0 } \frac{x -\left[ x-\frac{x^3}{6} + o(x^3) \right]}{-\frac{x^3}{2}} = -\frac{x^3}{6} \frac{2}{x^3} = -\frac{1}{3}
$$

### es 2

$$
\lim_{ x \to 0 } \frac{\sqrt[4]{ 1-4x^2+x^4 }-1+x^2}{x^4}
$$
usando: 
$$
(1+t)^\alpha = 1 +\alpha t + \frac{\alpha(\alpha-1)}{2}t^2 + \frac{\alpha(\alpha-1)(\alpha-2)}{6}t^3 + o(t^3)
$$
e quindi:
$$
(1-4x^2+x^4)^{1/4} = 1 + \frac{1}{4}(-4x^2+x^4) = 1-x^2+\frac{x^4}{4}
$$
sostituendo:
$$
\lim_{ x \to 0 } \frac{1-x^2+\frac{x^4}{4}-1+x^2}{x^4} = \frac{\frac{x^4}{4}}{x^4} = \frac{1}{4}
$$
c'e' un **errore nascosto**!  nello sviluppo di taylor se mi fermo a t ($1+\alpha t$), tralascio il termine $x^4$ che compare nel $t^2$ in $1+\alpha t+\alpha(\alpha-1)t^2$ dove $t=(-4x^2+x^4)$ e $t^2 = 16x^4 + o(x^4)$. Se nello sviluppo di taylor mi arresto al 4 grado, mi devo assicurare che ci siano tutti gli $x^4$ che potrebbero comparire allo sviluppo successivo!

quindi:
$$
(1-4x^2+x^4)^{1/4} = 1- x^2+\frac{x^4}{4} - \frac{3}{32}(-4x^2+x^4)^2 = 1-x^2-\frac{5}{4}x^4 + o(x^4)
$$
sostituendo abbiamo
$$
\lim_{ x \to 0 }-\frac{\frac{5}{4}x^4 + o(x^4)}{x^4} = -\frac{5}{4} 
$$

### es 3
$$
\lim_{ x \to 0 } \frac{1-e^{-x^2} + x^3 \sin\left( \frac{1}{x} \right)}{x^2} = \frac{0}{0} 
$$
possiamo dire che: $-x^3 \leq x^3 \sin x \leq x^3$ dato che $x\to 0$ per confronto studio $x^3$
faccio gli sviluppi:
$$
e^{-x^2} = 1+(-x^2) + o(-x^2) = 1-x^2+o(x^2)
$$
quindi:
$$
\lim_{ x \to 0 } \frac{1-(1-x^2 +o(x^2)) + x^3}{x^2}=\frac{x^2-o(x^2)+x^3}{x^2} = \frac{x^2 + o(x^2)}{x^2} = 1
$$
> $x^3$ e' un o-piccolo di $x^4$
### es 4
$$
\lim_{ x \to 0 } \frac{x^2-\sin^2x}{x^3(e^x - \cos x)} = \frac{0}{0} 
$$
$$
\sin x = x - \frac{x^3}{6} + o(x^3)
$$
al numeratore abbiamo:
$$
x^2 - \sin^2x = x^2 - \left( x-\frac{x^3}{6} + o(x^3) \right)^2 = x^2 - \left( x^2 -\frac{x^4}{6}+o(x^4) - \frac{x^4}{6} + \frac{x^6}{36} + o(x^6) + o(x^4) + o(x^6))\right)
$$

$$
\frac{x^4}{3}-\frac{x^6}{36} + o(x^6) = \frac{x^4}{3} + o(x^4)
$$
al denominatore abbiamo: 
$$
e^x = 1+x+o(x)
$$
$$
\cos x = 1+o(x)
$$
e sostituendo
$$
\lim_{ x \to 0 } \frac{\frac{x^4}{3} + o(x^4)}{x^3(1+x+o(x) - 1 -o(x))} = \frac{\frac{x^4}{3} + o(x^4)}{x^4 + o(x^4)} = \frac{1}{3}
$$
### es 5
$$
\lim_{ x \to 0 } \frac{tg(x^2) - x^2}{\log(3+2x)x^2(x^2-\sin^2x)} = \frac{0}{0}
$$

> $\log(3+2x)$ non c'e bisogno di svilupparlo perche' tende a $\log(3)$

$$
\tan x = x+\frac{x^3}{3} +\frac{2x^5}{15} + o(x^5)
$$
quindi: $\tan x^2 = x^2 + \frac{x^6}{3} + o(x^6)$
$$
\sin x = x -\frac{x^3}{6} + \frac{x^5}{5!} + o(x^5)
$$
quindi: $\sin^2x$ = $x^2 - \frac{x^4}{3}+o(x^4)$
e quindi: $x^2(x^2-\sin^2x) = x^2\left[ x^2 - \left( x^2 - \frac{x^4}{3}+o(x^4) \right) \right] = x^2\left[ \frac{x^4}{3}+o(x^4) \right] = \frac{x^6}{3} + o(x^6)$

dato che: $\log(3 + 2x) = \log(3)$ per $x\to 0$

abbiamo che:
$$
\lim_{ x \to 0 } \frac{x^2-\frac{x^4}{3} + o(x^4) - x^2}{\log(3) \frac{x^6}{3}} = \frac{\frac{1}{3}x^6}{\log(3) \frac{1}{3} x^6} = \frac{1}{\log(3)}
$$

### es 6
$$
\lim_{ x \to 0 }\frac{(5^{1+tg^2x} -5)(1+\sin^5x)}{1-\cos x} 
$$

...

### es 7
$$
\lim_{ x \to 0 } \frac{x^5[(e^{x^3})^2-1]}{\sqrt{ 1+x^8 }-\sqrt[3]{1+x^8  }} 
$$
...

### es 8
$$
\lim_{ x \to 0 } \frac{\log(1+\arcsin(x)) - \sin (\log(1+x))}{\arctan(x)} = \frac{0}{0}
$$
...
### es 9