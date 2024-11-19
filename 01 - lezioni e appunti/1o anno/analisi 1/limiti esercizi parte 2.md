## forme $1^\infty$
### es 7
$$
\lim_{ x \to \infty } \left( 1+\frac{x}{2x^2+1} \right)
^{3x} = 1^\infty
$$
$$
\lim_{ x \to \infty } \left( 1+\frac{1}{\frac{2x^2+1}{x}} \right)^{[(2x^2+1)/x][x/(2x^x+1)][3x]} = \exp\left( \frac{x}{2x^2+1}3x \right) = e^{3/2} = \sqrt{ e^3 }
$$

### es 8
$$
\lim_{ x \to \infty } \left( \frac{x^3-x-1}{x^3-5x+6} \right)^x
$$
riscrivendo: $x^3-x-1$ come $x^3-5x+6 + 4x - 7$ spezzo la frazione in due somme e ho che:
$$
= \lim_{ x \to \infty } \left( \frac{x^3-5x+6 + 4x - 7}{x^3-5x+6} \right)^x = \left(1+\frac{4x-7}{x^3-5x+6} \right)^x  = \left( 1+\frac{1}{\frac{x^3-5x+6}{4x-7}} \right)^x
$$

$$
=  \left( 1+\frac{1}{\frac{x^3-5x+6}{4x-7}} \right)^{x[\frac{x^3-5x+6}{4x-7}][\frac{4x-7}{x^3-5x+6}]} = e^{x[\frac{4x-7}{x^3-5x+6}]} = 1
$$
## analisi 1 prof. Samilli. Sapienza
### es 1: riscrivere x come $\frac{1}{\frac{1}{x}}$ 
$$
\lim_{ x \to 0^+ } (1+|\sin x|)^{1/x} = 1^{+\infty} 
$$
le forme: $1^{+\infty}$ si risolvono con le proprietà dell'esponenziale:
$$
= \left( 1 + \frac{1}{\frac{1}{|\sin x}|} \right) ^ { \frac{1}{|\sin x|} * \frac{|\sin x|}{x}} = e^1 = e
$$
### es 2
$$
\lim_{ x \to 0^+ } \frac{\ln(x+x^2)}{\ln(x)} = \frac{\infty}{\infty}
$$
$$
= \lim_{ x \to 0^+ } \frac{\ln x + \ln(1+x)}{\ln x} = 1 + \frac{\ln(1+x)}{\ln x} = 1+\frac{0}{\infty} = 1
$$
ricordando che: $\frac{0}{\infty} = 0 \frac{\,1}{\infty} = 0$
inoltre questo significa che $\ln(1+x)$ e' asintotico a $\ln x$.

### es 3
$$
\lim_{ x \to 0 } \frac{x(\pi^x-e^x)}{\cos(x)-1}
$$
...