In una rete wireless la componente principale e' la **stazione base**, connessa generalmente ad una rete cablata.

Ci sono due tipi di reti:
* a **infrastruttura**: ossia una stazione base ha una sua area di copertura e fornisce determinati servizi. con l'handoff quando l'host si sposta puo' collegarsi ad un'altra stazione e continuare a usare lo stesso servizio.
* **reti ad hoc**: non ci sono stazioni di base ma piuttosto ci sono host wireless che trasmettono ad altri host.

I collegamenti wireless sono soggetti ad:
* **attenuazione**
* **propagazione** su piu cammini con lunghezze diverse, dovuto alla **riflessione**
* **ostruzione**

> **Nota**: un bit trasmesso via wireless deve essere presente sul canale per abbastanza tempo per essere ricevuto.

> [!note] SNR
> Rapporto segnale rumore, ossia piu e' alto e meglio ricevo il segnale

> [!note] BER
> Tasso errore sui bit: probabilita che ricevo bit errato

> [!note] terminali nascosti
> `A,B` comunicano con `C` ma `A` e `B` non si vedono. Allora inconsapevolmente `A` ostruisce `B` se comunica con `C` e viceversa.

### CDMA
Ogni utente ha un codice, una sequenza di `chipping` per codificare i dati.

> [!note] ortogonalita 
> Se i codici sono ortogonali tra loro posso ridurre le interferenze tra segnali sulla stessa frequenza

> **Nota**: un singolo bit di dati viene codificato in $M$ bit. Quindi uso molta banda per trasmettere effettivamente meno bit

> **Nota**: quando devo decodificare raggruppo i bit $M$ ad $M$.

# wireless lan
Lo spettro e' **diviso in canali**, dove l'`AP` sceglie le frequenze da usare.

> 13 canali, posso sceglierne 3 contemporaneamente in modo che non interferiscono tra loro, dato che i canali devono essere a distanza 4 l'uno dall'altro.


Come mi associo ad un `AP`?
* scansiono per i `frame beacon` inviati periodicamente da `AP` con `SSID` e `MAC`. Quando mi autentico uso `dhcp` ecc...
* `frame` sonda di richiesta, ottengo in risposta le informazioni dagli `AP` in giro.

Vogliamo evitare le collisioni: `CSMA/CA` dato che e' difficile **rilevarle** a causa della debolezza del segnale:
1. se **percepisco canale inattivo** trasmetti l'intero fame (no `CD`).
2. altrimenti ritento con `binary exp backoff`.

> **Nota**: a differenza di CSMA/CD, ho bisogno di `ACK`. Qui non posso rilevare niente!!!! devo fare come TCP!!!!


> **Nota**: il ricevente prima di inviare ack deve aspettare dopo `SIFS`.


Per evitare ancora di piu le collisioni:
1. invio `RTS` (req to send) con `CSMA` normale
2. l'AP invia `cts` (clear to send)

> **Nota:** Se si presenta una collisione tra `RTS` successivamente qualche host ritenta a inviarlo.