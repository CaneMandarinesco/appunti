Il limite:
$$ 
\lim_{ x \to 0 } \frac{\sin x}{x} = \frac{0}{0}
$$
vale: **1**

la dimostrazione qui sotto non credo si accurata al 100% ma ha comunque senso, anche se viene $\frac{x}{\sin x} \to 1$ dovrebbe essere ok, perche' basterebbe invertire tutto (???). 
> [!note] DIM.
> $\frac{\sin x}{x}$ e' una funzione pari quindi basta studiare per $x\to0^+$  
> costruendo la circonferenza goniometrica si ricava che: 
> - $\frac{x}{2}$ e' l'area di una porzione di cerchio di angolo $\alpha$ (perché si, vale x/2, e non si discute)
> - $\frac{tg(x)}{2}$ e' l'area del triangolo rettangolo esterno che ha per cateto la tangente (dalla formula $\frac{cateto * cateto}{2}$)
> - $\frac{\sin x}{2}$ e' l'area del triangolo scaleno di altezza $\sin x$ calcolato sulla base che vale 1
> 
> quindi possiamo dire ad intuito che: 
> $$
> \frac{\sin x}{2} \leq \frac{x}{2} \leq \frac{\tan}{2} \to \sin x \leq x \leq \tan x
> $$
> ovvero moltiplicando per: $\frac{1}{\sin x}$ e scrivendo $\tan x$ come $\frac{\sin x}{\cos x}$
> $$
> 1 \leq \frac{x}{\sin x} \leq \frac{1}{\cos x}
> $$
> e per confronto per $x \to 0$ si ha che
> $$
> 1 \to 1, \frac{1}{\cos x} \to 1 \to \text{ quindi } \frac{x}{\sin x} \to 1
> $$
