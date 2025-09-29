`Linker e Loader` Sono simili ma eseguono azioni concettualmente diverse:

> [!note] Program Loading
> Copia del programma in memoria principale, cosi che sia pronto per l'esecuzione. Questa operazione include: gestione bit protezione, permessi, allocazione di storage, memoria virtuale ecc...

> [!note] Relocation
> Processo in cui assegnamo al programma degli indirizzi effettivi, utilizzabili per essere caricato in memoria.
> La rilocazione avviene sia nel **linker** (devo relocare gli indirizzi dei file oggetto) che al momento dell'**esecuzione** (devo caricare il file oggetto in memoria)

> [!note] risoluzione di simboli
> Bisogna risolvere da nome a **indirizzo** i nomi delle procedure chiamate nel file oggetto. 

> **Nota**: la `relocation` e' necessaria poiché il compilatore per file oggetto produce indirizzi che partono da `0x0`. 

> [!note] Loader e Linker
Un programma che fa *program loading* e' detto **Loader**, mentre un programma che fa *risoluzione dei simboli* e' un **Linker**.

### Linking a due passate
Prende come input un insieme di file oggetto e librerie producendo un'unico file oggetto in output con delle informazioni aggiuntive: load map e simboli per il debug.

Ogni file contiene dei **segmenti**: chunk di codice o dati da mettere nel file di output.
Inoltre, ogni file contiene una **tabella dei simboli**, dove questi possono essere:
* **esportati**: definiti nel file, *per essere usati in altri file*.
* **importati**: usati nel file, ma non definiti in quello stesso file. Dunque dovrebbe essere definito altrove!

All'esecuzione del linker bisogna: *scannerizzare i file in input per trovare le grandezze dei segmenti, e raccogliere definizioni e riferimenti di tutti i simboli*.

Durante la **prima passata** dunque vengono popolate la: segment table e la symbol table.

Alla **seconda passata** si effettua dunque il linking: leggo e ricolloco gli indirizzi nel file oggetto e i suoi segmenti scrivendo tutto in un'unico file di output!

> [!note] dynamic linking
> Il linking a runtime, viene ottenuto con un runtime linker che risolve la tabella del simboli importati a runtime!

> **Nota**: nel file di output troviamo informazioni per il debug dell'eseguibile e informazioni per il relinking.

> [!note] librerie a oggetti
> Devono essere risolte quando caricate!

### relocation e code modification
**Esempio**: considerando il seguente codice assembly:
```asm
mov a,%eax
mov %eax,b
```

Se `a` definito nella locazione `1234` e `b` e' **importato** da qualche altra parte allora in esadecimale il codice generato sara:
```hex
A1 34 12 00 00 mov a,%eax
A3 00 00 00 00 mov $eax,b
```

> **Nota**: `x86` legge le parole da destra a sinistra!

Considerando che il linker realloca tutti in modo che gli indirizzi siano spostati di `10000 byte` e che `b` sia all'indirizzo `9A12`, allora il linker modifica il codice questo modo:

```asm
A1 34 12 01 00 mov a,%eax
A3 12 9A 00 00 mov $eax,b
```

> **Nota**: scrivere `01` accanto a `12` equivale ad aggiungere l'offset di `1000 byte` all'indirizzo `1234`.

### compiler drivers

> **Nota**: l'operazione del linker e' invisibile al programmatore. E' eseguito automaticamente dal processo di compilazione!

> [!note] compiler driver
> La maggior parte dei sistemi di compilazione ha un `compiler driver` che invoca automaticamente le fasi del compilatore per ogni file, nel giusto ordine.

Di solito, i compiler drivers comparano la data di creazione di sorgente e file oggetto e ricompilano se e solo se i file sono cambiati effettivamente! 

> **Nota**: `make` e' un esempio di compiler driver.

### comandi per i linker
Ogni linker ha un linguaggio per essere controllato:
* **riga di comando**
* **intermixed** nei file oggetto
* **embedded** nei file oggetto: specifico le opzioni per quel file, all'interno di quello stesso file.
* **separate configuration language**: per esempio, il linker di `GNU` ha un suo linguaggio.

### esempio:
```c
// m.c
extern void a(char *);

int main(int ac, char **av)
{
	static char string[] = "Hello, World!\n";
	a(string);
}

// a.c
#include <unistd.h>
#include <string.h>

void a(char *s)
{
	write(1,s,strlen(s));
}
```

Compilato `m.c` , secondo l'autore, si ottiene un oggetto di 165 byte che contiene:
* `header` di lunghezza fissa
* `16 byte` di testo: contiene il programma
* `16 byte` di dati: contiene la stringa.

Poi ci sono due relocation entry:
* `pushl`: l'istruzione che mi prepara alla chiamata alla funzione, devo mettere il puntatore giusto alla stringa nello stack!
* `call` per `a`.

La tabella dei simboli e' in grado di esportare la definizione di `_main` ma invece importa `_a`.

> **Nota**: la `call` per `_a` fa riferimento all'indirizzo `0x0` dato che non puo' essere risolto.

Quando si fa il link per il programma finale, ogni segmento ha un padding per farlo arrivare al boundary di `4K`, per matchare la grandezza delle pagine in `x86`.

Il segmento di testo contiene il codice di una libreria di startup, chiamato `start-c`, poi `m.o` relocato a `10a4` e `a.o` relocato a `10b4`.