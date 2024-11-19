## definizione di f continua
se $x_{0}$ e' punto di accumulazione e $\lim_{ x \to x_{0} } f(x) = f(x_{0})$ allora si dice che $f(x) \text{ e' continua in } x_{0}$

## teorema degli zeri
Se f e' continua in $[a,b] \text{ e } f(a)f(b)<0$ (ovvero $f(a)$ e $f(b)$ hanno segni discordi) -> $\exists x_{0} \in (a,b): f(x_{0}) = 0$
se f e' continua in un intervallo con segni discorsi (che quindi interseca la retta y=0) allora esiste un $x_{0}$ t.c. $f(x_{0}) = 0$ 
>[!note] DIM.
> caso per: $f(a) < 0 \text{ e } f(b) > 0$  
> utilizziamo un metodo **ricorsivo**, definendo due successioni: $\{a_{n}\}_{n} \text{ e } \{b_{n}\}_{n}$ definite come: $a_{0} = a \text{ e } b_{0} = b$ e dati $a_{n}$ e $b_{n}$ definiamo $c_{n} = \frac{a_{n}+b_{n}}{2}$ (**punto medio**)
>  
> si inizia con $c_{0} = \frac{a_{0}+b_{0}}{2} = \frac{a+b}{2}$ e in generale abbiamo 3 casi possibili:
> - $f(c_{n}) = 0$ -> abbiamo trovato il nostro $x_{0}$
> - $f(c_{n}) > 0$ -> dobbiamo spostarci a sinistra: $a_{k+1} = a_{k}$ e $b_{k+1} = c_{k}$
> - $f(c_{n}) < 0$ -> dobbiamo spostarci a destra: $b_{k+1} = b_{k}$ e $a_{k+1} = c_{k}$
> 
> se non si verifica mai $f(c_{n}) = 0$ vuol dire che:
> - $\forall n \in \mathbb{N} \, a_{n}\leq a_{n+1} < b \text{ e } a < b_{n+1} \leq b_{n}$
> 	- dato che $a_{n}$ e' **crescente** mentre $b_{n}$ e' **decrescente** e sono superiormente/inferiormente limitate l'una dall'altra: $a<b$.
> - $\lim_{ n \to \infty } = sup\{a_{n}, n\geq 0\} = A$
> - $\lim_{ n \to \infty } = inf\{b_{n}, n\geq 0\} = B$
> 
> ora, studiamo cosa succede ad $a_{n} \text{ e } b_{n}$ quando $n\to \infty$.
> possiamo esprimere l'intera ricorsione come: $\lim_{ n \to \infty } b_{n} - a_{n} = \lim_{ n \to \infty }\frac{b-a}{2^n} = 0$
> sappiamo inoltre che $b_{n} \to B$ e $a_{n} \to A$ quindi $a_{n}-b_{n} \to B - A$ e dato che il limite studiato poco fa tende a 0, necessaria mente anche questo tende a 0 e quindi abbiamo che $B=A$.  
> quindi esiste un valore comune $x_{0} \text{ t.c. per un certo n allora } b_{n} = B = A = a_{n} = x_{0}$
> 
> quindi per concludere: esiste un valore comune $x_{0} \text{ t.c. } f(x_{0}) = 0$ ed essendo che:
> - $f(a_{n}) \leq 0$
> - $f(b_{n}) \geq 0$
> l'unica soluzione possibile per entrambe e' che: $f(x_{0}) = 0$
## teorema dei valori intermedi
se $f(x)$ e' continua in $[a,b]$ e $y_{0} \in [f(a),(b)]$ allora $\exists x_{0} \in (a,b): f(x_{0}) = y_{0}$ 
ovvero f(x) assume tutti i valori in $[a,b]$

> ma perche' sugli appunti c'e' scritto 1a parte. e non c'e' una seconda?