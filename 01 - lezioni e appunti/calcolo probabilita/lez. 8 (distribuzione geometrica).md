### distribuzione geometrica
Ho una successione di prove indipendenti, tutte hanno probabilità di successo $p \in (0,1]$.

Considero:
* v.a. $X = \# \text{ fallimenti prima del 1o successo}$
* v.a $Y = \# \text{ prove per avere il 1o successo }$
* $X = Y-1$ e $Y = X+1$

Allora possiamo dire che: 
* $X$ ha distribuzione **geometrica** di parametro $p$. (si dice anche geometrica che **parte da zero** )
* $Y$ ha distribuzione **geometrica traslata** di parametro $p$. (geometrica che **parte da uno**)

> si esclude $p=0$ altrimenti non si arriva mai ad un primo successo, ma avrei solo fallimenti.

> in caso di $p=1$ ho invece sempre successi.

#### densità discrete di X e Y
* per $k \geq 0$ si ha $p_{X}(k) = p(F \dots F S) = (1-p)\dots(1-p)\cdot p = (1-p)^k \cdot p$
* per $k \geq 1$ si ha $p_{Y} = (1-p)^{h-1} \cdot p$

> [!note] $X\sim Geo(p)$, con $\geq$
> $$ P(X\geq j) = (1-p)^j$$
> per $j \geq 0$

> [!note] $X\sim \text{GeoTraslata}(p)$, con $\geq$
> $$ P(X\geq j) = (1-p)^j-1$$
> per $j \geq 1$

>[!note] Proprietà della mancanza di memoria
> Per ogni $k,h\geq 0$ interi si ha $P(X=k+h | X \geq h) = P(X=k)$




