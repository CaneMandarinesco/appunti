definizione: 
$$
\lim_{ t \to b^- } \int_{a}^t f(x) \, dx  = \int _{a}^b f(x) \, dx 
$$
> $b \in \mathbb{R} \cup {+\infty}$

similmente abbiamo:
$$
\lim_{ t \to a^+ } \int_{t}^b f(x) \, dx = \int_{a}^b f(x) \, dx 
$$
> $a \in \mathbb{R} \cup -\infty$

quindi puo' essere:
* convergente
* divergente
* indeterminato

### es. 1
$$ 
\log(x)
$$
* e' una funzione continua in $(0,+\infty)$.
* tenendo presente che: $\int \log x \, dx = x\log x - x +c$

quindi se vogliamo calcolare:
$$
\int _{0}^1 \log(x) \, dx
$$
$$
= [x\log x + x]_{0}^1 = \lim_{  \to 0^+ }[x\log x+x]_{t}^1 = [-1 -\lim_{ x \to 0^+ } x\log x+x] = -1
$$
dove: 
$$
\lim_{ x \to \infty } x\log x+x = x(1+\log (1-1 +x)) \sim x(1-1+x) = 0
$$

invece se abbiamo:
$$
\int_{1}^\infty \log x \, dx 
$$
similmente:
$$
[x\log x-x]_{1}^\infty = [\lim_{ x \to \infty } (x\log x-x) + 1] = \lim_{ x \to \infty } x(\log(x)-1) + 1 = \infty
$$
### es. 2
considerando ora: $\sin x$
* continua in $R$
* $\int \sin x \, dx = -\cos x$
se abbiamo per esempio:
$$
\int_{0}^{+\infty} sen(x) \, dx  = [-\cos(x)]_{0}^{+\infty} = [-\lim_{ x \to \infty } \cos x ] = \nexists
$$

>[!note] OSS.
> se bisogna studiare il limite dell'integrale su tutti e due gli estremi, quindi fare due limiti, conviene spezzare l'intervallo di integrazione, cosi si ottengono due integrali piu' semplici
> 

### es. 3
$$
\int_{-\infty}^{+\infty} \frac{x}{x^2+1} \, dx 
$$
e' continua in $\mathbb{R}$.
$$
\int  \frac{x}{x^2+1} \, dx 
$$
e' della forma: $f(g(x)) + g^I(x)$ quindi:
$$
 = \frac{1}{2}\log(x^2+1 )+ c
$$
quindi ho (spezzando l'intervallo di in): 
$$
\frac{1}{2}[\log(x^2+1)]_{-\infty}^{+\infty} = [\log(x^2+1)]_{0}^{+\infty} + [\log(x^2+1)]_{-\infty}^{0} = 
$$

$$
\lim_{ t \to +\infty } [\log(x^2+1)]_{0}^{t} = \lim_{ t \to \infty } \log(1+t^2) =+\infty
$$

$$
\lim_{ t \to -\infty } [\log(x^2+1)]_{t}^{0} =  \lim_{ n \to \infty }  \log(1+t^2)= \infty
$$
quindi mettendo insieme facendo attenzione ai segni abbiamo:
$$
+\infty - \infty = \nexists
$$
# studio del parametro $\alpha$
## 1o tipo
$$
\int_{0}^1 \frac{1}{x^\alpha} \, dx 
$$
### 1. $\alpha  = 1$
$$
\int_{0}^1 \frac{1}{x} \, dx  = [\log x]_{0}^1 = 0-(-\infty) = +\infty
$$
$$
\int _{1}^\infty \frac{1}{x} \, dx = +\infty
$$
### 2. $\alpha \neq 1$
$$
\int _{0}^1 \frac{1}{x^\alpha} \, dx = \left[ \frac{x^{1-\alpha}}{1-\alpha} \right]_{0+}^1 = \frac{1}{1-\alpha} - \lim_{ x \to 0^+ } \frac{x^{1-\alpha}}{1-\alpha} 
$$
converge se: $a > 1$

---
$$
\int_{1}^\infty  \frac{1}{x^\alpha} \, dx = \left[ \frac{x^{1-\alpha}}{1-\alpha} \right]_{1}^{\infty} = \lim_{ x \to \infty  } \frac{x^{1-\alpha}}{1-\alpha} - \frac{1}{1-\alpha}  
$$
converge se: $\alpha<1$

quindi se l'estremo superiore e' 1, **converge** se $\alpha>1$ altrimenti se $+\infty$, **converge** se $\alpha<1$

## 2o tipo
$$
\int_{a}^b \frac{1}{x|\log(x)|^\beta} \, dx  = \int _{a}^b \frac{1}{t^\beta} \, dt \text{ con: } \; \;t=\log(x) \text{ e dt = } \frac{dx}{x}
$$
> assumendo che: $\beta \neq 0$

4 casi...
## 3o tipo bonus
$$
\int _{0}^{+\infty} e^{\alpha x} \, dx 
$$
converge solo se $\alpha<0$
infatti se: 
* $\alpha = 0$ allora abbiamo $\int_{0}^{+\infty} 1  \, dx = \infty$
* altrimenti: $\frac{1}{\alpha} \lim_{ x \to \infty } e^{\alpha x}-1$ 
quest'ultimo converge se $a<0$

# teoremi per il confronto
## teo confronto per integrali impropri
siano $f, g$ due funzioni tali $\forall x \in [a,b) \to 0\leq f(x) \leq g(x)$
* se $f(x)$ **diverge** allora $g(x)$ **diverge**
* se $g(x)$ **converge** allora $f(x)$ **converge**

dim: [[primitive e teoremi sugli integrali#teorema confronto per integrali impropri]]

da questo teorema ricaviamo che:
$$
\int_{2}^{+\infty} \frac{1}{x^\alpha(\log x)^\beta} \, dx 
$$
converge se:
* $\alpha > 1$ oppure se $\alpha=1 \text{ e } \beta > 1$

---
$$
\int_{0}^{1/2} \frac{1}{x^\alpha(\log(x))^\beta} \, dx 
$$
converge se:
* $\alpha<1$ oppure se $\alpha = 1 \text{ e } \beta<1$


## teorema del confronto asintotico
se $f(x) \sim Lg(x)$ per $x\to b^-$ allora:
$$
\int_{a}^b f(x) \, dx \text{ converge \; se e solo se \; }   \int  _{a}^b g(x) \, dx \text{ converge}
$$

> [!note] OSS.
> se la funzione $f(x)$ da integrare cambia infinite volte segno basta studiare la convergenza di $|f(x)|$
> $$
> \int_{a}^{+\infty} |f(x)| \, dx \text{ converge \;} \to \int_{a}^{+\infty} f(x) \, dx \text{ converge} 
> $$

