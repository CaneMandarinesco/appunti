4un esempio generico e': 
$$
\int  a + b + c \, dx = \int a \, dx + \int  b \, dx + \int  c \, dx   
$$

### es 1
$$
\int  \frac{1+x}{\sqrt{ x }} \, dx
$$
$$
\int  \frac{1}{\sqrt{ x }} + x^{1-1/2} \, dx 
 = \int  x^{-1/2} \, dx + \int  x^{1/2} \, dx  
= \frac{x^{1/2}}{\frac{1}{2}} + \frac{x^{3/2}}{\frac{3}{2}} + c
$$

### es 2

$$
\int  \frac{1}{\sqrt{ x\sqrt{ x } }} \, dx
$$
riscrivo la radice come potenza

$$
 = \int  \frac{1}{x^{1/2}x^{1/4}} \, dx = \int  \frac{1}{x^{3/4}} \, dx = \int  x^{-3/4} \, dx = \frac{x^{1/4}}{\frac{1}{4}} + c
$$
### es 3
$$
\int  \frac{\sqrt{ x }(5+x)+9}{x} \, dx 
$$

$$
\int  \frac{5\sqrt{ x }}{x} + \frac{\sqrt{ x }x}{x} + \frac{9}{x} \, dx = \int \frac{5\sqrt{ x }}{x} \, dx  \int  \sqrt{ x } \, dx +  \int  \frac{9}{x} \, dx 
$$

$$
5\int   \frac{\sqrt{ x }}{x} \, dx = 5 \int  x^{1/2-1} \, dx   = \frac{5x^{1/2}}{\frac{1}{2}} = 10\sqrt{ x } + c
$$

$$
\int \sqrt{ x } \, dx  = \frac{x^{1/2+1}}{\frac{1}{2}+1} = \sqrt{ x^3 } \frac{2}{3} + c
$$
$$
9\int  \frac{1}{x} \, dx = 9\log(x) + c
$$
quindi mettendo insieme tutto:
$$
\int  \dots \, dx =  10\sqrt{ x } + \frac{2}{3}\sqrt{ x^3 } + 9\log(x) + c
$$
### es 4: togli e aggiungi uno!
$$
\int  \frac{x}{x+1} \, dx 
$$
$$
\int  \frac{x+1-1}{x+1} \, dx  = \int  \frac{x+1}{x+1}-\frac{1}{x+1} \, dx = \int  1 \, dx -\int \frac{1}{x+1} \, dx    = x-\log|x+1| + c
$$

### es 5: divisone
$$
\int  \frac{1-x^6+x^2}{x^3+1} \, dx 
$$
$$
= \int  \frac{1-x^6}{x^3+1} \, dx + \int \frac{x^2}{x^3+1} \, dx 
$$

$$ 
\int  \frac{1-x^6}{x^3+1} \, dx =\int \frac{(1-x^3)(1+x^3)}{1+x^3} \, dx = \int  (1-x^3) \, dx 
$$
$$
 = x - \frac{x^4}{4} + c
$$
$$
\int  \frac{x^2}{x^3+1} \, dx = \frac{1}{3}\int  3x^2 \, \frac{1}{x^3+1} \, dx = \log(x^3 + 1) + c
$$

quindi:
$$
x-\frac{x^4}{4}+ \frac{1}{3}\log(x^3+1) + c
$$

### es 6
$$
\int  \frac{3x}{3x+2} \, dx 
$$
$$
\int  \frac{3x+2-2}{3x+2} \, dx  =x -\frac{2}{3}\int \frac{3}{3x+2} \, dx = x-\frac{2}{3\log(3x+2)} + c 
$$

### es 7
$$
\int \frac{x-1}{x+1} \, dx 
$$

$$
\int  \frac{x-2+1}{x+1} \, dx  = x -2\int   \frac{1}{x+1}\, dx = x-2\log(x+1) + c
$$

