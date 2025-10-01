Ogni funzione ha:
* `nome`: simbolo che marca dove inizia la funzione
* `parametri`
* `variabili locali`: non accessibili al resto del programma.
* `variabili statiche`: come le variabili locali, ma sono riutilizzate. Dunque non vengono mai liberate
* `variabili globali`
* `ret addr`: dove devo andare quando finisco l'esecuzione?
* `ret value`:

> [!note] calling convention
> Definisce come ortganizzare in memoria queste informazioni. Ogni linguaggio di programmazione potrebbe avere la sua calling convention.

> **Nota**: La calling convention di `C` e' quella piu usata ed e' lo standard per Linux.

> [!note] stack
> Lo stack si trova in cima alla memoria: posso fare `pushl` o `popl`.

> **Nota**: dato che lo *stack cresce verso il basso*, allora con `TOP` mi riferisco alla porzione piu in basso dello stack.

`%esp` tiene sempre in memoria un riferimento al `top` dello stack.

Prima di eseguire una funzione si mettono tutti i parametri nello stack, nell'ordine opposto in cui appaiono nella firma, poi, con `call` eseguo la procedura.

`call` inserisce nello stack l'indirizzo a cui ritornare e modifica `%eip` per puntare alla nuova istruzione da eseguire.


```
param #N
...
param 2
param 1
ret addr <--- (%esp)
```

**All'inizio**:  la procedura deve mettere nello stack `%ebp`, ossia il current base pointer register, che viene usato per accedere a parametri e variabili locali e poi viene fatto `movl %esp, %ebp`, dunque posso usare il valore dello stack al momento dell'istruzione, per fare riferimento ai dati della procedura.

> **Nota**: applicando un offset ad `%ebp` posso accedere ai dati che voglio, tenendo a mente come e' fatta la calling convention

> **Nota**: posso usare tutti i registri che voglio per accedere ai dati nello stack, ma in `x86`, `%ebp` e' molto piu veloce.

**Quando termina**: la procedura deve mettere il valore di ritorno in `%eax`, resettare lo stack (liberare lo spazio usato),  reimpostare `%eip` al valore specificato nello stack.

E quando il controllo ritorna al chiamante, devo togliere dalla memoria i dati usati come parametri e esaminare `%eax` (ossia il valore di ritorno)

> [!note] distruzione dei registri
> Alla chiamata di una procedura si assume che il contenuto dei registri venga cancellato. Se voglio ricordarmi il loro valore, li devo mettere nello stack!

> **Nota**: quindi chiamare una funzione in assembly, vuol dire conoscere la sua calling convention e preparare stack e registri per l'esecuzione.

> **Nota**: il valore di ritorno per la `exit` Deve essere minore di `256`

Con: `.type power, @function` diciamo a linker che...

> [!note] `call vs jmp`
> Call mette nello stack l'indirizzo a cui fare ritorno chiamando ret 










