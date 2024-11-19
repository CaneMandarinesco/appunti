## o-piccolo
se:
$$
\lim_{ x \to x_{0} } f(x) = \lim_{ x \to x_{0} }  = 0
$$
e: 
$$
\lim_{ x \to x_{0} } \frac{f(x)}{g(x)} = 0 
$$
allora diremmo che $g(x)$ e' un o-piccolo di $f(x)$ per $x\to x_{0}$, scritto:
$$
f(x) = o(g(x))
$$

# taylor con resto di peano

considerando $f(x)$ in un intorno, con $r>0$ di  $I(x_{0},r)$ possiamo dire che 
$$
f(x) = T_{n,x_{0}}(x) + o((x-x_{0})^n)
$$
ovvero che esiste un'approssimazione di $f$ nel punto $x_{0}$.
ricordando che: 
$$
T_{n,x_{0}} = \sum_{k=0}^n \frac{f^{(k)}(x_{0})}{k!}(x-x_{0})^n
$$
procediamo con la dimostrazione.
### dim.
dalla definizione di o-piccolo, dobbiamo dimostrare che $f(x)-T_{n,x_{0}}(x)$ converga e che sia un o-piccolo di $(x-x_{0})^n$ quindi scriveremo:
$$
\frac{f(x)-T_{n,x_{0}}(x)}{(x-x_{0})^n} = \frac{f\left( h+x_{0} \right) - \sum_{k=0}^n \frac{f^{(k)}(x_{0})}{k!}h^k}{h^n} = \frac{f(h+x_{0}) - \sum_{k=0}^{n-1} \frac{f^{(k)}(x_{0})}{k!}h^k}{h^n} + \frac{f^{(n)}}{n!}
$$

> $h=x-x_{0}$

se svolgiamo il limite per $h\to 0$ otteniamo una forma indeterminata quindi possiamo applicare **de Hospital**, analizzando cosa succede termine per termine:
* $f(h+x_{0})$ diventa $f'(h+x_{0})$
* $h^n$ diventa $n* h^{n-1}$
per analizzare la sommatoria proviamo a svilupparla e avremo che:
$$
- \sum_{k=0}^n \frac{f^{(k)}(x_{0})}{k!} h^k = -\frac{f(x_{0})}{1}(1) - \frac{f'(x_{0})}{1}h - \frac{f''(x_{0})}{2}h^2 \dots
$$
ora se deriviamo 1 volta otteniamo: 
$$
0 - f'(x_{0}) - f''(x_{0})h
$$
> nota: $f(x_{0})$ e' una costante quindi la sua derivata e' 0
 
derivando una seconda volta  si ottiene:
$$
0 - 0 - f''(x_{0})
$$
e cosi via. quindi derivando (applicando de hopital) per $n-1$ volte si ottiene :
$$\frac{f^{(n-1)}(h+x_{0})-f^{(n-1)}(x_{0})}{n!h} = \frac{f^{(n)}}{n!}$$
ottengo quindi la definizione di derivata di: $f^{n}(x_{0})$

da qui segue la tesi
$$

$$