### `.bss`

> [!note] buffer
> Un buffer e' un blocco continuo di byte usati per `bulk data transfer`

Quando voglio leggere un file ho bisogno di un buffer dove buttare dentro il suo contenuto, cosi che possa leggerlo.

Dunque se voglio per esempio leggere un file riga, per riga, senza sapere quanto e' lunga ogni riga: posso usare buffer di dimensione fissa, e se non basta posso allocare un'altro buffer e continuare a leggere.

> **Nota**: i buffer hanno grandezza fissata dal programmatore. Quando voglio leggere da un file devo specificare buffer, e la grandezza massima di questo.

Posso creare un buffer con storage statico o dinamico:
* `statico`: dichiarato con `.long` e `.byte`
* `dinamico`: discusso nel capitolo 9.

Con la sezione `.bss` posso definire buffer statici in questo modo:

```
.sectiom .bss
.lcomm my_buffer 500
```

Ossia ho definito il simbolo `my_buffer` che si riferisce ad uno storage di `500byte`.

Se voglio per esempio leggere da un file con il descrittore in `%ebx` allora:
```asm
movl $my_buffer, %ecx
movl 500, %edx
movl 3, %eax # per chiamare la read
int $0x80
```

Ci sono almeno 3 file descriptor aperti in linux:
* `STDIN`: file in sola lettura per l'input da tastiera, dove `STDIN=0`
* `STDOUT`: file in sola scrittura che rappresenta il buffer del terminale
* `STDERR`: come `STDOUT` ma per errori
