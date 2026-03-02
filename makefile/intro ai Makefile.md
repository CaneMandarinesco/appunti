**Makefile**: set di regole del tipo
```makefile
targets: prerequisistes
	command
	command
	command
```
* **targets**: nomi di file.
* **commands** o **recipe**: step per creare i target
* **prerequisites**: nomi di file che costituiscono le **dipendenze** dei target.

## esempio
```makefile
blah: blah.c blah.h
	cc blah.c -o blah
```
Il target `blah` viene eseguito se e solo se:
* `blah` non esiste
* **oppure** usa l'euristica basata su `timestamp`: `blah.c` e' più nuovo di `blah`.
* **oppure** `blah.h` e' più nuovo di `blah`.

> [!warning] L'euristica `timestamp`
> `make` compara i le date di modifica dei file guardando il filesystem! Dunque se modifico manualmente la data di modifica di `blah.c`, `make` potrebbe non accorgersi di dover rieseguire i target. 


## `clean`
```makefile
clean:
	rm main.o ciao.o lib1.o ...
```

`clean`: di solito, serve a pulire file di build. Inoltre `clean` **non e'** il nome di un file di un progetto! Se lo fosse non avrebbe senso.

> [!note] Target e Comandi
> I target non sanno nulla riguardo a quali comandi vengono eseguiti



## first target e variabili
**primo target**: in ordine di apparizione nel `Makefile`, viene eseguito per primo.  Gli altri target eseguiti sono dipendenze del primo target.

`OBJ`: (o nome equivalente) e' la variabile usata di solito per elencare i file oggetto necessari per il link del target.

```makefile
objects = main.o kbd.o command.o display.o \
	insert.o search.o files.o util
```

`$(OBJ)`: per rimpiazzare nel Makefile il nome della variabile col suo contenuto.

## deduzione
**deduzione**: Makefile sa dedurre che deve generare un target che termina per `nomefile.o` compilando il corrispettivo `nomefile.c` usando `cc -c

> [!note] `cc -c`
> Compila senza fare il link. Il file oggetto prodotto non contiene le risoluzioni per le procedure esterne.

## regole per pulire la directory
```makefile
.PHONY : clean
clean : 
	-rm edit $(OBJ)
```