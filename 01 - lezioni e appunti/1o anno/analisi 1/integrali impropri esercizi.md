### es 1
$$
\int _{1}^{+\infty} \frac{1}{(x-1)^{\alpha}}\log\left( \frac{x}{x-1} \right)\, dx 
$$
> 1. discutere la convergenza per $a \in R$
> 2. calcola l'integrale per $\alpha = \frac{1}{2}$

### 1.
abbiamo problemi di convergenza per sia $+\infty$ che a $1$. posso studiare usando il criterio del confronto asintotico dato che $f(x) \geq 0$

per $x\to +\infty$ abbiamo che:   
$$
f(x) \sim \frac{1}{x^\alpha} \log\left( \frac{x-1+1}{x-1}\right) = \frac{1}{x^\alpha} \log\left( 1 +\frac{1}{x -1} \right)
$$
> per $x \to \infty$ le quantità costanti sono trascurabili, quindi: $x-1 \sim x$

> se avessi raccolto la $x$ anche dentro al logaritmo, avrei ottenuto: $\log(1)$ che fa $0$, il che non mi permette di studiare la convergenza della funzione. devo cercare un'altra strada che mi permetta di confrontare il logaritmo con $\frac{1}{x^\alpha}$

> un'altra alternativa e' aggiungere $1-1$ al numeratore. allenandosi, ci si accorge ad occhio che facendo cosi si ottiene un logaritmo avente la forma: $\log(1+x)$ per cui possiamo applicare Taylor (dato che $\frac{1}{x-1}$ tende a 0 per $x\to \infty$)

dove usando $\log(1+x) = x+o(x)$ (con $x=\frac{1}{x-1}$)
$$
 f(x) \sim \frac{1}{x^\alpha} \frac{1}{x-1} \sim \frac{1}{x^ax} = \frac{1}{x^{\alpha+1}}
$$
dato che stiamo studiando la convergenza a $+\infty$ dobbiamo imporre: $\alpha>1$, in questo caso:
$$
\alpha + 1 > 1 \to \alpha > 0
$$

per $x \to 1$ invece abbiamo che:
$$
f(x) \sim \frac{1}{(x-1)^\alpha} \log\left( \frac{1}{x-1} \right) = \frac{1}{(x-1)^\alpha} (\log(1) - \log(x-1)) = -\frac{\log(x-1)}{(x-1)^\alpha}
$$
> dove ho sostituito 1 ad $x$ dove sono sicuro che non crea problemi

> ho diviso il logaritmo in due parti usando: $\log\left( \frac{a}{b} \right) = \log(a) - \log(b)$

ora posso imporre: $t = x-1$ e studiare per $t\to 0$, quindi:
$$
f(x) = f(t) = - \frac{\log (t)}{t^\alpha} = - \frac{1}{t^\alpha \log(t)}
$$
ora per $t->0$, l'integrale converge se $\alpha<1$

di conseguenza l'integrale converge se $0<\alpha<1$
