**spell correction**: *molti utenti sbagliano a scrivere / non sanno la parola precisa*

**wildcard**: `*`. permette cercare parole se ne si conosce solo una parte.

**albero binario**: se i termini sono ordinati in un albero binario, allora posso facilmente recuperare le parole corrispondenti alla query `mon*` per cui fare le query.

**come faccio a processare la query**: `se*ate AND fil*er`?
* **pero come processo `se*ate`**? faccio l'AND tra `se*` ed `*ate`.
* **problema**: troppi `AND` se ho tante wildcard di questo tipo. Costa troppo anche se uso un albero binario.

**soluzione definitiva**: *Permuterm Index*
* **gestisce** le wildcard query.
* **genera**: tutte le rotazioni di una parola.
* **puntatore**: ogni rotazione punta al termine originale.
* **perché**? ruotando trasformo tutte le wild-card nella forma standard con `*` solo alla fine. 
* **struttura dati per le rotazioni**: `B-tree`.
* `$` indica la fine del termine
![[Pasted image 20260327110215.png]]

![[Pasted image 20260327110556.png]]
## bigram (k-gram) indexes
* **enumera** i $\text{k-grammi}$, ossia le sequenze di $k$ caratteri che occorrono in un termine.
	* i $2\text{-grammi}$ sono tutte le sequenze di 2 caratteri.
![[Pasted image 20260327111113.png]]
* **inverted index**: devo mantenerne uno per andare dai bigrammi ai termini nel dizionario.
* ![[Pasted image 20260327111246.png]]

**processare una query**: se devo risolvere `mon*` allora devo fare `$m AND mo AND on`.
* **falsi positivi**: devo applicare un post filter, il sistema trova anche `moon` che contiene `$m`, `mo` ed `on`.
* **meglio del permuterm**: piu' efficiente.

## spelling task
* spelling error **detection**
* spelling error **correction**

**errori di spelling**:
* `non-word`: ossia il termine non appartiene al dizionario. Facile da individuare come errore.
* `real-word`: il termine esiste ma ha bisogno di contesto.
* `cognitive`: errore di pronuncia o suono simile
	* `peace -> piece`, `two -> too`
	* errori che dipendono da come si pronuncia la parola.

**rimediare ad** `non-word spelling`:
* *