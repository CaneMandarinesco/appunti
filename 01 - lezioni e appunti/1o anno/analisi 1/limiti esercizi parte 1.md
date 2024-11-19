## forme 0/0 con radici
### es 1
$$
\lim_{ x \to 5 } \frac{\sqrt{ x-1 }-2}{x-5} = \frac{0}{0}
$$
tutti gli esercizi di forma simile a questa si svolgono razionalizzazione
$$
\lim_{ x \to 5 } \frac{\sqrt{ x-1 }-2}{x-5}\frac{\sqrt{ x-1 }+2}{\sqrt{ x-1 }+2} = \lim_{ x \to 5 } \frac{x-1-4}{(x-5)(\sqrt{ x-1 }+2)} = \lim_{ x \to 5 } \frac{x-5}{(x-5)(\sqrt{ x-1 }+2)}
$$
quindi semplificando:
$$
\lim_{ x \to 5 } \frac{1}{\sqrt{ x-1 }+2} =\frac{1}{4}
$$

### es 2
$$
\lim_{ x \to 0 }\frac{\sqrt{ 1+x }-1}{x} = \frac{0}{0}
$$
usando il limite notevole della potenza del binomio $1+x$:
$$
\lim_{ x \to 0 } \frac{(1+x)^b-1}{x} = b
$$
abbiamo che:
$$
\lim_{ x \to 0 }\frac{\sqrt{ 1+x }-1}{x} = \frac{(1+x)^{1/2}-1}{x} = \frac{1}{2}
$$

### es 3
$$
\lim_{ x \to 2 } \frac{\sqrt[3]{1+(x-2)}-1 }{x-2} = \frac{1}{2}
$$
> $x-2$ per $x\to 2$ vale $0$ quindi si puo usare il limite notevole

### es 4
$$
\lim_{ x \to 0 } \frac{\sqrt{ 1+x+x^2 } -1}{x} = \frac{0}{0}
$$
fattorizzando $x^2+x$ e moltiplicando per $\frac{x+1}{x+1}$, possiamo ricondurci al limite notevole del binomio
$$
= \lim_{ x \to 0 } \frac{\sqrt{ 1+x(x+1) } -1}{x} = \lim_{ x \to 0 } \frac{\sqrt{ 1+x(x+1) } -1}{x} \frac{x+1}{x+1}
$$
$$
= \frac{1}{2}\lim_{ x \to 0 } x+1 = \frac{1}{2} 
$$
### es 5
$$
\lim_{ x \to 0 } \frac{\sqrt[3]{ 2+x }-\sqrt[3]{2}}{x} = \frac{0}{0}
$$
#### con limiti notevoli: 
$$
\lim_{ x \to 0 } \frac{\sqrt[3]{2\left( 1+\frac{x}{2}\right)} - \sqrt[3]{ 2 }}{x} = \lim_{ x \to 0 } \frac{\sqrt[3]{ 2 }\left( \sqrt[3]{ 1+\frac{x}{2}} \right) - 1)}{\frac{x}{2} 2} = \lim_{ x \to 0 } \sqrt[3]{ 2 } * \frac{1}{2} \frac{1}{3} = \frac{\sqrt[3]{ 2 }}{6}
$$
> si può risolvere anche razionalizzando ma e' più complicato

## forme 0/0 con funzioni trascendenti
> si risolvo con i limiti notevoli di sen, cos, tan ecc...
### es 1
$$
\lim_{ x \to 0 } \frac{\sin(2x)}{tg(x)} = \frac{0}{0}
$$
usando:
$$
\lim_{ x \to 0 }  \frac{\sin(x)}{x} = 1 \text{ e } \lim_{ x \to 0 } \frac{tg(x)}{x} = 1
$$
abbiamo che:
$$
\lim_{ x \to 0 } \frac{\sin(2x)}{tg(x)} \frac{2x}{2x} = \frac{\sin(2x)}{2x} \frac{x}{tg(x)}2 = 2
$$
### es 2
$$
\lim_{ x \to 0 } \frac{\sin^2(x)}{tg(x)} = \frac{0}{0} 
$$
$$
\lim_{ x \to 0 } \frac{(\sin x)^2}{tg(x)}  = \left( \frac{\sin(x)}{x}x \right)^2 \frac{1}{tg(x)}=\left( \frac{\sin x}{x} \right)^2 \frac{x}{tg(x)} x = 1 * 1 * x = 0
$$

