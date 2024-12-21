### Es 1.
Una citta e' modellata come un grafo diretto e pesato $G=(V,E,w)$ dove ogni arco ha un peso che rappresenta la benzina che ci vuole per attraversarlo. Lo stadio si trova nel nodo $t$. Guala e clementi si trovano in $s_{1}$ e $s_{2}$, e sarebbe comodo se uno dei due andasse a prendere l'altro in modo da parcheggiare con una macchina sola. Per ogni nodo $v$ conosciamo $c(v)$, ossia il costo per parcheggiare li. $c(s_{1}) = c(s_{2}) = c(t) = 0$. Progettare in $O(m + n \log n)$ un'algoritmo che trova il percorso migliore!

Tieni presente che: $\text{costo} = \text{costo parcheggio} + \text{ costo benzina}$.


### Es. 2


