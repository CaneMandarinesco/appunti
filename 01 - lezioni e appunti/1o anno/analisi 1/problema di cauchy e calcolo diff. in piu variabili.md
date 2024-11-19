# problema di cauchy
>[!note] equazione differenziale
> compare sia $y(x)$ che la derivata $y^I(x)$

esempio:
$$
y^I(x) = \frac{1}{1+x^2} 
$$
integrando si ha che: 
$$
y(x) = \arctan(x) + c
$$

forma generica dei un **equazione differenziale lineare del primo ordine**:
$$
y^I(x) + a(x)y(x) = f(x)
$$
## risoluzione
* moltiplicare tutto per il fattore integrante: $e^A(x)$
	* dove $A(x)$ e' la primitiva di $a(x)$
	* otteniamo: $e^{A(x)} y^{I}(x) + e^{A(x)} e^{a(x)}y(x) = e^{A(x)} f(x)$
* riconosco che la parte a sinistra e' la derivata di: $e^{A(x)} \, f(x)$
* quindi basta integrare la parte a destra ed eliminare $e^{A(x)}$ moltiplicando per il suo **reciproco** il risultato

**soluzione generale**: $y(x) = e^{-A(x)} \int e^{A(x)} \, f(x) \, dx$

da qui viene il **problema di cauchy**:


$$
\begin{cases}
y^I(x) + a(x)y(x) = f(x) \\
y(x_{0}) = y_{0}
\end{cases}
$$
viene imposta una condizione da rispettare, che ci porta alla ricerca del valore di $c$ (costante di integrazione) t.c. valga la condizione

esempio:
$$
\begin{cases}
y^I(x) - \frac{y(x)}{e^x-1} = e^x\\
y(0) = -1
\end{cases}
$$
calcolo una primitiva di $a(x)$:
$$A(x) = -\int \frac{1}{e^x-1} \, dx = \log(1+e^{-x})$$
ossia:
$$
e^{A(x)} = 1+e^{-x}
$$
quindi:
$$
\int e^{A(x)} f(x) \, dx = \int  (1+e^{-x})e^x \, dx = e^x+x+c
$$
la soluzione generale e':
$$
y(x) = \frac{e^x+x+c}{1+e^{-x}}
$$
ponendo la condizione: $y(0) = -1$
ovvero sostituendo la $x$ con $0$ abbiamo che:
$$
-1= \frac{e^0+0+c}{1+1} = \frac{1+c}{2} \to c = -3
$$
quindi la soluzione e':
$$
y(x) = \frac{e^x+x-3}{1+e^{-x}}
$$

### es 1
$$
\begin{cases}
y'(x) = 4x-\frac{2y(x)}{3\sqrt[3]{x  }} \\
y(1) = 5
\end{cases}
$$
> per $x\in (0,+\infty)$

bisogna intanto ricondurre la prima espressione alla forma dell'equazione differenziale:
$$
y'(x) +\frac{2y(x)}{3\sqrt[3]{ x }} = 4x
$$
calcolo $e^{A(x)}$:
$$
\int  \frac{2}{3\sqrt[3]{ x }} \, dx  =\frac{2}{3} \int \frac{1}{\sqrt[3]{ x }} \, dx =\frac{2}{3} \int x^{-1/3} \, dx  = \frac{2}{3}\frac{x^{-1/3+1}}{-\frac{1}{3}+1} = \frac{2x^{2/3}}{2} = x^{2/3}
$$
quindi:
$$
e^{A(x)} = e^{x^{2/3}}
$$
ora devo integrare: $4x e^{x^{2/3}}$ e dividere il risultato per $e^{-A(x)}$
quindi ho che: 
$$
4\int xe^{x^{2/3}} \, dx 
$$
riconosco che x e' simile a una parte della derivata di $e^{x^{2/3}}$, ovvero $e^{x^{2/3}} \frac{2}{3}x^{\frac{2}{3}-1} = \frac{2}{3} e^{x^{2/3}} x^{-1/3}$  ma non posso ricondurmi a quello. procedo per sostituzione.  

