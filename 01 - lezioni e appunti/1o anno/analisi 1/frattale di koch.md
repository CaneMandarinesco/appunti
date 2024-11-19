* si costruisce **ricorsivamente**
* **passo base** $n=0$, si traccia un segmento di **lunghezza 1**
* **passo ricorsivo** $n \geq 1$: si divide ogni lato del passo $n-1$ in 3 **parti uguali** e si pone sulla parte centrale un **triangolo equilatero**.
Quanto vale $\lim_{ n \to \infty } S_{n}$? (con $S_{n} = \text{area dell'insieme}$ al passo n)

![[Pasted image 20240215173941.png]]

quindi, per ogni passo:
* $n=0$, ho un segmento, dunque $S_{0} = 0$
* $n=1$, 1 triangolo di lato $\frac{1}{3}$, $S_{1} = S_{0} + 4$