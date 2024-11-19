## metodo della sostituzione
1. indovinare la forma della soluzione
2. dimostrarla per induzione
3. risolvere rispetto alle costanti
### esempio:
$$
T(n) = n + T\left( \frac{n}{2} \right)
$$
Proviamo a dimostrare per induzione che, $T(n) \leq c \cdot n$, con $c$ **costante opportuna**.
* Passo base: $T(1)=1 \leq c \cdot 1$ per ogni $c\geq 1$.
* Passo induttivo: $T(k) \leq c \cdot k$ per ogni $k<n$

$$
T(n) = n + T\left( \frac{n}{2} \right) \leq n +c\left( \frac{n}{2} \right) = n \left(\frac{c}{2} + 1 \right)
$$
ora devo verificare che $\frac{c}{2}+1\leq c$ (1) da qui segue che $c\geq 2$. La dimostrazione e' conclusa e' oltre ad aver visto che $T(n) = O(n)$ so anche che $T(n)\leq 2n$.
La  formula (1) conclude la dimostrazione perché': sapendo che $T(n) \leq n \left( \frac{c}{2}+1 \right))$, per confermare la mia ipotesi devo verificare che $T(n) \leq c \cdot n$. E' ciò e' vero solo per $c\geq 2$.

### esempio
$$
T(n) = 4T\left( \frac{n}{2} \right)+n
$$
proviamo a dimostrare che: $T(n) \leq c \cdot n^3$. 
$$
T(n) = 4 T\left( \frac{n}{2} \right) + n \leq \frac{1}{2}c \cdot n^3 + n =c n^3 - \left( \frac{1}{2} cn^3 -n \right) \leq c n^3
$$
nella formula: $c n^3 - \left( \frac{1}{2} cn^3 -n \right)$ ho messo in evidenza:
* il **goal**: $cn^3$
* il **residuo**: $\frac{1}{2}cn^3 -n$, che devo imporre essere $\geq 0$ nella dimostrazione, quindi $c\geq 2$.

Posso concludere quindi che: $T(n) \leq 2 n^3$

### esempio in cui le cose vanno male!
$$
4T\left( \frac{n}{2} \right) + n
$$
ma provo a dimostrare che: $T(n) = O(n^2)$.

Assumendo che $T(n)\leq c k^2$ per $k<n$:
$$
T(n) = 4T\left( \frac{n}{2} \right) + n\leq 4c\left( \frac{n}{2} \right)^2 + n = cn^2 + n \nleq cn^2
$$

Qui non funziona perché l'ipotesi non e' **abbastanza forte**. Provo a dimostrare che: $T(n) \leq c_{1}n^2 - c_{2}n$ e che se vale per ogni $k<n$ allora vale per $n$.
$$
T(n) = 4T\left( \frac{n}{2} \right) +n \leq 4\left( c_{1}\left( \frac{n}{2} \right)^2 - c_{2}\left( \frac{n}{2} \right) \right) + n = c_{1}n^2 - 2c_{2}n + n = 
$$
$$
c_{1}n^2-c_{2}n - (c_{2}n - n) \leq c_{1}n^2-c_{2}n
$$
che vale con $c_{2}n - n \geq 0$, con $c_{2}\geq 1$.

## Teorema master
metodo che mi da una stima esatta del mio algoritmo. Si applica alla ricorsione:
$$
T(n) = \begin{cases}
a T\left( \frac{n}{b} \right) + f(n) \text{ se n > 1} \\ \\
\Theta(1) \text{ se n=1}
\end{cases}
$$

1. $T(n) = \Theta(n^{\log_{b}a}) \text{ se } f(n)=O(n^{\log_{b}a - \epsilon}) \text{ per } \epsilon > 0$. Ovvero il $n^{\log_{b}a}$ e' piu veloce di $f(n)$.
2. $T(n) = \Theta(n^{\log_{b}a} \log n) \text{ se } f(n) = \Theta(n^{\log_{b}a})$. Ovvero $n^{\log_{b}a}$ e' asintoticamente uguale a $f(n)$
3. $T(n) = \Theta(f(n)) \text{ se } f(n) = \Omega(n^{\log_{b}a + \epsilon}) \text{ per } \epsilon > 0 \text{ e } a f\left( \frac{n}{b} \right) \leq cf(n) \text{ per } c <1 \text{ e } n \text{ sufficientemente grande }$ ossia 

> e' piu veloce $n^{\log_{b}a}$ o $f(n)$? 

