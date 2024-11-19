### es. 1
$$
\int_{e}^{e^\sqrt{ 3 }} \frac{\arctan(\log x)}{x\log^2(x)} \, dx
$$

con $t = \log(x)$ e $dt = \frac{dx}{x}$

$$
\int _{e}^{e^\sqrt{ 3 }} \frac{\arctan(\log x)}{(\log x)^2} \frac{1}{x} \, dx  = \int _{1}^\sqrt{ 3 } \frac{\,\arctan(t)}{t^2} dt = \int  _{1}^{\sqrt{ 3 }} \frac{1}{t^2} \arctan(t) \, dx 
$$
integrando per parti:
$$
\left[ -\frac{1}{t} \arctan(t) \right]_{1}^\sqrt{ 3 } +  \int_{1}^\sqrt{ 3 }   \, \frac{1}{t} \frac{1}{1+t^2}dx 
$$
risolvo l'integrale a sinistra:
$$
\left[ -\frac{1}{t} \arctan(t) \right]_{1}^\sqrt{ 3 } = -\frac{1}{\sqrt{ 3 }} \arctan(\sqrt{ 3 }) + 1\arctan(1) = -\frac{1}{\sqrt{ 3 }} \frac{\pi}{3} + \frac{\pi}{4}
$$

continuo con la risoluzione scomponendo $\frac{1}{t} \frac{1}{1+t^2}$:
$$
\frac{1}{t(1+t^2)} = \frac{A}{t} +\frac{Bt+C}{t^2+1}
$$
$$
1 = A(t^2+1) + Bt^2+Ct \to 1 = t^2(A+B) + tC + A
$$
da cui ricaviamo il sistema costituito da:
* $A+B =0$ (1)
* $C = 0$
* $A = 1$

sostituendo nella (1), ho che: $1+B = 0$ e quindi $B = -1$
quindi la soluzione e: $A = 1, B = -1, C = 0$
sostituendo abbiamo:  
$$
\int_{ 1}^\sqrt{ 3 } \frac{1}{t} + -\frac{t}{t^2+1} \, dt  =
$$

$$
\int_{1}^\sqrt{ 3 } \frac{1}{t}  \, dx  - \int_{1}^\sqrt{ 3 }  \frac{t}{t^2+1} \, dx =[\log|t|]_{1}^{\sqrt{ 3 }} - \frac{1}{2}\left[ \log(1+t^2) \right]_{1}^\sqrt{ 3 }
$$
quindi sommando con il risultato di prima e calcolando l'integrale:
$$
-\frac{\pi}{3\sqrt{ 3 }} + \frac{\pi}{4} +\log\sqrt{ 3 }- \frac{1}{2}log(4)  \,
$$
bisognerebbe razionalizzare la radici e sommare ma sti cazzi.

### es 3.
$$
\int _{0}^{\log 2}  e^{2x} \log(1+e^x)\, dx 
$$

> nota: $\int \log(1+t) dt = \int \log(1+t) \, d(1+t) = [1+t]\log(1+t) - \int \dots \, dx$ 