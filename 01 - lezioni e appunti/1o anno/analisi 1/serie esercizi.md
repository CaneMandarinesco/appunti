### es 1
$$
\sum_{n=2}^{\infty} \frac{n}{e^n}
$$
il termine generale della serie e' infinitesimo.  
posso applicare il **criterio della radice**:
$$
\lim_{ n \to \infty } \sqrt[n]{| \frac{n}{e^n}|} = \lim_{ n \to \infty } \frac{\sqrt[n]{ n}}{e} = \frac{n^{1/n}}{e} = \frac{e^{\frac{1}{n} \log n}}{e} = \frac{1}{e} < 1 \text{ quindi converge}
$$
si poteva anche fare confrontando con la serie: $\frac{1}{e^n} \text{ che e' t.c.} > \frac{n}{e^n}$ 
infatti la serie $\frac{1}{e^n}$ e' una serie geometrica di **ragione**: $\frac{1}{e}$

### es 2
$$
\sum_{n=1}^{\infty} \left( \frac{n}{2} \log\left( \frac{n+1}{n} \right)\right)^n
$$
e' una serie a caratteri positivi? si.
il limite del termine generale e' infinitesimo?
$$
\lim_{ n \to \infty } \left( \frac{n}{2} \log\left( \frac{n+1}{n} \right)\right)^n = \left( \frac{\log\left( \left( 1 + \frac{1}{n} \right)^n \right)}{2} \right)^n = \left( \frac{\log(e)}{2} \right)^n = \lim_{ n \to \infty }  \frac{1}{2^n} = 0 
$$
verificata la convergenza applico il criterio della radice: 
$$
\lim_{ n \to \infty } \sqrt[n]{ \left( \frac{n}{2}\log \left( \frac{n+1}{n} \right) \right)^n } = \frac{n}{2}\log \left( \frac{n+1}{n} \right) = \frac{1}{2} < 1 \text{ quindi converge }
$$
oppure posso usare il confronto asintotico avendo, svolto in precedenza il limite so che:
$$
\left( \frac{n}{2} \log\left( \frac{n+1}{n} \right)\right)^n \sim \frac{1}{2^n}
$$
e $\frac{1}{2^n}$ e' una serie geometrica di ragione < 1 quindi converge

### es 3
$$
\sum_{n=1}^{\infty} \sqrt[n]{n+1} - \sqrt[n]{ n }
$$
e' una serie a termini positivi.
studio il limite:
$$
\lim_{ n \to \infty } (\sqrt[n]{n+1} - \sqrt[n]{ n } = \sqrt[n]{ n }\left( \left( 1+\frac{1}{n} \right)^{\frac{n}{n^2}} -1\right) = \sqrt[n]{ n }\left( e^{\frac{1}{n^2}} -1  \right) = \sqrt[n]{ n } \,\frac{n^2\left( e^{\frac{1}{n^2}} -1  \right)}{n^2} = \frac{\sqrt[n]{ n }}{n^2} = n^{\frac{1}{n}-2} =
$$
svolgendo con i limiti notevoli mi viene $\frac{1}{n^2}$, quindi 0.

per confronto asintotico con: $\sum_{n=1}^{\infty} \frac{1}{n^2}$ , la serie data converge
### es 4 
$$
\sum_{n=1}^{\infty} \sqrt[3]{n+1} - \sqrt[3]{ n } 
$$
$$
$$
$$
\lim_{ n \to \infty } \sqrt[3]{n+1} - \sqrt[3]{ n }= \sqrt[3]{ n\left( 1+ \frac{1}{n} \right) } = \sqrt[3]{ n }\left( \left( 1+\frac{1}{n} \right)^{ \frac{n}{3} \frac{1}{n} } -1 \right) = \sqrt[3]{ n }(e^{1/3n} -1 ) = \sqrt[3]{ n }\frac{(e^{1/3n}-1)}{3n}3n
$$
$$
= 3n^{1/3 - 1} = \frac{1}{3}n^{-2/3} = \frac{1}{3} \frac{1}{n^{2/3}} \to 0
$$
converge a 0, e quindi per confronto asintotico con : 
$$
\sum_{n=1}^{\infty} \frac{1}{n^{2/3}} 
$$
e' una serie armonica con esponente $\leq 1$ quindi diverge
### es 5
$$
\sum_{n=1}^{\infty} \sqrt{ n }\, lg \frac{2n^2+3}{2n^2+2}
$$
e' a termini positivi? si
studio il limite:
$$
\lim_{ n \to \infty } \sqrt{ n }lg \left( \frac{2n^2+3}{2n^2+2} \right) = \frac{\lg\left( \frac{2n^2 \left( 1+\frac{3}{2n^2} \right)}{2n^2\left( 1+\frac{2}{2n^2} \right)} \right)}{\frac{1}{\sqrt{ n }}} = \frac{0}{0}
$$
potrei continuare con Hospital oppure provo a sviluppare il logaritmo:
$$
= \lim_{ n \to \infty }\log \left(2 \left(1 + \frac{3}{2n^2}  \right)^{n/2}\right) - \log \left( 2 \left( 1+\frac{1}{n^2} \right) \right)  = \lim_{ n \to \infty } 2e^{3} - 1
$$

## leibnitz (serie a segno alterno)
### es 1
$$
\sum_{n=1}^\infty \frac{(-1)^{n-1} \,n}{n^2+1} 
$$

$$
= -\sum_{n=1}^\infty \frac{(-1)^n(-1)^{-1} \, n}{n^2+1} = -\sum_{n=1}^\infty \frac{(-1)^n \, n}{n^2+1}
$$
considero la serie $|a_{n}|$:
$$
\sum_{n=1}^\infty |a_{n}|=\sum_{n=1}^\infty \frac{n}{n^2+2}
$$
per confronto asintotico: 
$$
x\to \infty \; \sum_{n=1}^\infty|a_{n}| \sim \sum_{n=1}^\infty\frac{1}{n} \to +\infty
$$
non possiamo dire con certezza (in generale quando il risultato di un teorema/criterio e' $\infty$), che per le serie a segno alterno il risultato sia assolutamente divergente. bisogna usare il criterio di Leibniz

--- 
allora applico il criterio di Leibniz:
* il $\lim_{ n \to \infty } a_{n} = 0$ (teorema: la successione limitata $(-1)^n$ moltiplica per una successione infinitesima ha limite infinitesimo.)
* $\frac{n}{n^2+1}$: per studiarne il la crescenza/decrescenza possiamo:
	* considerare la sua $f(x)$ (teorema ponte), calcolarne la derivata e controllare il risultato.
	* oppure dimostrandolo (per esempio per induzione)

seguendo uno dei due modi abbiamo che: $a_{n} > a_{n+1}$

quindi la serie **converge**

### es 2
$$
\sum_{n\geq 1}^\infty (-1)^n \sin\left( \frac{1}{n} \right)
$$
considero $|a_{n}|$ -> $\sum_{n=1}^\infty \sin\left( \frac{1}{n} \right) \sim \sum_{n=1}^\infty \frac{1}{n} = \infty$ non possiamo affermarlo con certezza quindi uso il criterio di Leibniz
sapendo che:
$$
x\to \infty: \; 0\leq\sin\left( \frac{1}{n} \right)\leq 1
$$
ricordando il grafico di $f(x) = \sin(x)$:
![[Pasted image 20240209113524.png]]
abbiamo che la funzione e' **decrescente** per $x\to 0$ quindi la serie e' **convergente**

### es 3
$$
\sum_{n=1}^\infty (-1)^n \frac{1}{\sqrt{ n} +2}
$$

considero come prima per i termini assoluti:
$$
\sum_{n=1}^\infty |a_{n}| = \sum_{n=1}^\infty \frac{1}{\sqrt{ n }+2} \sim \frac{1}{\sqrt{ n }} = \frac{1}{n^{1/2}} 
$$
dato che $\frac{1}{2}<1$ allora la serie diverge, quindi bisogna applicare Leibniz.

$\frac{1}{\sqrt{ n }+2} > \frac{1}{\sqrt{ n+1 } + 2}$ dato che $\sqrt{ n } >\sqrt{ n+1 }$: allora $a_{n}$ e' decrescente
il limite converge quindi la serie converge.

### es 4
$$
\sum_{n=1}^\infty(-1)^n \left( \frac{2n+100}{3n+1} \right)^n
$$

provo a studiare $|a_{n}|$:

$$
\lim_{ n \to \infty } \left( \frac{2n+100}{3n+1} \right)^n = \frac{(2n)^n\left( 1+\frac{100}{2n} \right)^n}{(3n)^n \left( 1+\frac{1}{3n} \right)^n} =\frac{2^n}{3^n} \frac{e^{50}}{e^{1/3}} = 0
$$
abbiamo che converge **assolutamente** (assolutamente perché si) quindi la serie data converge

## studio al variare di $a$
### es 1 
>[!warning]
>esercizio svolto con lo studio della convergenza del limite, che il tauraso non ha fatto.

per quali $\alpha \in (0,\infty)$ converge la serie?
$$
\sum_{n=1}^\infty \left( \frac{1}{1-\frac{1}{2n^\alpha}} \right)^{n^2}
$$
e' una serie a termini positivi quindi studiamo il limite di $a_{n}$
$$
\lim_{ n \to \infty } \left( \frac{1}{1-\frac{1}{2n^a}} \right) ^{n^2} = \left( 1-\frac{\frac{1}{2}}{n^\alpha} \right)^{-n^2 \frac{n^a}{n^a}} = (e^{-1/2})^{-2n/n^\alpha} = e^{\frac{1}{2} \frac{n^2}{n^\alpha}} = e^{1/2 \, n^{2-\alpha}}
$$
questo limite non e' mai infinitesimo:
* per $\alpha = 2$ abbiamo $e^{1/2}$
* per $\alpha < 2$ abbiamo $+\infty$ (es: $n^{2-0} \text{ per } n\to \infty \text{ e' } \infty$ )
* per $a>2$ abbiamo $1$ (es: $n^{2-4} = n^{-2} \text{ che tende a } 0$ quindi $e^0 = 1$)

quindi non converge mai

altrimenti si puo' svolgere cosi:
$$
\sum_{n=1}^\infty \left( 1-\frac{1}{2n^\alpha} \right)^{-2n} = \exp\left(-2n \log\left( 1-\frac{1}{2n^\alpha} \right) \right) =\exp\left( -2n \log\left( \frac{2n^\alpha - 1}{2n^\alpha} \right) \right) 
$$
### es 2
$$
\sum_{n=1}^\infty \frac{k^\alpha}{(\sqrt[k]{ k} -1)^4}
$$
$\sqrt[k]{ k-1 }$ va riscritto come logaritmo! $= (k)^{1/k} = \exp\left( \frac{1}{k} \log(k)\right) = \exp\left( \frac{\log(k)}{k} \right)$ 
che e' asintotico a: $\exp\left( \frac{\log(k)}{k} \right)$ che a sua volta per $k\to \infty$ e' asintotico a $1+\frac{\log(k)}{k}$ e quindi abbiamo:

$$
\sum_{n=1}^{\infty} \frac{k^\alpha}{(\sqrt[k]{ k} -1)^4} \sim \sum_{n=1}^\infty \frac{1}{k^{-\alpha} \, \frac{\log(k)^4}{k^4}}
$$
ovvero:
$$
\sum_{n=1}^\infty \frac{1}{k^{-a-4} \log(k)^4}
$$
che converge se:
* $-a-4 > 1 \to -a > 5 \to a< -5$
* $-a-4 = 1$ e $4 > 1 \to -a=5 \to a=-5$

quindi converge per: $a\leq-5$

### es 3
con $a>0$
$$
\sum_{k=1}^\infty \frac{\left( 1-\frac{k}{k+1} \right)^{5\alpha}}{\left( \left( \frac{k+1}{k} \right)^\alpha - 1 \right)^{3/2}}
$$

$\sum_{k=1}^\infty a_{n}$ e' asintotico a:
$$
\sum_{k=1}^\infty  \frac{\left( \frac{k+1-k}{k+1} \right)^{5a}}{\left( \left( 1+ \frac{1}{k} \right)^\alpha-1 \right)^{3/2}} = \frac{1}{(k+1)^{-5\alpha} \left( \left( 1+\frac{1}{k} \right)^{\alpha} - 1 \right)^{3/2}} \sim \frac{1}{(k+1)^{-5\alpha} \left( 1+\frac{1}{\alpha k} + 0\left( \frac{1}{k}\right) -1 \right)^{3/2}}
$$

> nota: $\frac{1}{k+1} \sim \frac{1}{k}$ 
> nota: lo sviluppo di $\left( 1+\frac{1}{k} \right)^\alpha$ presenta $\alpha$ al denominatore per via di $\frac{1}{k}$

$$
\alpha^{3/2} \frac{1}{k^{-5a + 3/2}}
$$
quindi converge se $-5a+\frac{3}{2} > 1 \to -5a = -\frac{3}{2} \implies 5a = \frac{3}{2}\to a > \frac{3}{10}$

### es 4 
$$
\sum_{k=1}^\infty \frac{1}{2^{\log(k)}}
$$
e' a termini positivi, decrescente

$$
\lim_{ k \to \infty } \frac{1}{2^{\log(k)}} = 0
$$

$$
\frac{1}{2^{\log(k)}} = \frac{1}{e^{\log(k)\log (2)}} = \frac{1}{k^{\log(2)}}
$$
dove $\log(2) < 1$ quindi converge.

### es 5
$$
\sum_{k=1}^\infty \frac{(k!)^2}{(2k)!}
$$
e' a termini positivi.
il limite diverge

applico il criterio del rapporto:

$$
\lim_{ k \to \infty } \frac{((k+1)!)^2}{(2(k+1))!} \frac{(2k)!}{(k!)^2} = \frac{(k+1)^2 \cancel{k!^2}}{(2k+2)!} \frac{(2k)!}{\cancel{(k!)^2}} = \frac{(k+1)^2}{\cancel{(2k)!} (2k+1)(2k+2)} \cancel{(2k)!}
$$

$$
\frac{k^2+2k+1}{4k^2+6k+2} = \frac{k^2}{k^2} \frac{1 + \frac{k}{k^2} + \frac{1}{k^2}}{4+\frac{6k}{k^2} + \frac{2}{k^2}} = \frac{1}{4} < 1
$$
quindi converge

### es 6
$$
\sum_{k=0 (?1)}^\infty \cos(\pi k)\arctan\left( \frac{1}{k} \right)
$$
$\cos(\pi k)$ si comporta come se fosse $(-1)^k$
* $\cos(\pi) = -1$
* $\cos(2\pi) = 1$
* $\cos(3\pi) = -1$ 
* ecc...
quindi studiamo la convergenza di:
$$
\sum_{k=0 (?1)}^\infty (-1)^k\arctan\left( \frac{1}{k} \right)
$$

$$
\sim \sum_{k=1}^\infty (-1)^k\frac{1}{k}
$$
dunque per leibnitz:
1. $\frac{1}{k}$ e' decrescente
2. $\lim_{ n \to \infty }\frac{1}{k} = 0$ e quindi converge

quindi la serie data converge

### es 7
$$
\sum_{k=1}^\infty k^ke^{-k^2}
$$
e' sicuramente a **termini positivi** e per criterio della radice:

$$
\lim_{ k \to \infty } \sqrt[k]{ k^ke^{-k^2} } = ke^{-k} = e^{\log(k)} e^{-k}= \frac{e^{\log(k)}}{e^k} \to 0 < 1
$$

quindi e' convergente

### es 8 
$$
\sum_{k=1}^\infty \frac{\sin\left( \frac{1}{k} \right)}{\log(k^2+k+1)}
$$

$$
0<\sin\left( \frac{1}{k} \right)<1
$$
quindi provo a studiare:
$$
\sum_{k=1}^\infty \frac{1}{\log(k^2+k+1)} \sim \frac{1}{\log(k^2)} = \frac{1}{2\log(k)}
$$
$\log^\alpha(k)$ con $\alpha=1$ quindi diverge

### es 9
$$
\sum_{k=1}^\infty\left( \frac{\cos^2\left( \frac{1}{k} \right)-\sin^2\left( \frac{2}{k} \right)}{\cos\left( \frac{3}{k} \right)} \right)^{k^3}
$$
> discutere la convergenza della serie

e' a termini positivi?
* il numeratore si dato che abbiamo termini elevati al quadrato
* per il denominatore in teoria no??
* nel dubbio uso il criterio della radice (cosi $(a_{n})^{k^3}$ diventa $(a_{n})^{k^2}$)
$$
\lim_{ k \to \infty } \left( \frac{\cos^2\left( \frac{1}{k} \right) -\sin^2\left( \frac{2}{k} \right)}{\cos\left( \frac{3}{k} \right)}\right)^{k^2}
$$
sviluppo in serie di Taylor, la base della potenza
* $\cos^2\left( \frac{1}{k} \right) = \left( 1-\frac{1}{2k^2} + o\left( \frac{1}{k^3} \right) \right)^2$ = $1- \frac{1}{2k^2} - \frac{1}{2k^2} + \frac{1}{4k^4} + o\left( \frac{1}{k^4} \right)$ = $1- \frac{1}{k^2} + o\left( \frac{1}{k^3} \right)$.
* $\sin^2 = \left( \frac{2}{k}  +o\left( \frac{1}{k^2} \right) \right)^2$ = $\frac{4}{k^2} + o\left( \frac{1}{k^3} \right)$
* $\cos^{-1}\left( \frac{3}{k} \right) = \left( 1- \frac{9}{2k^2} + o\left( \frac{1}{x^3} \right) \right)^{-1}$ = $1+ \frac{9}{k^2} + \left( -\frac{9}{2k^2} \right)^2$ =  $1+ \frac{9}{2k^2} + \frac{36}{4k^2}$

usando: 

$$
\begin{cases}
\cos(x) = 1-\frac{x^2}{2}+\frac{x^4}{4!}+ o(x^4)\\
\sin(x) = x-\frac{x^3}{6}o(x) \\
(1+x)^\alpha = 1 + \alpha x + \frac{\alpha(\alpha-1)}{2}x^2
\end{cases}
$$

quindi sostituendo ho:
$$\left( 1- \frac{1}{k^2} + o\left( \frac{1}{k^3} \right) \right) - \left( \frac{4}{k^2} + o\left( \frac{1}{k^3} \right) \right)\left( 1+ \frac{9}{2k^2} + o\left( \frac{1}{k^2} \right) \right)$$
$$
= \left( 1-\frac{1}{k^2} - \frac{4}{k^2} \right) \left( 1+\frac{9}{2k^2} \right) = \left( 1-\frac{5}{k^2} \right)\left( 1+\frac{9}{2k^2} \right) 
$$
$$
1+\frac{9}{2k^2} - \frac{5}{k^2} + o\left( \frac{1}{k^2} \right) = 1 + \frac{9-10}{2k^2} = 1 -\frac{1}{2k^2}
$$
reinserendo nell'espressione la potenza che avevo tralasciato prima ho che, per $x\to \infty$:
$$
\left( 1-\frac{1}{2k^2} \right)^{k^2} = e^{-1/2}
$$

dove $e^{-1/2} < 1$ quindi per il criterio della radice, la serie data converge.
