### strategie di allocazione memoria
* **Equa**: tutti i processi hanno la stessa memoria disponibile. *Non e' il sistema migliore.*
* **Proporzionale**: assegno i frame in base ai processi, dinamico.

Il sistema operativo garantisce un **minimo di pagine** a cui ogni processo deve accedere senza doverle recuperare dalla memoria, per eseguire le operazioni fondamentali.

Come applico il sistema proporzionale? Modifico le pagine in base alla frequenza dei page fault. Diminuisco le pagine per il processo se i page fault sono più rari.

Se il processo ha 1 sola pagina, allora ogni volta che richiedo un pagina nuova ho un page fault: page fault $\infty$ per $p\to 1$. Indipendentemente dalla memoria disponibile fisicamente, il sistema operativo cerca un **trade off** tra zona $A$ (molto page fault, ho poche pagine) e $B$ (poco page fault, ho troppe pagine). 

Bisogna trovare  il trade off migliore che diminuisce il mio **overhead**, dovuto al fatto che se richiedo una pagina che non e' in memoria e ho esaurito lo spazio, devo scambiare le pagine.

Strategie **estreme** per gestire le pagine:
* **Out Of Memory Killer**: se un processo va oltre il numero di pagine massime, questo daemon di sistema uccide il processo. Va fatto per evitare di compromettere la stabilita del sistema.
* **Scheduling a due livelli**: le pagine dei processi in attesa vengono messi in memoria non volatile. Funziona per processi in background che vengono usati molto raramente.
* Altre tecniche possono essere: **compattamente, compressione e de-duplicazione**.
* **Paging Daemon**: processo che esegue in background, non posso interagirci, si attiva periodicamente per controllare lo stato della memoria. Se ci sono pochi frame liberi, sposto delle pagine in memoria non volatile usando degli algoritmi tra quelli già visti.

### ...

### Dimensione delle pagine e bilancio dei fattori
Se scelgo pagine troppo grandi, il processo potrebbe richiedere una pagina per poi usarne solo una piccola parte, ma riduce la frammentazione. Se scelgo pagine troppo piccole ho bisogno di una tabella delle pagine più grande.  

Calcolo della dimensione ottimale della pagine:
* **dimensione media** di un processo: $s$ byte
* **dimensione della pagina**: $p$ byte
* **Dimensione di ogni voce** nella tabella delle pagine: $e$ byte (es: 4 o 8 byte).

Calcolo dell'overhead:
* **Numero di pagine** per processo: $\sim s/p$
* **Spazio occupato** nella tabella delle pagine: $s \cdot e / p$
* **Frammentazione interna** nell'ultima pagina: in **media** e' $\frac{p}{2}$. 

Devo derivare $\frac{se}{p} + \frac{p}{2}$ rispetto a $p$, e poi esplicito $p$: $p = \sqrt{ 2se }$.