### es 8
$$
\int  \frac{x^2}{x+1} \, dx 
$$

$$
\int  \frac{x^2 - 1}{x+1} + \frac{1}{x+1} = \int  \frac{(x+1)(x-1)}{x+1} \, dx + \int \frac{1}{x+1} \, dx = \int  x-1 \, dx + \log(x+1) = \frac{x^2}{2} -x  + \log(x+1) + c
$$

### es 9
$$
\int  \frac{\sqrt{ (1-x) }}{\sqrt{ 1+x }} \, dx 
$$

$$
\int \frac{\sqrt{ 1-x }}{\sqrt{ 1+x }} \frac{\sqrt{ 1-x }}{\sqrt{ 1-x }} \, dx = \frac{1-x}{\sqrt{1-x^2}} = \int  \frac{1}{\sqrt{ 1-x^2 }} \, dx -\frac{1}{2}\int \frac{2x}{\sqrt{ 1-x^2 }} \, dx =
$$

$$
\arcsin(x) - \int x (1-x^2)^{-1/2} \, dx = \arcsin(x) - \frac{1}{2}\frac{(1-x^2)^{1/2}}{-\frac{1}{2}} = \arcsin(x)+\sqrt{1-x^2 }
$$

### es 10
$$
\int x(x^3+e^{x^2}) \, dx 
$$
$$
= \int  x^4 \, dx + \frac{1}{2}\int  2xe^{x^2} \, dx   = \frac{x^5}{5} + \frac{1}{2}e^{x^2} + c
$$

### es 11
$$
\int  \frac{x-1}{\sqrt{ x} + 1} \, dx 
$$
$$
\int  \frac{x-1}{\sqrt{ x} + 1} \frac{\sqrt{ x }-1}{\sqrt{ x } - 1}\, dx = \int \frac{(x-1)(\sqrt{ x } -1) }{x-1} \, dx  = \int  \sqrt{ x } -1 \, dx 
$$
$$
\int  \sqrt{ x } -1 \, dx = \frac{x^{3/2}}{\frac{3}{2}} -x + c
$$

### es 12
$$
\int  \frac{dx}{\sqrt{ x+1 }}
$$

$$
 \int  (x+1)^{-1/2} \, dx = 2(x+1)^{1/2} + c
$$
 > razionalizzando  si ritorna all'integrale di partenza

## trigonometrici (non e' utile saperli)
$\sin(2x) = 2\sin x\cos x$
$\cos(2x) = \cos^2x - \sin^2x$
### es 13
$$
\int  \cos^2x \, dx 
$$
si puo' fare solo con le formule di duplicazione (ho provato mezz'ora a farlo senza)
quindi abbiamo che da $1-2\sin^2x = 2\cos^2x-1$ -> (???) $\cos^2x = \frac{1+\cos(2x)}{2}$

$$
\int \frac{1+\cos(2x)}{2} \, dx = \int  \frac{1}{2} \, dx + \int  \frac{\cos(2x)}{2} \, dx  = \frac{1}{2}x + \frac{1}{2} \int  \cos(2x) \, dx = \frac{1}{2}x +\frac{1}{2} + \frac{1}{4}\sin(2x) + c
$$

### es 14

$$
\int  \sin^2x \, dx 
$$
similmente a prima
$$
\int  \frac{1-\cos(2x)}{2} \, dx  = \frac{x}{2} - \frac{1}{2}\int  \cos(2x) \, dx = \frac{x}{2}-\frac{1}{4}\sin(2x) + c
$$
### es 15
$$
\int \tan^2x \, dx 
$$
sapendo che: $\frac{d}{dx}\tan x = \frac{1}{\cos^2x}$
$$
\int  \frac{\sin^2x}{\cos^2x} \, dx =\int \frac{1-\cos^2x}{\cos^2x} \, dx = \int \frac{1}{\cos^2x} \, dx - x = \tan(x)-x 
$$

### ...
