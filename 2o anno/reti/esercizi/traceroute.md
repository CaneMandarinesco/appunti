Stampa la lista di router attraversati per arrivare a destinazione mandando una sequenza di pacchetti `ICMP` con `TTL` che cresce da 0.

> **Nota**: *i ritardi di accodamento sono molto variabili*, potrebbe essere dunque che il router $k$ nel momento in cui e' stata effettuata la misurazione abbia un `RTT` piu' elevato di una richiesta al router $h$ con $h > k$. (paradossale ma e' dovuto al ritardo di accodamento!)

> **Nota**: l'asterisco indica che non ho ricevuto nessun pacchetto `ICMP`. Potrebbe essere andato perso o il router non risponde usando `ICMP`.

> **Nota**: `traceroute` non invia pacchetti `ICMP`, ma piuttosto invia **datagrammi** `UDP`, dove nell'intestazione `IP` manipolo il `TTL`.

> **Nota**: `traceroute` usa **datagrammi** `UDP` con porta improbabile, in modo da ottenere come risposta un'errore `ICMP` dall'host di destinazione.

```bash
traceroute -M icmp www.google.it
```
con `-M` indico se voglio usare `UDP` o `ICMP` (e usare quindi `echo req` e `echo rep`)

> **Nota**: potrebbe essere che con `-M icmp` a differenza di `-M udp` ottengo un percorso diverso vero la destinazione!

```bash
traceroute -m 2 www.google.it
```
Dove con `-m` indico un massimo di `TTL`