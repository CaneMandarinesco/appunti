
* [materiale tauraso](https://www.mat.uniroma2.it/~tauraso/aa2324/an1-lezione27.pdf)
* [video](https://www.youtube.com/watch?v=dF_hyMKu_tA)
* definizione: 
$$
\sum_{k=k_0}^{\infty} a_{k} = \lim_{ n \to \infty } \sum_{k=k_{0}}^{n} a_{k} 
$$
dove: 
* $a_k$ e' il termine generico
* $\sum_{k=k_{0}}^{n} a_{k}$ e' la somma parziale
il risultato può:
* convergere
* divergere a $\pm \infty$
* essere indeterminato 

Esempi:
$$
\text{(1)  }\sum_{k=1}^{\infty} \frac{1}{10^k} = 0.\overline{1} = \frac{1}{9}
$$
$$
\text{(2)  }\sum_{k=1}^{\infty} 1 = \lim_{ n \to \infty } \sum_{k=1}^{n} 1 = \lim_{ n \to \infty } n = +\infty 
$$
$$
\text{(2)  }\sum_{k=1}^{\infty} (-1)^k = \lim_{ n \to \infty } \sum_{k=0}^{n}(-1)^k = \lim_{ n \to \infty } S_{n} = \nexists
$$
### serie telescopica

^d04ed2

$$
S_{n} = \sum_{k=k_{0}}^{n} b_{n}
$$
dove:
$$
b_{n} = a_{n} - a_{n+1}
$$
quindi i termini generati dalla serie si semplificano e **sopravvivono** solo $a_{k_{0}} \text{ e } a_{n+1}$,  la sua somma si calcola cosi:
$$
S_{n} = \sum_{k=k_{0}}^{n} a_{n} - a_{n+1} = a_{k_{0}}- a_{n+1}
$$

**esempio**: 
$$
S_{n} = \sum_{n=1}^{\infty} \frac{1}{n(n+1)} =\frac{1}{n} - \frac{1}{n+1} = 1- \frac{1}{n+1}
$$
trasformando la frazione (usando i **fratti semplici**), la serie ha assunto la forma di una serie telescopica

### serie geometrica
$$
S_{n} = \sum_{k=0}^{n} x^k
$$
vale:
- $n+1$ se $x=1$
- $\frac{1-x^{n-1}}{1-x}$ se $x \neq 1$ (per ricavarlo basta notare che facendo la somma parziale fino ad $n$ e moltiplicando per $(1-x)$ si ottiene il numeratore, quindi basta dividere per $(1-x)$ per ottenere il valore originario) 
e il limite di $a_{n}$ vale (ricordando che nel limite il valore che varia e' $n$):
* $\frac{1}{1-x}$ se $|x| < 1$ perché $x^{n-1} \to 0-$
* $+\infty$ se $x\geq1$
* $\nexists$ se $x \leq -1$
### serie armonica
$$
\sum_{k=1}^{\infty} \frac{1}{k} = +\infty
$$

questo e' un contro esempio per la condizione **necessaria** per la convergenza. in questo caso il fatto che $\frac{1}{k} \to 0$ non e' causa sufficiente per affermare che la serie diverga.
**interpretazione geometrica**: area di un rettangolo

> nota che:
$$
\sum_{n=1}^{\infty} \frac{1}{n} = +\infty \text{ e }\sum_{n=1}^{\infty} \frac{1}{n^2} = \frac{\pi^2}{6}
$$

> la convergenza/divergenza delle serie e' determinata dalla loro velocità
#### dim. che la serie di $\frac{1}{n}$ diverge
$$
S_{n} = 1+(\frac{1}{2}+\frac{1}{3})+(\frac{1}{4}+\frac{1}{5} + \frac{1}{6}+\frac{1}{7}) + \dots \geq T_{n} = 1 + \frac{1}{2}+\frac{1}{2}+ \dots
$$
quindi $S_{n} > T_{n}$ e $T_{n} = +\infty$ . Allora per **confronto** $S_{n}  = +\infty$

### serie armonica generalizzata
$$
\sum_{n=1}^{\infty} \frac{1}{n^\alpha}
$$
* la successione converge se $\alpha \leq 1$
* la successione diverge se $\alpha > 1$

## criterio necessario per la convergenza
se una successione converge -> il termine generale $a_{n}$ tende a 0
$$
\sum_{n=n_{0}}^{\infty}a_{n} \text{ converge} \to \lim_{ n \to \infty } a_{n} \text{ converge}
$$
quindi quando dobbiamo determinare la convergenza di una serie, il primo passo sara' studiare il limite del suo termine generale. se il limite **diverge** allora siamo sicuri che la successione **diverge**. lo stesso ragionamento non vale per la convergenza (implicazione)

### dim.
direttamente dall'ipotesi, abbiamo che $\sum_{n=n_{0}}^\infty a_{n}$ converge, e per esempio:
$$
\sum_{k=0}^n a_{n} - \sum_{k=0}^{n-1} a_{n} = a_{n}
$$
ora le due serie convergono entrambe a $L$ (dato che essendo entrambe estratte dalla stesa serie, allora hanno stesso limite) quindi $L-L=0$ e quindi anche $a_{n} \to 0$

## criterio del confronto
date due successioni $a_{n} \text{ e } b_{n}$ t.c: $0 \leq a_{n} \leq b_{n}$
* se $a_{n}$ diverge allora $b_{n}$ diverge
* se $b_{n}$ converge allora $a_{n}$ converge

## criterio del confronto integrale
sia $f(x)$ continua  e **decrescente** in $[1,+\infty]$ e $a_{n} = f(x)$ ->
la serie di $a_{n}$ converge se e solo se converge $\int_{1}^{+\infty} f(x)\, dx$

## criterio del confronto asintotico
se $a_{n} \sim L * b_{n}$ (sono asintoticamente equivalenti) ->
$a_{n}$ converge/diverge $\Leftrightarrow b_{n}$ converge/diverge 

## criterio della radice e del rapporto
sia $a_{n}$ una successione, converge/diverge 
* se $\exists \lim_{ n \to \infty } \sqrt[n]{ a_{n} } = L$ (criterio della radice) 
* oppure se $\exists \lim_{ n \to \infty } \frac{a_{n+1}}{a_{n}} = L$
allora se:
* $L>1$ allora la serie di $a_{n}$ diverge a $+\infty$
* $L < 1$ allora la serie di $a_{n}$ converge

## criterio di Leibniz
* se $\lim_{ n \to \infty } a_{n} = 0$
* e $a_{n}$ **monotona non crescente** 
allora: 
$$
\sum_{n=k_{0}}^{\infty}(-1)^ka_{n}\text{ converge}
$$
### dim.
considero la **somma** parziale $S_{n} = \sum_{k=0}^n (-1)^k a_{k}$ e vediamo se tende a $L \in R$:
![[Pasted image 20240217222535.png]]
ad intuito sembrerebbe che gli indici pari decrescano e che quelli dispari crescano, infatti:
* $S_{2n+2} = S_{2n} + (-a_{2n+1} + a_{2n+2}) \leq S_{2n}$
* $S_{2n+1} = S_{2n-1} + (a_{2n} - a_{2n+1}) \geq S_{2n-1}$
inoltre questi valori sono limitati in $[S_{1},S_{0}]$.

considero le serie: $S_{2n}$ e $S_{2n-1}$ che tendono rispettivamente a $L_{0} \text{ e } L_{1}$
ho infine che: 
$$
L_{0} - L_{1} = \lim_{ n \to \infty } (S_{2n} - S_{2n-1}) = \lim_{ n \to \infty }  a_{2n} = 0
$$
quindi: $L_{0} = L_{1} = L$

---

> [!note] OSS.
> i criteri valgono solo per le serie a termini positivi!

>[!note] OSS.
>se una serie e' a termini negativi basta **trasformare** la serie data in una serie a termini positivi e mettere il meno fuori!
# frattale di koch
....