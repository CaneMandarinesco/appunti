> [!note] metodo di esaustione-compressione
> origine del problema del calcolo integrale, usato da **Archimede** per calcolare l'area del cerchio (quadratura del cerchio)

il metodo consiste nell'inscrivere e circoscrivere un cerchio con una figura composta da **poligoni regolari** (triangoli) e di studiare il comportamento di questi due poligoni al variare del numero di lati dei poligoni.
esempio con $N =8$.
![[Pasted image 20240205094107.png]] 
notiamo quindi che l'area del cerchio e' compresa tra quella di questi due poligoni.
abbiamo che ogni triangolo ha $\alpha_{N} = \frac{2\pi}{N}$

quindi la formula:
$$
\lim_{ N \to \infty } s_{N} = A = \lim_{N \to \infty } S_{N}
$$
e' l'interpretazione **moderna** (ovvero usando la notazione dei limiti) che possiamo dare al metodo di Archimede. infatti man mano che aumentiamo il numero  di poligoni, le aree delle due figure tendono a quella del cerchio, di seguito le formule per calcolare $s_{N} \text{ e } S_{N}$.

## formule per: $s_{N} \text{ e } S_{N}$ e comportamento per $N\to \infty$
facendo riferimento a questa immagine:


guardando il triangolo rosso vediamo che:
$$
s_{N} = N \, \frac{1}{2} \, R^2 \, \sin(\alpha_{N}) = \left( \frac{\sin\left( \frac{2\pi}{N} \right)}{\frac{2\pi}{N}} \right)\pi R^2 \to \pi R^2
$$
> [!note] OSS.
> - moltiplichiamo per N perché abbiamo N triangoli
> - $\frac{1}{2}R^2$ e' l'area di un singolo triangolo (dato che i due cateti hanno la stessa lunghezza)
> - $\sin(\alpha)$ ??? boh

---
guardando il triangolo blu vediamo che:
$$
S_{N} = N \, R^2 \, \tan\left( \frac{\alpha_{N}}{2} \right) = \frac{\tan\left( \frac{\pi}{N} \right)}{\frac{\pi}{N}}\pi R^2 \to \pi R^2
$$
> [!note] OSS.
>- manca $\frac{1}{2}$ rispetto a $s_{N}$ perché l'area va moltiplicata per 2 dato che cosi stiamo calcolando l'area di meta' triangolo (guarda l'immagine allegata sopra)
> - abbiamo $\frac{a_{N}}{2}$ perche' il triangolo che stiamo studiando ha un'apertura pari a meta di quella di $s_{N}$
> - tan(a) ??? boh

e per confronto: $A = \pi R^2$

> per ricordare le due formule $s_{n} \text{ e } S_{n}$ basta ricordare il fatto che l'area del cerchio e' $\pi R^2$, e che bisogna applicare i limiti notevoli di sin e tan

# definizione di integrale
Adattiamo il metodo per il calcolo dell'area del grafico di una funzione
$$
T = \{(x,y): x \in [a,b], y \in [0, f(x)] \lor{}[f(x),0] \}
$$
* $[0,f(x)] \text{ se }f(x) \geq 0$ lavagna con pennare
* $[f(x), 0] \text{ se } f(x)\leq 0$
e $f(x)$ e' limitata in $[a,b]$
con questa definizione di $f(x)$ mettiamo in evidenza il fatto che in alcuni punti $f(x)$ puo' essere positiva o negativa, nel calcolo dell'area dobbiamo fare attenzione al segno:
![[Pasted image 20240205103131.png]]

---
definiamo poi (per $N\geq 1$) $\theta$, una suddivisione di $[a,b]$ data dai punti:
$$
a = x_{0}<x_{1}<x_{2}<x_{3}<\dots <x_{n-1} <x_{n} = n
$$
> nota: $a$ **coincide** con $x_{0}$ e $b$ **coincide** con $x_{n}$

graficamente avremo che:
![[Pasted image 20240205103506.png]]e possiamo calcolare l'area **inscritta** (inferiore) e **circoscritta** (superiore)
>[!note] somma inferiore
>$$
>s(f, \theta) = \sum_{k=1}^{N}m_{k} (x_{k} - x_{k-1}) 
>$$
> dove: $m_{k} = inf\{f(x): x\in [x_{k-1}, x_{k}]\}$ ovvero il valore minimo di $f(x)$ in per esempio: $[x_{0}, x_{1}]$  
> nota: cosi calcolo l'area al di sotto della linea rossa

> [!nota] somma superiore
> $$
> S(f,\theta) = \sum_{k=1}^{N} M_{k}(x_{k} - x_{k-1})
> $$
> dove: $M_{k} = sup\{f(x): x\in[x_{k-1}, x_{k}]\}$

abbiamo quindi che: $s(f, \theta) \leq S(f, \theta)$.
### analisi delle suddivisioni di $\theta$
guardando questa figura notiamo che:
![[Pasted image 20240205105334.png]]
per $s$ abbiamo che:
$$
s(f,\theta)\leq s(f, \theta \cup{} x)
$$
> dove: $\theta$ corrisponde alla suddivisione in $x_{k-1} \text{ e } x_{k}$

invece per $S$ abbiamo che:
$$
S(f, \theta) \geq s(f, \theta \cup{}x)
$$
---
per capire meglio:
![[Pasted image 20240205105840.png]]

---
## definizione di integrale per riemann-darboux
proseguendo il discorso abbiamo che: $sup\{s(f,\theta_{1}): \theta_{1}\text{ suddivisione di [a,b]}\} \leq inf\{s(f,\theta_{2}): \theta_{2}  \text{ suddivisione di [a,b]}\}$
ovvero che il **valore superiore** della divisione per $\theta_{1} = x_{0}<x_{1} \to a<b$ e' minore dell **valore inferiore** della divisione per $\theta_{2} = x_{0}<x_{1}<x_{2} \to a<x_{1}<b$
   
e secondo ***RIEMANN-DARBOUX*** se $sup = inf$ loro valore comune e' definito come: $\int _{a}^{b} f(x) \, dx$

## teorema
se f e' continua in $[a,b]$ allora e' integrabile in $[a,b]$

## osservazioni
### 1. modifica di un numero finito di punti
$f(x)$ se e' integrabile e viene modificata in un **numero finito di punti** e' ancora integrabile (in $[a,b]$)

### 2. inversione degli estremi e estremi uguali
se $f(x)$ e' integrabile in $[a,b]$ allora l'integrale da $a \text{ a } b$ e' uguale all'opposto dell'integrale con intervalli opposti (da $b \text{ ad } a$)

### 3. additività dell'integrale
il valore dell'integrale in $[a,b]$ e' uguale alla somma degli integrali in $[a,c]$ e $[c,b]$

### 4. linearità
due funzioni integrate possono essere spezzate in due integrali diversi, e le costanti moltiplicative possono essere portate fuori dall'integrale

### 5. monotonia e modulo
siano f e g integrabili in $[a,b]$ e tali che $f(x) \leq g(x)$, allora la stessa relazione vale per i loro integrali.
inoltre vale che:
$$
|\int_{a}^{b} f(x) \, dx| \leq \int _{a}^{b}|f(x)| \, dx 
$$

### 6. forma equivalente dell'integrale
$$
\int _{a}^{b}f(x)dx \, dx = \lim_{ N \to \infty } \frac{b-a}{N} \sum_{k=1}^{N}f\left( a+ \frac{k}{N}(n-a) \right) 
$$
dove $x_{k}-x_{k=1} = \frac{b-a}{N}$
