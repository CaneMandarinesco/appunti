**commenti**: `#`.

**andare a capo**: `/`, perche' essendo il Makefile basato su righe, uso il backslash per dire che vado a capo intenzionalmente.

## trick per andare a capo
```makefile
var := one$\
		word
```

E' equivalente a `var := oneword`, che e' equivalente a `var := one$ word`. 

## nome del makefile, includere 
`Makefile`: questo nome, avendo la `M` grande, dovrebbe comparire "in alto" quando faccio il listing della cartella, insieme a file importanti come `README`.

`include`: dice a `make` di iniziare a leggere immediatamente un'altro file.



