### Teorema 9.1
Un linguaggio $L \subseteq \Sigma^*$ appartiene a $NP$ se e solo se esistono $T$ macchina di Turing deterministica e due costanti $h,k\in \mathbb N$ per cui: $\forall x \in \Sigma^{*}, x\in L$ se e solo se:
* $\exists y_{x}= \{0,1\}^*: |y_{x}| \leq |x|^k$
* $T(x, y_x)$ accetta
* $dtime(T,x,y_{x}) \in O(|x|^h)$

Ossia, deve esistere una parola $y_x$ la cui lunghezza e' lineare in $|x|$ per cui $T(x,y_{x})$ accetta in tempo lineare in $|x|$, ma chi e' $T$?

E' una macchina che se ha la parola $y_x$ giusta per la parola $x$, allora accetta! E questa $y_x$ gli deve essere suggerita, ma chi e' $y_x$? 

> $y_x$ e' una sequenza di quintuple che costituisce una computazione accettante di $NT(x)$ che e' suggerita dal **genio**

> $T(x,y_{x})$ verifica che $y_{x}$ sia una computazione accettante di $NT$ su input $x$.

Quanto impiega $T(x,y_{x})$? 
* $|y_{x}|\leq |x|^{k}$ dato che $ntime(NT,z) \leq |z|^{k}$.
* Verificare che $|y_{x}|$ e' sequenza di quintuple in $NT$ mi basta $O(|y_{x}|)$
* Simulare le quintuple richiede $O(|x|^{k} \cdot |y_{x}|) \subseteq O(|x|^{2k})$

> Dunque, con $ntime(NT,x) \leq |x|^{k}$ allora $dtime(T,x,y) \leq |x|^{hk}$.

> **Abbiamo dimostrato che** se $L$ sta in $NP$ se $\exists T$ che data in input una computazione deterministica di $NT$, posso verificare in tempo polinomiale se questa accetta o rifiuta su input $x$.

Ora sappiamo fare l'inverso? Ossia da $T, y_{x}, x$ ricavare $NT$? e quindi dimostrare che $L\in NP$?

$NT$:
* fase 1: **scegli** $y: |y| \leq |x|^k$ (scelgo non deterministicamente!) con $f(x)=|x|^k$ time constructible.
* fase 2: invoca $T(x,y)$ e se accetta in $O(|x|^h)$ passi, allora accetta, altrimenti rigetta.

> $NT$ e' simile ad un **programma in pascal minimo** che usa l'istruzione `scegli` per costruire $y$.

La fase 2 ha bisogno di:
* calcolare $g(x) = c |x|^h$ che e' time constructible in unario.
* simulare istruzione per istruzione $T(x,y)$ e verificare se accetta entro $g(|x|)$ istruzioni.

Ora $NT$ opera in tempo polinomiale:
* calcolo $f(|x|)$ e scelgo $y$ in tempo: $O(|x|^k)$
* simulo $T(x,y)$ in $O(|x|^h)$ operazioni, dove il ciclo while usa un numero costante di operazioni per ogni istruzione di $T(x,y)$.



