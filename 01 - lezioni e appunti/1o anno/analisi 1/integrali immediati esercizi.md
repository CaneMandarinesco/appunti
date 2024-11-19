$$
\int g(f(x))\,f^I(x)\, dx = G(f(x)) + c
$$
ovvero: l'integrale di una funzione $g(f(x))$ moltiplicata per la derivata di $f(x)$ e' uguale alla primitiva di $g(x)$

### es 1 (funzione composta con derivata)
$$
\int \sin^5x \cos x \, dx 
$$
$$
= \int  (\sin x)^5 \cos x \, dx
$$
dato che abbiamo $f(x) = \sin x \text{ e } f^I(x) = \cos x$, ovvero troviamo dentro all'integrale una funzione composta $g(f(x))$, che ha come argomento f(x), e la sua derivata, allora il risultato e': G(x) (la primitiva di g(f(x))). quindi:
$$
= \frac{\sin ^ 6x}{6} + c
$$
**ragionamento all'inverso**: infatti derivando il risultato otteniamo $\sin^5x$ e dato che stiamo derivando una funzione composta comparirà anche la derivata di $\sin x$, ovvero $\cos x$

### es 2
$$
\int e^{\sin x} \cos x \, dx 
$$
come prima riconosco il fatto che abbiamo una funzione composta che moltiplica una derivata e ho:
$$
= e^{\sin x} + c
$$
### es 3
$$
\int \frac{x}{\sqrt{ 9-x^2 }} \, dx 
$$
se lo scrivo come potenza:
$$
= (9-x^2)^{-\frac{1}{2}} x
$$
posso ricondurmi ai casi precedenti riconoscendo che: $f(x) = 9-x^2$, pero' la derivata di $-x^2$ e' $-2x$ che non compare nell'integrale, allora svolgo cosi:
$$
= -\frac{1}{2}\int (9-x^2)^{-\frac{1}{2}} \,-2x \, dx 
$$
dove moltiplico e divido per: $-2$, e quindi ho che:
$$
= -\frac{1}{2} \frac{\sqrt{ 9-x^2 }}{\frac{1}{2}} + c = -\sqrt{ 9-x^2 } + c
$$
### es 4
$$
\int \frac{dx}{\sqrt[3]{ 2x+5 }} =
$$
come prima:
$$
 = \frac{1}{2}\int (2x+5)^{-1/3} \,2dx 
$$
dove qui avremo $f(x) = 2x$ e $f^I(x) = 2dx$ (dove dx essendo la derivata di x vale 1 (???)).
quindi risolvo cosi:
$$
= \frac{1}{2} \frac{(2x+5)^{2/3}}{\frac{2}{3}} + c = \frac{1}{3}\sqrt[3]{ (2x+5)^2 } + c
$$

### es 5
$$
\int \frac{e^{2x}}{\sqrt{ 1-e^{4x} }} \, dx =
$$
qui e' un po camuffato, pero riconosciamo che f(x) ha la forma della derivata di un arcoseno
$$
\int \frac{e^{2x}}{\sqrt{ 1-(e^{2x})^2 }} \, dx =\frac{1}{2}\int \frac{2e^{2x}}{\sqrt{ 1-(e^{2x})^2 }} \, dx = \frac{1}{2} \arcsin(e^{2x}) + c
$$
si poteva risolvere anche per sostituzione.

### es 6
$$
\int \frac{lg^4x}{x} \, dx
$$
$$
= \int (lgx)^4 \, \frac{1}{x} \, dx 
$$
dove $f(x) = lgx$ e $f^I(x) = \frac{1}{x}$, quindi:
$$
= \frac{lg^5x}{5} + c
$$

### es 7
$$
\int \frac{\cos \sqrt{ x }}{\sqrt{ x }} \, dx 
$$
dove $f(x) = \sqrt{ x }$ e $f^I(x) = -\frac{1}{2x^{-1/2}}$ quindi
$$
= 2\int \frac{\cos \sqrt{ x }}{2\sqrt{ x }} \, dx 
$$
infine:
$$
= 2\sin \sqrt{ x } +c
$$
### es 8
$$
\int \frac{dx}{5+2x} 
$$
$$
= \frac{1}{2}\int  \frac{2dx}{5+2x} = \frac{1}{2} lg|5+2x| + c
$$
### es 9
$$
\int \frac{e^x}{e^{2x}+1} \, dx 
$$
$$
= \int \frac{e^x}{(e^x)^2 + 1} \, dx = \arctan(e^x) + c 
$$