### es 3

$$
\lim_{ x \to 0 } \frac{1-\cos^3(x)}{x\sin(2x)} = \frac{0}{0}
$$
usando: $a^3-b^3 = (a-b)(a^2+ab+b^2)$ ovvero $(a-b) \text{ che moltiplica il falso quadrato}$
$$
\lim_{ x \to 0 } \frac{2x}{\sin(2x)} \frac{(1-\cos(x))(1+\cos (x)+\cos^2(x)))}{x (2x)} = 1 * 3 \frac{1-\cos(x)}{x^2} \frac{1}{2}= \frac{3}{4}
$$
> nota: $(1+\cos (x)+\cos^2(x)) = 3 \text{ per } x\to_{0}$

### es 4
$$
\lim_{x \to 0^+ } \frac{\sqrt{ 1-\cos (x) }}{x} =\frac{0}{0}
$$
$$
\lim_{ x \to 0^+ } \frac{\sqrt{ 1-\cos (x) }}{\sqrt{ x }} \frac{1}{\sqrt{ x }} = \sqrt{ \frac{1-\cos(x)}{x} \frac{x}{x}} \frac{1}{\sqrt{ x }}   = \sqrt{ \frac{1-\cos(x)}{x^2} x}\text{ } \frac{1}{\sqrt{ x }}  = \sqrt{ \frac{1}{2}x } \frac{1}{\sqrt{ x }} = \sqrt{ x } \frac{1}{\sqrt{ x }}\sqrt{ \frac{1}{2} } = \sqrt{ \frac{1}{2} }
$$

### es 5
$$
\lim_{ x \to 0^+ } \frac{e^{2x} -1}{3x} = \frac{0}{0} 
$$
$$
\lim_{ x \to 0^+ } \frac{\frac{e^{2x}-1}{2x}2x}{3x} = \frac{2x}{3x} = \frac{2}{3}
$$
### 0/0 con cambio di variable
### es 1
$$
\lim_{ x \to e } \frac{lgx -1 }{x-e} = \frac{0}{0}
$$
$$
= \lim_{ t \to 0 } \frac{lg(t+e) -1}{t+e-e} = \frac{lg(t+e)-1}{t} = \frac{lg\left( e\left( 1+\frac{t}{e} \right) \right) -1}{t} = \frac{lg(e) + lg\left( 1+\frac{t}{e}\right) -1 }{t}
$$
$$
= \frac{lg\left( 1+\frac{t}{e} \right)}{\frac{t}{e}} \frac{1}{e} + \frac{\lg(e)-1}{t} = 1 \text{ }\frac{1}{e} + 0
$$
> nota: $lg(e) = 1$

### es 2
$$
\lim_{ x \to 1 } \frac{e^x-e}{x-1} =\frac{0}{0}
$$
$$
= \lim_{ t \to 0 }  \frac{e^{x+1} - e}{x} = e \frac{e^x-1}{x} = e * 1 = e
$$
### es 3
$$
\lim_{ x \to \frac{\pi}{2} } \frac{3\sin^2x+\sin x-4}{\left( x-\frac{\pi}{2} \right)^2} = \frac{0}{0} 
$$
usando $y=\sin x$
$$
3\sin^2x+\sin x-4= 3y^2+y-4 \to y_{1} = 1 \text{ e } y_{2} = -\frac{4}{3}
$$
$$
\lim_{ x \to \frac{\pi}{2} } \frac{(\sin x-1)\left( 3\sin x + 4 \right) }{\left( x-\frac{\pi}{2} \right)^2} = \lim_{ t \to 0 }  \frac{\left( \sin\left( t+\frac{\pi}{2} \right)-1 \right)\left( 3\sin\left( t+\frac{\pi}{2} \right) + 4 \right) }{t^2}
$$
$$
\lim_{ t \to 0 }\frac{ (\cos(t)-1)(3\cos(t) +4)}{t^2} = \frac{1}{2}(3+4) = \frac{7}{2}
$$