con $t = x^{2/3}$ e $x=t^{3/2}$ ho che, integrando per parti per 3 volte ho che :
$$
4\int t^{2}e^t \, dx = 6e^t(t^2-2t+2) + c
$$
ovvero:
$$
= 6\,e^{x^{2/3}}(x^{4/3} - 2x^{2/3} + 2) + c
$$
moltiplicando per il reciproco ho che la soluzione generale e':
$$
= 6(x^{4/3}-2x^{2/3}+2) + c e^{-x^{2/3}}
$$
ora ponendo la condizione: $y(1) = 5$
$$
6(1-2+2) + c \frac{1}{e} = 5 \to 6+c \frac{1}{e} = 5 = 6e+c = 5e
$$
quindi $c=-e$, sostituendo nella soluzione generale ho:
$$
= 6(x^{4/3}-2x^{2/3}+2) - e^{1-x^{2/3}}
$$


# calcolo differenziale in piu variabili

intorno di un punto $(x_{0}, y_{0})$ di raggio $r>0$:
$$
B_{r} (x_{0},y_{0}) = \{(x,y): \sqrt{ (x-x_{0})^2+ (y-y_{0})^2} < r\}
$$

massimo relativo:
$$
\exists r>0: \forall(x,y) \in B_{r}(x_{0},y_{0})\cap D \; f(x,y)\leq f(x_{0},y_{0})
$$
minimo relativo:
$$
\exists r > 0: \forall(x,y) \in B_{r}(x_{0},y_{0}) f(x,y) \geq f(x_{0}, y_{0})
$$

derivata parziale rispetto a x: (applico $h$ su $x$)
$$
\frac{\partial f}{\partial x}(x_{0},y_{0}) =f_{x}(x_{0},y_{0}) = \lim_{ h \to 0 } \frac{f(x_{0}+h,y_{0})-f(x_{0},y_{0})}{h} 
$$
derivata parziale rispetto a y: (applico $h$ su $y$)
$$
\frac{\partial f}{\partial y} (x_{0},y_{0})= f_{y}(x_{0},y_{0}) = \lim_{ h \to 0 }  \frac{f(x_{0},y_{0}+h)-f(x_{0},y_{0})}{h}
$$
gradiente (**coppia di derivate parziali**):
$$
\triangledown f(x_{0},y_{0}) = \left( \frac{\partial f}{\partial x} (x_{0},y_{0}), \frac{\partial f}{\partial y} (x_{0},y_{0}) \right) = (f_{x}(x_{0},y_{0}), f_{y}(x_{0},y_{0}))
$$
>[!note] punto critico
>$\triangledown f(x_{0},y_{0}) = (0,0)$

equazione **piano tangente**: (simile alla **retta tangente** $y = f(x_{0}) + f^I(x_{0})(x-x_{0})$)
$$
z = f(x_{0},y_{0}) + f_{x}(x_{0},y_{0})(x-x_{0}) + f_{y}(x_{o},y_{0})(y-y_{0})
$$
> [!note] 
> - nel calcolo di $f_{x}$, $y$ e' una costante
> - nel calcolo di $f_{y}$, $x$ e' una costante

**punto di sella**:
$$
f(x_{1},y_{1}) \frac{>}{<} f(x_{0},y_{0})
$$

**matrice hessiana**:
$$
H_{f}(x_{0},y_{0}) =\left[\begin{array}{cc}
f(x_{0},y_{0})_{x x} & f_{xy}(x_{0},y_{0}) \\
f(x_{0},y_{0})_{yx} & f_{yy}(x_{0},y_{0})
\end{array}\right]
$$
**determinate**:
$$
\det(H_{f}) = f_{x x} f_{y y} - f_{x y}f_{y x}
$$
* **massimo** relativo: $\det (H_{f}) > 0 \text{ e } f_{x x} > 0$ 
* **minimo** relativo: $\det(H_{f}) > 0 \text{ e } f_{x x}<0$
* **punto** di sella: $\det(H_{f}) < 0$

