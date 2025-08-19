> E' calcolabile tutto (e solo) cio' che puo' essere calcolato da una macchina di Turing.

> Linguaggi di programmazione, grammatiche tipo 0, logica combinatoria e altri modelli di calcolo sono equivalenti alla Macchina di Turing.

> Non e' dimostrato formalmente che se la macchina di Turing non sa calcolare qualcosa, allora nessun'altro modello di calcolo puo' farlo.

Andiamo a considerare i seguenti costruitti:
* $x \gets y$ dove $y$ e' una variabile, valore costante o un'espresione.
* `if (cond) then begin (istr.) end else begin (istr.) end`
* `while (cond.) do begin (istr.) end`
* `Output: <nomeDiVariabile>` che mostra l'output.

Assumiamo che con una parola `Input:` vengano passate le informazioni in input prima dell'esecuzione del programma.

## Teorema 3.5
Per ogni programma in `PascalMinimo`, esiste $T$ di tipo trasduttore che scrive sul nastro di output lo stesso valore fornito dal programma in `PascalMinimo`.

**Schema Dimostrazione**
* ogni variabile e ogni valore costante ha il suo nastro. (i valori costanti non vengono mai modificati)
* un nastro per valutare le espressioni e le condizioni
* codifica binaria per il contenuto dei nastri.
* per ogni salto condizionale esiste $q_i$ oppure la coppia di stati $q_{i},q_{j}$ (nel caso di **else**)
* Ad ogni loop corrisponde lo stato $q_i$
* $q_0$ stato iniziale