> nota: $\sin\left( \frac{\pi}{2} +x \right) = \cos(x)$

### forma $\infty - \infty$
### es 1
$$
\lim_{ x \to \infty } \frac{e^{x-2}-\sqrt{ x }}{x^2} = \frac{\infty-\infty}{\infty}
$$
$$
\lim_{ x \to \infty } \frac{e^{x(1-2/x)}-\sqrt{ x }}{x^2  } = \lim_{ n \to \infty } \frac{e^x -\sqrt{ x }}{x^2} = \frac{e^x\left( 1-\frac{\sqrt{ x }}{x} \right)}{x^2} = \frac{e^x}{x^2} = +\infty
$$
> nota: $e^{x(1-2/x)}$ quando $x\to \infty$ diventa $e^x$

### es 2

$$
\lim_{ x \to \infty } x^2 - \sqrt{ x^3-1 } = \infty - \infty
$$

$$
\lim_{ x \to \infty } x^2 - \sqrt{ x^3\left( 1-\frac{1}{x^3} \right) } = x^2 - x^{3/2}\sqrt{ 1 } = x^2(1-\frac{x^{3/2}}{x^2}) = x^2 = \infty
$$

### es 3: non si può risolvere raccogliendo
$$
\lim_{ x \to \infty } 3x+1 - \sqrt{ 9x^2 + 6x+2 } = \infty-\infty
$$
$$
\lim_{ x \to \infty } (\sqrt{ (3x+1)^2 } - \sqrt{ 9x^2+6x+2 }) \frac{\sqrt{ (3x+1)^2 } + \sqrt{ 9x^2+6x+2 } }{\sqrt{ (3x+1)^2 } + \sqrt{ 9x^2+6x+2 } }
$$
$$
= \frac{(3x+1)^2 - 9x^2 - 6x - 2}{\sqrt{ (3x+1)^2 } + \sqrt{ 9x^2+6x+2 }} = \frac{9x^2+1+6x-9x^2 - 6x - 2}{3x\left( 1+\frac{1}{4x} \right) + \sqrt{ 9x^2\left( 1 + \frac{6x}{9x^2} + \frac{2}{9x^2} \right) }}  
$$
$$
= \lim_{ x \to \infty } \frac{-1}{3x +3x} =\frac{1}{6x} = 0
$$

### es 4
$$
\lim_{ x \to \infty } \sqrt[3]{ (x+1)^2 } - \sqrt[3]{ (x-1)^2 } = \infty - \infty
$$
moltiplico per $(a^2+ab+b^2)$ quindi abbiamo che $(a-b)(a^2+ab+b^2) = (a^3-b^3)$
$$
\sqrt[3]{ (x+1)^2 } - \sqrt[3]{ (x-1)^2 } \frac{\sqrt[3]{(x+1)^4 } + \sqrt{ (x+1)^2(x-1)^2 } + \sqrt[3]{(x-1)^4}}{\sqrt[3]{(x+1)^4 } + \sqrt{ (x+1)^2(x-1)^2 } + \sqrt[3]{(x-1)^4}}
$$
$$
\frac{(x+1)^2 - (x-1)^2}{\sqrt[3]{(x+1)^4 } + \sqrt{ (x+1)^2(x-1)^2 } + \sqrt[3]{(x-1)^4}} 
$$
Al denominatore in tutte e tre le radici il grado massimo e' $x^4$ quindi:
$$
\frac{(x+1)^2 - (x-1)^2}{3x^{3/4}} = \frac{4x}{3x^{4/3}} = 0
$$

