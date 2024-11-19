## asintoto verticale
la retta: $x=x_{0}$ e' un asintoto verticale di $f(x)$ se:
* $\lim_{ x \to x_{0} } f(x) = \pm \infty$ da **destra** o da **sinistra**

## asintoto obliquo
la retta: $y = mx+q$ e' un asintoto obliquo di $f(x)$ se:
* $\lim_{ x \to \infty } f(x)-(mx+q) = 0$

>[!note] OSS.
>$\lim_{ x \to \infty } f(x)-mx = q$
>$\lim_{ x \to \infty } \frac{f(x) + \cancel{q}}{x} = m$

## concavità e convessità
> [!note] TEO. criterio di convessità/concavità
> in un intervallo I se $f(x)$
> 1. convessa -> crescente
> 2. concava -> decrescente

## punto di flesso 
$x_{0}$ e' un punto di flesso se:
* $f(x)$ e' **strettamente convessa/concava** in $(x_{0}-r,x_{0})$
* $f(x)$ e' **strettamente concava/convessa** in  $(x_{0}, x_{0+r})$
* $f(x)$ derivabile in $x_{0}$
quindi bisogna controllare cosa succede a sinistra e a destra del punto $x_{0}$


## punti di discontinuità
### eliminabile
$$
\lim_{ x \to x_{o}^+ } f(x) = \lim_{ x \to x_{0}^- } f(x) \neq f(x_{0})  
$$
il limiti sono uguali ma la funzione assume un'altro valore

### di salto
$$
\lim_{ x \to x_{0}^+ } f(x) =l_{1} \neq l_{2} = \lim_{ x \to x_{0}^- } f(x)  \text{ e } f(x_{0}) = l_{1} \text{ o } f(x_{0}) = l_{2} \text{ oppure }f(x_{0}) = l_{3}
$$
ovvero limite destro e sinistro sono diversi e $f(x)$ puo' coincidere con uno dei due limiti

### non derivabilita
$L_{1}$ e $L_{2}$ si riferiscono a limite destro e sinistro della derivata.
* angoloso: $L_{1} \neq L_{2}$ e almeno uno dei due e' **finito**
* cuspide:  $L_{1} \neq L_{2}$ e entrambi **non finiti**
* flesso a tangente verticale: $L_{1} = L_{2}$ e entrambi **non finiti**