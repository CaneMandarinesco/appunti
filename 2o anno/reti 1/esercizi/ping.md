```bash
ping www.google.it
PING www.google.it (...) 56(84) bytes of data
```
dove `84=56+20+8` dove `20` sono i byte dell'intestazione `IP` e 8 di intestazione `ICMP`.

```bash
ping -c 1 www.google.it
```
Con `-c` comando voglio mandare solo una richiesta di ping.

```bash
ping -w 3 www.google.it
```
Con `-w` invece indico un tempo massimo per la risposta.

```bash
ping -i 1.495 -w 3 wwww.google.it
```
Con `-i` indico l'intervallo che intercorre tra un pacchetto e l'altro.

> **Nota**: il timestamp e' un campo aggiuntivo in `ICMP` e viene usato da `ping` per calcolare l'`RTT`.

```bash
ping -s 10 www.google.it
```
Dove con `-s` indico la grandezza in byte del segmento `ICMP`. 

> **Nota**: se con `-s` specifico un valore minore di 16, allora non c'e' spazio per il timestamp.

```bash
ping -t 11 -c 1 www.google.it
```
Con `-t` imposto il `TTL` nell'intestazione `IP`. Ma non tutti i router rispondono con `ICMP`, dunque e' utile utilizzare `-W` per forzare un timeout.

Dunque:
* `-s`: size in byte
* `-t`: `TTL`
* `-W`: timeout
* `-c`: numero di pacchetti da inviare
* `-i`: intervallo