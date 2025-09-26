Il formato e' descritot in `System V ABI` (un'insieme di convenzioni per la compilazione ed esecuzione di file) che specifica e distingue come sono costruite le sezioni `TEXT, DATA e BSS`.

Permette di immagazzinare vari tipi di programmi compilati e `linkati`.  

> **Nota**: contiene come minimo un `header ELF` e un segmento, e di solito contiene la sezione `.text` e `.data`. 

> [!note] `segmento` e `sezioni`
> Un **segmento** informazioni per l'esecuzione a run time, le **sezioni** invece contengono dati importanti per il `linking`.

> **Nota**: i file oggetto di librerie contengono dunque solo `sezioni` in quanto devono essere `linkate`!

> **Nota**: L'**header** dice al sistema come si crea l'immagine.

* `.text`: dove vive il codice. `objdump -drS process.o` mostra proprio questa parte.
* `.data`: dove  si trovano variabili statiche, e globalmente accessibili. `objdump -s -j .data. process.o`
* `.bss`: contiene array e variabili non inizializzate che dovrebbero essere messi a 0.
* `.rodata`: dove vanno le stringhe.
* `.stab`, `.stabstr`: simboli per il debug.

L'header contiene le informazioni per caricare un eseguibile `ELF`, bisogna dunque:
* verifica che inizia con `ELF` magic number
* leggere il layout dell'`elf` guardando l'intestazione.
* leggere l'header del programma per determinare il numero di segmenti da caricare.
* leggere il punto di entrata

