
* $M \in \mathbb{R}$ si dice **maggiorante** di A se $M$ e' tale che: $\forall a \in A \, a \leq M$
* $m \in \mathbb{R}$ si dice **minorante** di A se $m$ e' tale che: $\forall a \in A \, a \geq m$
---
* $M \in \mathbb{R}$ si dice **massimo** di A se $M$ e' tale che: $\forall a \in A \, a \leq M$ e **soprattutto** $M \in A$
* $m \in \mathbb{R}$ si dice **minimo** di A se $m$ e' tale che: $\forall a \in A \, a \geq m$ e **soprattutto** $m \in A$
---
* A si dice **limitato superiormente** se $\exists M: \forall a \in A$, $a\leq M$ con $M \in \mathbb{R}$
* A si dice **limitato inferiormente** se $\exists m: \forall a \in A$, $a \geq m$ con $m \in \mathbb{R}$
* A si dice **limitato** se valgono entrambe
## assioma di completezza 
* se A e' **limitato superiormente** ammette almeno un **maggiorante** che coincide con $sup\{\forall x \in A\}$
* se A e' **limitato inferiormente** ammette almeno un **minorante** che coincide con $inf\{\forall x \in A\}$
inoltre se:
* $sup(A) = +\infty$ allora non e' limitata superiormente
* $inf(A) = -\infty$ allora non e' limitata inferiormente
## proprietà di sup e inf
* $inf(A) = -\infty$ -> $\forall m \in R$, $\exists a \in A$ tale che $a < m$
* $inf(a) = m$ -> $\forall l>0$, $\exists a \in A$ tale che $a<m+l$
	* ovvero: ogni numero più grande di m non e' **minorante**
* similmente valgono le proprietà sopra per $sup$