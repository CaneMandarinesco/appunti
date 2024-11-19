> [!note] definizione di primitiva 
> $\forall x \in A, \; F^I(x) = f(x)$

quindi quella funzione che **derivata** mi da $f(x)$

> [!note] OSS.
> per completezza, le primitive di una $f(x)$ sono infinite e sono espresse dalla forma: $F(x)+c$

## teorema della media integrale
se f e' continua in $[a,b]$ allora $\exists c \in [a,b]$ t.c.
$$
\frac{1}{b-a} \int _{a}^b f(x) \, dx  = f(c)
$$
ovvero esiste una $f(c)$ con $c \in [a,b]$ che e' uguale al **rapporto** tra l'integrale di $f(x)$ e la **lunghezza dell'intervallo di integrazione**
![[Pasted image 20240205114427.png]]

>[!note] DIM.
> sia $\delta$ la suddivisione in solo 2 punti di $[a,b]$ (ovvero $a<b$)
> quindi:
> $$
> m(b-a) = s(f,\delta) \leq \int _{a}^b f(x) \, dx \leq S(f,\delta) = M(b-a)
> $$
> ossia:
> $$
> m(b-a) \leq \int _{a}^b f(x) \, dx \leq M(b-a) \to m \leq \frac{1}{b-a}\int _{a}^b f(x) \, dx \leq M
> $$
> dove $m \text{ e } M$ sono **minimo e massimo** di $f(x)$, che per teorema **Weierestrass**, esistono sempre, e dato che per il teorema dei **valori intermedi** $f(x)$ assume tutti i valori in $[m,M]$ allora e' confermata la ipotesi.
> 

## teorema fondamentale del calcolo integrale
sia $f(x)$ una funzione continua in $[a,b]$ e $I$ la **funzione integrale**:
$$
I(x) = \int _{a}^x f(t) \, dt 
$$
allora:
1. $I$ e' derivabile in $[a,b]$ e quindi $I^I(x) = f(x)$ ossia $I$ e' una **primitiva** di $f(x)$
2. se invece $F$ e' una qualunque primitiva di $F(x)$ allora: $\int _{a}^bf(x) \, dx = [F(x)]_{a}^b = F(b) - F(a)$

> [!note] DIM. (1)
> studiamo il rapporto incrementale di $I(x)$
> sia $x \in [a,b]$ e sia $h \neq 0$  t.c. $x+h\in[a,b]$, allora abbiamo che:
> $$
> \frac{I(x+h)-I(x)}{h} = \frac{1}{h}\left( \left( \int _{a}^{x+h}f(t) \, dt  \right)-\int _{a}^xf(t) \, dt  \right) = 
> $$
> $$
> \frac{1}{h}\int_{x}^{x+h}f(t) \, dt 
> $$
> e per teorema della **media integrale** $\exists c(h) \in [x,x+h]$
> - dove c(h) e' tale che tende $x$ (???)
> 
> infine se $h\to 0$ per doppio confronto (???) $c(h)\to x$ e dato che $f$ e' continua -> $fc((h)) \to f(x)$
> quindi il limite del rapporto incrementale di $I(x)$ e' $f(x)$

>[!note] DIM. (2)
>Se $F$ e' una primitiva di f allora $\exists c \in R$ t.c. $F(x) = I(x)+c$ e $F(b)-F(a) = I(b) + c - I(a)- c = I(b)$ dato che $I(a) = 0$ e infine abbiamo che $= \int _{a}^b f(x) \, dx$  

>[!note] OSS.
>  dal teorema fondamentale del calcolo segue il problema del calcolo dell'**integrale definito**

## teorema confronto per integrali impropri
### OSS. integrale di $f \geq 0$ e' $\geq 0$
se $f(x)\geq 0$
sia $F(x) = \int _{a}^x f(t) \, dt$, se considero una suddivisione di $[a,b]$ = $a \leq x_{1} < x_{2} < b$ allora posso dire che:
$$
F(x_{2})-F(x_{1}) = \int _{a}^{x_{2}}f(x) \, dx - \int _{a}^{x_{1}} f(x) \, dx = \int  _{x_{1}}^{x_{2}} f(x)\, dx   
$$
quindi ho che $F(x_{2})\geq F(x_{1})$ e che (dato che $f(x)\geq 0$): $F(x_{2}) - F(x_{1}) \geq 0$
quindi se considero il limite della funzione integrale:
$$
\lim_{ x \to b^- } \int _{a}^x f(x) \, dx \geq 0  
$$
quindi puo': **convergere** o **divergere** a $\infty$

quindi l'integrale di una funzione positiva e' sempre $\geq 0$ e puo divergere

### teorema
sia $0\leq f(x)\leq g(x)$, allora:
* $\int_{a}^b f(x) \, dx$ converge allora converge $\int _{a}^bg(x) \, dx$
* $\int _{a}^b g(x) \, dx$ diverge allora diverge $\int _{a}^b f(x) \, dx$

#### dim.
siano:  
$$
F(x) = \int _{0}^x f(t)\, dt \;\; \text{ e } \;\; G(x) = \int _{0}^x g(t) \, dt
$$
con $a\leq x<b$:
$$
G(x)-F(x) = \int _{a}^x g(t)-f(t)\, dt \geq 0 
$$
> vale per l'osservazione precedente, quindi $\int g(t) \, dt \geq \int  f(t) \, dt$

considero i limiti per $x\to b$ e ho che:
$$
\lim_{ x \to b^- }  \int _{a}^x g(t)  \, dt \geq \lim_{ x \to b^- }  \int_{a}^x f(t) \, dt 
$$
da qui segue l'ipotesi