### es 10 
$$
\int  \frac{\sin(2x)}{1+\cos^2x} \, dx 
$$
$$
\int \frac{2\sin x\cos x}{1+\cos^2x} \, dx 
$$
e trattando $\cos^2x$ come la nostra f da derivare abbiamo che:
$$
-\frac{1}{2}\int -2\frac{\sin x \cos x}{1+\cos^2x} \, dx = -lg|1+\cos^2x|+c
$$

### es 11
$$
\int  \frac{\log(x)}{x} \, dx 
$$
vedendo $\log(x)$ come $\log(x)^1$
allora $g(f(x)) = \log(x)^1$: 
$$
 = \frac{\log(x)^2}{2} +c
$$

### es 12
$$
\int \frac{e^x}{\sqrt{ 1-e^{2x} }} \, dx  
$$

$$
\int \frac{e^x}{\sqrt{ 1-(e^{x})^2 }} \, dx = \arcsin(e^x) + c
$$
ricordando che: $\frac{x}{dx} \arcsin(x) = \frac{1}{\sqrt{ 1-x^2 }}$ con $x=e^x$

### es 13
$$
\int x\sqrt{ 6-x^2 } \, dx  
$$
$$
\frac{1}{2}\int 2x(6-x^2)^{1/2}  \, dx  = \frac{1}{2} \frac{(6-x^2)^{3/2}}{\frac{3}{2}}
$$
$$
\frac{1}{3}\sqrt{ (6-x^2)^3 } + c
$$

### es 14
$$
\int x(x^2+7)^5 \, dx 
$$
$$
\frac{1}{2}\int 2x(x^2+7)^5 \, dx = \frac{1}{2} \frac{(x^2+7)^6}{6} + c
$$
### es 15
$$
\int \frac{e^{\arctan(x)}}{1+x^2} \, dx = e^{\arctan(x)} + c
$$
### es 16
$$
\int (2x-9)^7 \, dx 
$$

$$
\frac{1}{2}\int 2(2x-9)^7  \, dx = \frac{1}{2} \frac{(2x-9)^8}{8} + c
$$

### es 17
$$
\int  \frac{\cos(\log x)}{x} \, dx 
$$

$$
 = \sin(\log(x)) + c
$$
### es 18
$$
\int  \frac{dx}{1+e^{-x}} 
$$
$$
\int  \frac{1}{1+\frac{1}{e^x}} \, dx = \int  \frac{1}{\frac{e^x+1}{e^x}} \, dx =\int  \frac{e^x}{e^x+1} \, dx = \log(e^x+1)+c 
$$
dato che: $\frac{d}{dx}\log(e^x+1) = \frac{1}{e^x+1} \frac{d}{dx}e^x = \frac{e^x}{e^x+1}$ 

### es 19
$$
\int  \frac{e^\sqrt{ x }}{\sqrt{ x }} \, dx = 
$$

$$
2 \int e^\sqrt{ x } \frac{1}{2\sqrt{ x }} \, dx = 2e^\sqrt{ x } +c
$$
### es 20
$$
\int  \frac{\sin x \cos x}{\sqrt{ 1+\cos^2x}} \, dx 
$$
$$
 = -\frac{1}{2}\int  -2\sin x \cos x (1+\cos^2x)^{1/2} \, dx = -\frac{1}{2} \frac{(1+\cos^2x)^{-1/2\,+1}}{-\frac{1}{2} + 1}
$$

$$
 = -\sqrt{ 1+\cos^2x$ } + c
$$
in quanto: $\frac{d}{dx} \cos^2x = -2\sin(x)\cos(x)$

### es 21
$$
\int \frac{1}{x\sqrt{ \log(x) }} \, dx 
$$

$$
2\int   \, dx \frac{1}{2\sqrt{\log(x) }} \frac{1}{x} \, dx = \sqrt{ \log(x) }
$$

### es 22

$$
\int  \frac{x^2}{1+x^6} \, dx 
$$

$$
= \frac{1}{3}\int  \frac{1}{1+(x^{3})^2} 3x^2 \, dx = \frac{1}{3}\arctan(x^3)
$$