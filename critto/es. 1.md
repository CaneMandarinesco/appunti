
## attacco a permutazione
* guardo le lettere piu frequenti e i digrammi, trigrammi piu frequenti e assegno la permutazione in base a cosa mi conviene.
## attacco a vigenere
**Test di kasiski**: nel testo compare spesso il trigramma `HJV`.
* Le distanze tra i trigrammi sono: $18,138,54,12$
* $\text{MCD}(18,138,54,12)=6$

**Sottostringhe**: dunque, ci sono 6 sotto-stringhe $w_{i} \text{ per } 0 \leq i < 6$. Con $y_{i}$, $\text{i-esimo carattere del testo cifrato}$ 
* $w_{i} = y_{i}y_{i+m}y_{i+2m}\dots$ e' la sottostringa $i\text{-esima}$

**Analisi indice incidenza**:
* allora per ogni $w_{i}$ voglio trovare lo shifting $k_{i}$
* Questo $k_{i}$ deve essere quello che si avvicina di piu a $0.065$
* Equivalentemente, Per ogni $w_{i}$ voglio trovare $k$ che massimizza $s_{k} = \sum f_{{i}_{k}} p_{k}$, dato che $\sum p_{i}^2$ e' l'indice di incidenza nella lingua inglese.
	* $p_{k}$: indice incidenza della lettera $k$ nella lingua inglese
	* $f_{{i}_{k}}$: indice incidenza della lettera $k$ nella sottostringa $w_{i}$

**Risultato**: $k=\text{CRYPTO}$