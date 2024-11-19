* [materiale didattico](https://www.mat.uniroma2.it/~tauraso/analisi1inf2324.html)
* [limiti notevoli](#limiti-notevoli)
* [derivate](#derivate)

> [!warning] importante!
> per esercitarsi consiglio di guardare i video di [marcello dario cerroni](https://www.youtube.com/@MarcelloDarioCerroni). ha una playlist di centinaia di video sulla risoluzione di limiti, integrali e serie.
## teoria
* [[insiemi, funzioni]]
* [[induzione, disuguaglianze bernoulli, potenza binomio]]
* [[maggiorante, minorante, ecc...]]
* [[limiti di successioni e di funzioni, nepero, teo. ponte]]
* [[teoremi delle funzioni continue]]
*  [[sin(x) su x]]
* [[derivate e teoremi (funzioni derivabili e non)]]
* [[asintoti, flesso, discontinuita]]
* [[o-piccoli e taylor con resto di  peano]]
* [[definizione di integrale]]
* [[primitive e teoremi sugli integrali]]
* [[integrali impropri]]
* [[serie numeriche]]
* [[frattale di koch]]
* [[numeri complessi]]
* [[problema di cauchy e calcolo diff. in piu variabili]]

## esercizi
* [[limiti esercizi parte 1]]
* [[limiti esercizi parte 2]]
* [[integrali immediati esercizi]]
* [[integrali per decomposizione esercizi]]
* [[integrali  raz. fratti esercizi]]
* [[integrali esercizi generali]]
* [[integrali impropri esercizi]]
* [[limiti teoria e esercizi taylor]]
* [[serie esercizi]]
> gli esercizi e i vari appunti sono presi da video su youtube, siti e libri vari. **possono contenere imprecisioni e errori**

> p.s. ho seguito poco (per nulla) le lezioni del tauraso, e se seguivo non capivo nulla

# dimostrazioni
* $\sqrt(2)$ non e' reale
* disuguaglianza di bernoulli
* potenza di un binomio
* dimostrazioni proprieta' dei limiti
* teorema degli zeri
* $\lim_{x\to0} \frac{\sin(x)}{x} = 1$
* teorema dei valori intermedi
* teorema di fermat sui punti stazionari
* teorema di bolzano-weierstrass
* teorema di weierstrass
* teorema di lagrange (o del valor medio)
* teorema di cauchy
* teorema de l'hopital
* criterio di monotonia
* taylor con resto di peano
* media integrale
* teorema fondamentale calcolo integrale
* teorema confronto integrali impropri
* condizione necessaria per convergenza
* criterio del confronto integrale
* criterio di leibniz

# limiti notevoli
1. $\lim_{x\to\infty} (1+\frac{1}{x})^x = e$
2. $\lim_{x\to\infty} (1+\frac{k}{x})^x = e^k$
3. $\lim_{x\to0} (1+kx)^\frac{1}{x} = e^k$
4. $\lim_{x\to0} \frac{\ln(1+x)}{x} = 1$
5. la **4** deriva da questa:   $\lim_{x\to0} \frac{\log_a(1+x)}{x} = \frac{1}{ln(a)}$
6. $\lim_{x\to0} \frac{e^x+1}{x} = 1$
7. $\lim_{x\to0} \frac{a^x+1}{x} = \ln(a)$
8. $\lim_{x\to0} \frac{(1+x)^c-1}{x} = c$
9. $\lim_{x\to0} \frac{\sin(x)}{x} = 1$
10. $\lim_{x\to0} \frac{1-\cos(x)}{x^2} = \frac{1}{2}$

# derivate
| $f(x)$ | $\frac{df}{dx}$ |
| ---- | ---- |
| $a$ | $0$ |
| $ax$ | $a$ |
| $x^a$ | $ax^{a-1}$ |
| $\ln(x)$ | $\frac{1}{x}$ |
| $\sin(x)$ | $\cos(x)$ |
| $\cos(x)$ | $-\sin(x)$ |
| $\tan(x)$ | $\frac{1}{\cos^2(x)}$ |
| $\arcsin(x)$ | $\frac{1}{\sqrt{1-x^2}}$ |
| $\arccos(x)$ | $-\frac{1}{\sqrt{1-x^2}}$ |
| $\arctan(x)$ | $\frac{1}{1+x^2}$ |
# sviluppi di Taylor
### esponenziale
$$e^x = 1+x+\frac{1}{2}x^2 +\frac{2}{6}x^3 + \dots + o(x^n)$$
### logaritmo (non ha il fattoriale)
$$
\log(1+x) = x - \frac{1}{2}x^2 + \frac{1}{3}x^3 + \dots + o(x^n)
$$

### potenza di un binomio (no segno alternato, derivate di $\alpha$)
$$
(1+x)^\alpha = 1 + \alpha x + \frac{\alpha(\alpha-1)}{2}x^2 +\frac{1}{6}\alpha(\alpha-1)(\alpha-2)x^3 + \dots + o(x^n)
$$
### sin
$$
\sin x = x-\frac{x^3}{6} + \frac{x^5}{5!}-\dots + o(x^n)
$$

### cos
$$
\cos x = 1-\frac{x^2}{2}+\frac{x^4}{24}-\frac{x^6}{6!} +\dots o(x^n)
$$

### tan (come sin, ma ha $\frac{2}{15}$ e $3$ senza fattoriale)
$$
\tan x = x-\frac{x^3}{3} + \frac{2}{15} x^5
$$