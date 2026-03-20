![[Pasted image 20260320115157.png]]
* **provo a togliere i numeri nella seconda riga**: riduce di poco la grandezza del posting
* **case folding**: accorpa parole che differiscono per maiuscole e minuscole. riduco del $17 \%$ la **grandezza del dizionario**.
* **togliere 30 o 150 stopword**: **cambia poco il dizionario**.
* **stemming**: tolgo le desinenze, i plurali ricondotti alla desinenza, riduco di un'altro $17\%$ la **grandezza del dizionario**.

**Cosa cambia nelle posting list**:
* **i numeri usati nei documenti sono generalmente pochi**, ma sono usati molte volte in Reuters, dunque ho una riduzione dell'$8 \%$.
* **case folding**: non risparmio molto, non sono parole molto frequenti.
* **rimuovere 150 stopword**: risparmio del $30\%$, sul positional index fino al $47\%$. Queste sono molto frequenti, dunque ho molto risparmio.
* **stemming**: nel dizionario ottimizzo, ma nel positional index d devo comunque ricordare dove compaiono le parole, non cambia niente.

**Nota**: poche parole che hanno alta frequenza, e tante parole con bassa frequenza.

**lossy**: queste tecniche per diminuire la grandezza delle strutture dati perdono dati. Non posso ricostruire i documenti originari.

**Heaps**: la grandezza del dizionario segue il numero di token nella collezione. **E' una legge empirica**.
* $M := \text{grandezza vocabolario}$
* $T := \text{numero di token}$
* $30 \leq k \leq 500$ e $b \sim 0.5$
$$
M = kT^b
$$

**Heaps come cresce**? per documenti grandi Heaps approssima molto bene.

> [!error] Fare esercizio su heaps per esame
> 

## Zipf's law
* si applica quando ho pochi fenomeni rappresentativi e tanti che non contano nulla.
* **La $i^{th}$ parola piu frequente ha frequenza proporzionale ha $\frac{1}{th}$.**
* **OSS**: si vede bene con le stopwords, infatti rimuovendole dimezzo le postings.

**conseguenza**:
* se `the` (priam piu frequente) ha frequenza $\text{cf}_{1}$
* allora per `of` (seconda piu frequente) $\text{cf}_{2}=\frac{\text{cf}_{1}}{2}$
* allora ... $\text{cf}_{3} = \frac{\text{cf}_{1}}{3}$.
* ecc...

**gemini**: e quindi a che serve sto zip?

## Compressione della porcamadonna
**perche' comprimere il dizionario?** perche' e' grande, sta in memoria, e voglio leggerlo velocemente. 
* **dispositivi embedded**: poca memoria!

**assunzione**: usiamo **un'albero binario di ricerca** porco dio.

**28 byte per ogni termine**: 
* **20 byte**: il termine **ma magari non li uso tutti**.
* **4 byte**: frequenza
* **4 byte**: puntatore

**problema**: spreco memoria
**soluzione**: una stringa lunga con tutti i termini concatenati. Il dizionario e' fatto cosi:
* **freq**: frequenza del termine
* **postings ptr**: la sua posting list
* **string ptr**: ed il puntatore alla stringa.

**struttura blocking**: non memorizzo tutti i puntatori. Ne tengo in memoria uno ogni $k$.
**lunghezza all'interno della stringa**:   salvo nella stringa la lunghezza del termine, cosi so quando fermarmi.
**problema**: piu risparmio con $k$ alto e piu e' la complessita nella ricerca della stringa.

![[Pasted image 20260320124059.png]]


**osservazione**: le lettere sono ordinate, nel senso che iniziano con una stessa sequenza di lettere.

**front coding**:
![[Pasted image 20260320124821.png]]

## compressione della posting
compressione della posting: devo tenere in memoria i $\text{docID}$ che su 400.000 documenti richiedono $\log_2 400.000 = 20$ bit

**bitmap vector**: l'ideale per indicare in quali documenti compare un termine denso. Meglio rispetto a ricordare tutti i numeri di tutti i documenti in cui compare il termine. 

**delta o gap**: tengo in memoria il delta tra la posizione ed il numero precedente 

![[Pasted image 20260320130039.png]]

**obiettivo**: utilizzare pochi bit se l'intero ne richiede pochi.
**soluzione**: variable length encoding con **unary code**.


**unary code**: conto gli uni e poi metto uno 0
![[Pasted image 20260320130437.png]]

**gamma code**:
* rappresentare numeri in binario, ma devo identificare dove inizia uno e finisce l'altro.
* **osservazione**: ogni numero binario inizia con 1, lo posso togliere.
* $13\to 1101 \to 101$
* mi devo ricordare che $101$ e' lungo 3: in unario e' $1110$
* dunque mischiando le due cose: $1110.101$.
* **codifica** **ottima**: 