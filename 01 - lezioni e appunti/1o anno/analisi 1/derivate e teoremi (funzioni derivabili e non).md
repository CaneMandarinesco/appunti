## massimi e minimi di una funzione
* $x_{0}$ si dice  punto di massimo **assoluto** in A se: $\forall x \in A: \, f(x)\leq f(x_{0})$ ossia $f(x_{0}) = max \{f(x):x \in A\}$
* $x_{0}$ si dice punto di massimo **relativo** in A se: $\exists r > 0: \forall x \in [I(x_{0},r) \cap{} A$] -> $f(x) \leq f(x_{0})$
* similmente per minimo **assoluto**/**relativo**

## definizione di derivata 
f e' derivabile in $x_{0}$ se:
$$
\exists \lim_{ x \to \infty } \frac{f(x_{0}+h)-f(x_{0})}{h} = f'(x_{0}) = \frac{df}{dx}(x_{0})
$$
dove, la notazione $f'(x_{0})$ (alternativamente $\frac{df}{dx}(x_{0})$) indica la derivata in un punto $x_{0}$
da questa definizione deriva il significato geometrico della derivata:
* retta tangente: $y = f'(x_{0})(x-x_{0}) + f(x_{0})$, ovvero la retta che interseca in **un solo punto il grafico** della funzione in $x_{0}$
* retta secante: $y=\frac{f(x_{0}+h) - f(x_{0})}{h}(x-x_{0}) + f(x_{0})$, ovvero la retta che interseca il grafico in **due punti** della funzione $x_{0}$

> [!note] OSS.
> se $f$ e' continua in $x_{0}$ avremo che per $x\to x_{0}$:
> $$
> f(x) = f(x_{0}) + \left( \frac{f(x)-f(x_{0})}{x-x_{0}}(x-x_{0}) \right) \to f(x_{0})
> $$

> [!warning] IMPORTANTE: derivata del modulo di x
> per derivare il modulo di x, e' opportuno come per lo studio di funzione, dividere la funzione in due parti in modo da eliminare il modulo

## teorema di fermat
sia $f(x)$ una funzione continua in $(a,b)$ e sia $x_{0}$ un punto di **massimo o minimo  relativo** (o anche assoluto), se f e' derivabile in $x_{0}$ allora $f^I(x_{0}) = 0$ ossia $x_{0}$ e' un **punto stazionario**

> [!note] DIM.
> dimostrazione per $x_{0}$ **massimo relativo**.
> andiamo a dimostrare che il massimo relativo ha derivata destra e sinistra e che hanno un'**unica soluzione comune**.
> 
> sicuramente $\exists r>0 \text{ t.c.  } (x_{0}-r,x_{0}+r)$ e' sotto insieme di $(a,b)$ (quindi $x_{0}$ si trova dentro $(a,b)$) ed essendo $x_{0}$ massimo relativo -> $\forall x \in (x_{0}-r, x_{0}+r) \to f(x)\leq f(x_{0})$.
> ora andiamo a studiare il segno del numeratore e del denominatore del **rapporto incrementale** da destra e da sinistra($\frac{f(x_{0}+h)-f(x_{0})}{h}$)in $x_{0}$:
> - per $h\to 0^+$:  $\frac{\leq 0}{+} \text{ ovvero } \leq 0$
> - per $h\to 0^-$: $\frac{\geq 0}{-} \text{ ovvero } \geq 0$
> 
> dato che $f(x)$ e'derivabile in $f(x_{0})$ applichiamo la [[limiti di successioni e di funzioni, nepero, teo. ponte#3. tipo permanenza del segno|permanenza del segno]] sulla derivata destra e sinistra(???)
> applicando la permanenza del segno sappiamo che :
> - $f^I_{+}(x) \leq 0$ 
> - $f^I_{-}(x) \geq 0$ 
> 
> hanno un'unica soluzione comune ovvero: $f^I(x_{0}) = 0$
> 

## teorema di Bolzano-Weierstrass
se $\{a_{n}\}_{n}$ limitata allora $\exists a_{n_{k}}$  convergente.
ovvero se esiste una successione limitata -> esiste una sua **sotto-successione convergente**
lagrange
## teorema di Weierstrass
* [dimostrazione simile a quella del tauraso](https://www.youmath.it/forum/analisi-1/61639-dimostrazione-del-teorema-di-weierstrass.html)
se $f(x)$ e' continua in $[a,b]$ (***COMPATTO***) allora $\exists x_{min} \in [a,b]$ e $\exists x_{max} \in [a,b]$ t.c $\forall x \in [a,b]: f(x_{min}) \leq f(x) \leq f(x_{max})$
ovvero se f e' continua in $[a,b]$ allora esistono un $x_{min}$ (punto di **minimo** in $[a,b]$) e un $x_{max}$ (punto di **massimo** in $[a,b]$) tali che $f(x)$ in $[a,b]$ e' **compresa** tra $f(x_{min})$ e $f(x_{max})$ (ossia $f(x_{min})$ e' **minimo assoluto** in $[a,b]$  e $f(x_{max})$ e' **massimo assoluto** in $[a,b]$

> OSS. $x_{min} \text{ e } x_{max}$ sono i termini n-esimi di una successione $x_{n}$

>[!note] OSS.
> quindi per il [[teoremi delle funzioni continue#teorema dei valori intermedi|teorema dei valori intermedi]] $f(x)$ e per il teorema di Weierstrass  $f(x)$ assume **tutti i valori** in $[a,b]$ ed ha anche un **massimo** e un **minimo** **assoluto** in $[a,b]$

>[!note] DIM.
> consideriamo $M = sup \{f(x_{n}), \forall x \in [a,b]\}$ (valore massimo di $f(x)$ in $[a,b]$)
> per dimostrare che esiste un **massimo assoluto**, bisogna far vedere che $M$ non puo essere $+\infty$.
>
> per $M = +\infty$, vuol dire  che in un qualsiasi intorno di $+\infty$, $f(x_{n})$ e' compresa in $(n, +\infty)$(def. di limite di f) e quindi $f(x_{n}) > n$. dato che noi stiamo studiando $f(x_{n})$ in $[a,b]$, $x_{n}$ in $[a,b]$ e limitata e per **Bolzano-Weierestrass** esiste un $x_{n_{k}}$ che **converge**
> se esiste il limite $x_{n}$ allora $\exists x_{n_{k}}$ e le altre sue sotto successioni devono tendere a $+\infty$ ma abbiamo appena dimostrato che $x_{n_{k}}$ converge. **assurdo**
> 
> invece per $M \in \mathbb{R}$ abbiamo che $f(x) \leq M$ e applicando la definizione di **limite superiore** abbiamo che:
> - $M-\epsilon < f(x_{n}) \leq M$ dove $\epsilon=\frac{1}{M}>0$
> 
> quindi possiamo costruire una successione $x_{n}$, che e' limitata in $[a,b]$. quindi per **Bolzano-Weierstrass** esiste una sotto successione $x_{n_{k}}$ **convergente** a $x_{0}$.
> dato che $f(x)$ e' continua in $[a,b]$ allora -> $\lim_{ k \to +\infty } f(x_{n_{k}}) = f(x_{0}) = M$
## teorema di lagrange
il teorema afferma che se $f(x)$ e' continua in $[a,b]$ allora $\exists c: f(c) = \frac{f(b)-f(a)}{b-a}$, ovvero il coefficiente angolare della retta secante che passa per $b$ e $a$.
### DIM.
considero la **funzione ausiliaria:**
$$
h(x) = f(x) - \left( \frac{f(b)-f(a)}{b-a}(x-a) + f(a) \right)
$$
tenendo presente che:
* $h(a) = f(a) - f(a) = 0$
* $h(b) = f(b) - (f(b) - f(a) + f(a)) = 0$

ora dato che $h$ e' continua in $[a,b]$: assume tutti i valori tra $a$ e $b$ ed ammette un **minimo**: $x_{min}$ e un **massimo**: $x_{max}$ assoluto. possiamo distinguere ora due casi:
* $x_{min} \text{ e } x_{max} \in \{ a,b \}$ quindi $h$ e' costante in $[a,b]$ e la sua derivata vale sempre $0$
* uno tra $x_{min} \text{ e } x_{max}$ e' in $[a,b]$ quindi, per **fermat**: $\exists c: h'(x) = 0$

in entrambi i casi esiste almeno un punto $c$ in cui $h'(c) = 0$, e quindi:
$$
0 = h'(c) = f'(c) - \frac{f(b)-f(a)}{b-a}
$$
quindi risolvendo l'equazione si ha che:
$$
f'(c) = \frac{f(b)-f(a)}{b-a}
$$

## Criterio di monotonia
se $f$ e' continua e derivabile in $(a,b)$ allora $\forall x \in D$:
* $f'(x)\geq 0$ se e solo se $f(x)$ e' crescente in $(a,b)$
* viceversa

### dim. $f'(x)\geq 0 \text{ allora } f(x) \text{ e' crescente }$
studiamo cosa succede alla derivata destra e sinistra:
* $f_{-}'(x) = \frac{f(x+h)-f(x)}{h}$ dove: $N \leq 0$ dato che $f$ e' crescente ($h<0$), e $D < 0$, quindi e' $\geq 0$
* $f'_{+}(x) = \frac{f(x+h)-f(x)}{h}$ dove: $N \geq 0$ e $D \geq 0$ (per lo stesso ragionamento di prima). quindi e' $\geq 0$

quindi $f'(x)\geq 0$ implica che $f(x)$ sia crescente


### dim. $f(x) \text{ e' crescente , allora } f'(x) \geq 0$
prendiamo in considerazione: $x_{0}, x_{1}: x_{0}<x_{1}$, per definizione di funzione crescente abbiamo che: $f(x_{0}) \leq f(x_{1})$.
applicando il teorema del valor medio, abbiamo che: $\exists c: f'(c) = \frac{f(x_{1})-f(x_{0})}{x_{1}-x_{0}}$ dove $f'(c) \geq 0$ perche' essendo $f(x)$ crescente per ipotesi, allora $\frac{f(x_{1})-f(x_{0})}{x_{1}-x_{0}}$ 

quindi se $f(x)$ e' crescente allora $f'(x)\geq 0$

## Teorema di cauchy (simile a lagrange)
se $\forall x \in (a,b), g'(x) \neq 0$  (condizione fondamentale del teorema), allora:
$$
\exists c \in (a,b): \frac{f(b)-f(a)}{g(b)-g(a)} = \frac{f'(c)}{g'(c)} 
$$

## Teorema de l'hopital
siano $f$ e $g$ tali che:
* $\lim_{ x \to x_{0}^+ }f(x) = \lim_{ x \to x_{0}^+ } g(x) = 0$ oppure $\lim_{ x \to x_{0}^+ } f(x) = \pm \infty$ e $\lim_{ x \to x_{0}^+ } g(x) = \pm \infty$
* $\forall x \in (x_{0}, x_{0}+r) g'(x)\neq 0$
* $\exists \lim_{ x \to x_{0}^+ } \frac{f(x)}{g(x)} = L$
Allora $\exists \lim_{ x \to x_{0}^+ } \frac{f(x)}{g(x)} = L$

### dim.  per limiti tendenti a 0
poniamo che $f(x_{0}) = g(x_{0})=0$, estendendone quindi il dominio di queste due funzioni, cosi sono continue in $[x_{0}, x_{0}+r)$
consideriamo una successione $\{ x_{n} \}_{n}$ in $(x_{0}, x_{0}+r)$ tale che $x_{n} \to x_{0}^+$ allora $\forall n \in N^+$ per Cauchy:
$$
\exists c_{n} \in (x_{0},x_{n}): \frac{f(x_{n})}{g(x_{n})} = \frac{f(x_{n})-f(x_{0})}{g(x_{n}) - g(x_{0})} = \frac{f'(c_{n})}{g'(c_{n})}
$$
dato che $x_{0} < c_{n} < x_{n}$, per doppio confronto si ha che: $c_{n} \to x_{o}^+$ (dato che $x_{n} \to x_{0}^+$)e:
$$
\frac{f(x_{n})}{g(x_{n})} = \frac{f'(c_{n})}{g'(c_{n})} \to L
$$
e per teorema ponte cio' vale anche per i limiti di funzioni

> [!warning] non abusare di de l'hopital!
> non affidarti solo a de l'hopital per risolvere i limiti, usa sempre i limiti notevoli.
> 