## forma $1^\infty$
si risolvono con i limiti notevoli della forma esponenziale
### es 1

$$
\lim_{ x \to \infty } \left( \frac{5+x}{x} \right)^x = 1^\infty
$$
$$
 \lim_{ x \to \infty } \left( \frac{ x\left( \frac{5}{x} +1 \right)}{x} \right)^x = \frac{x^x\left( 1+\frac{5}{x} \right)^x}{x^x} = \left( 1+\frac{5}{x} \right)^x = e^5
$$

### es 2
$$
\lim_{ x \to \infty } \left( \frac{x}{x-7} \right)^x = 1^\infty
$$
$$
\lim_{ x \to \infty } \left( \frac{x-7}{x} \right)^{-x} = \left( 1-\frac{7}{x} \right)^{-x} = (\left( 1-\frac{7}{x} \right)^x)^{-1} = (e^7)^{-1} = e^{-7}
$$

### es 3
$$
\lim_{ x \to \infty } \left( \frac{x^2 +1}{x^2} \right)^{3x-1}
$$
$$
\lim_{ x \to \infty }  \left( \left( \frac{x^2+1}{x^2} \right)^{x^2} \right)^{(3x-1)/x^2} = e^0 = 1
$$

### es 4
$$
\lim_{ x \to \infty } \left( \frac{x+2}{x-1} \right)^{2x} 
$$
#### 1o modo
$$
\lim_{  x \to \infty } \left( \frac{x-1+3}{x-1} \right)^{2x} = \left( \frac{x-1}{x-1} +\frac{3}{x-1}\right)^{2x} = \left( 1+\frac{3}{x-1} \right)^{(x-1) (2x)/x-1}
$$
$$
= e^{3(2x)/x-1} = e^6
$$
> nota: $a^{\alpha\beta} = (a^\alpha)^\beta$

#### 2o modo
$$
\lim_{ x \to \infty } \left( \frac{x+2}{x-1} \right)^{2x} = \lim_{ x \to \infty }\left[ \frac{x\left( 1+\frac{2}{x} \right)}{x\left( 1-\frac{1}{x} \right)}\right]^{2x} = \frac{e^4}{e^{-2}} = e^6
$$

#### 3o modo: divisione tra polinomi
$$
\left( \frac{x+2}{x-1} \right)^{2x}
$$
$$
(x+2):(x-1) = \left( 1+\frac{3}{x-1} \right)^{2x}
$$

### es 5: camuffato
$$
\lim_{ x \to \infty } (1+2x)^{1/x}
$$
$$
= \lim_{ x \to \infty } \left( 1+\frac{2}{\frac{1}{x}} \right)1^{1/x} = e^2
$$

### es 6
$$
\lim_{ x \to \infty } x lg\left( \frac{x^2+3}{x^2+2x+3} \right) = \infty * 0
$$
$$
\lim_{ x \to \infty } lg \left( \frac{x^2+2x+3-2x}{x^2+2x+3} \right)^x  = lg\left( \frac{x^2+2x+3}{x^2+2x+3} - \frac{2x}{x^2+2x+3} \right)^x = lg\left( 1-\frac{2x}{x^2+2x+3} \right)^x = 
$$
$$
= lg\left( 1-\frac{1}{\frac{x^2+2x+3}{2x}} \right)^{x * [(x^2+2x+3)/2x]*[(2x)/x^2+2x+3]} = lg(e^{-2}) = -2

$$

oppure
$$
\lim_{ x \to \infty } lg\left( \frac{x^2+2x+3}{x+3} \right)^{-x} = \dots
$$
> NOTA: l'esponente alle parentisi si riferisce all'argomento, non al risultato del logaritmo