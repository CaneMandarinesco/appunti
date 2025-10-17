Mentre il livello di applicazione definisce un protocollo per definire come comunicano due end point, il livello di trasporto offre un **servizio di connessione logica** **tra due socket** tramite:
* multiplexing/demultiplexing dei pacchetti
* checksum
* affidabilità
* arrivo in ordine dei pacchetti
* controllo del flusso
* controllo della congestione router

Ci sono due principali protocolli ti trasporto:
* `UDP`: *protocollo senza fronzoli*, fa la checksum del pacchetto ma non garantisce nulla sull'affidabilità, e sull'arrivo in ordine.
* `TCP`: *protocollo "sicuro"*, garantisce l'arrivo a destinazione dei messaggi e che arrivino in ordine fornendo l'astrazione di un canale di comunicazione "sicuro". *Richiede una **connessione** prima di iniziare a scambiare messaggi*.

> `TCP` e' piu robusto perche' il protocollo `ip` sottostante e' di tipo `best effort`, ossia non garantisce l'arrivo dei pacchetti a destinazione, e inoltre non offre multiplexing dei pacchetti.

# multiplexing e demultiplexing
Su un `host` possono esistere piu' socket contemporaneamente, dunque bisogna effettuare il demultiplexing, verso la giusta porta, ossia la giusta socket.

> `multiplexing`: azione con cui un protocollo di trasporto prende i messaggi dalle socket e li incapsula in segmenti

> `demultiplexing`: azione con cui i segmenti una volta a destinazione vengono consegnati alla giusta socket

> `tcp` prima di effettuare il demultiplexing si occupera' di controllare se i pacchetti sono arrivati correttamente e di consegnarli in ordine. 

Ma come faccio a conoscere la porta di destinazione a priori? Semplice: a seconda del tipo di software con cui devo dialogare e' standardizzato (Es: `SSH` su porta 22, `HTTP` su porta 80, ecc...).

> [!note] demult UDP/TCP
> * `TCP`: in base a  `(source port, dest port)`
> * `UDP`: in base a `dest port`

## perche' UDP?
> `UDP` e' un protocollo che da la possibilita di implementare i meccanismi di TCP lato utente, insieme ad altri controlli di sicurezza.

> `TCP`: potrebbe essere svantaggioso a causa dell'overhead. effettua molti controlli e aggiunge moltoo byte in overhead. Ci sono eventuali ritrasmissioni inutili.
> Non va bene per real time e streaming in genere.

Ci sono infatti applicazioni che tollerano la perdita di messaggi e l'arrivo fuori ordine, o che tollerano anche una rete congestionata.

> *Lo streaming UDP e' tuttavia sconsigliato*: richiede molta banda e non si regola! Congestiona la rete.

> [!note] socket passiva
> Un `host` ha una socket passiva per accettare le richieste di connessione `tcp`.
# UDP
La struttura di un segmento  UDP e' semplice:
* **numero porta di origine**
* **numero porta di destinazione**
* lunghezza
* checksum

> [!warning] porta origine
> A che serve la porta di origine in `udp`? 
 
## Checksum
La checksum e' un campo che permette di identificare con una certa accuratezza se sono avvenuti errori di trasmissione.

Per usare la checksum bisogna vedere il pacchetto come una sequenza di interi a 16 bit rappresentati in complemento a 1. 
Ogni sequenza viene sommata: inclusa la checksum, se il risultato e' $(1...1)$ ossia 0, allora il pacchetto e' stato ricevuto correttamente.

> **Nota**: il riporto alla cifra più alta va sommato al primo numero.

> **Nota**: l'ultimo bit, diciamo in posizione $n$, in complemento a 1 vale: $2^n -1$. Dunque `100` e' `3`. 
# Trasmissione affidabile
La trasmissione affidabile e' implementata come un protocollo umano: *per essere sicuro che il destinatario abbia ricevuto il messaggio attendo da lui un messaggio di conferma.*

Per costruire la trasmissione affidabile useremo una macchina a stati finiti che lavora sulle primitive:
* `rdt_send`: evento ricevuto dal livello di applicazione
* `udt_send`: chiamato dal mittente per inviare attraverso il canale di trasmssione sottostante
* `rdt_rcv`: event ricevuto dal canale di trasmissione sottostante
* `deliver_data`: chiamato dal ricevente per inviare all'applicazione i dati
Sia assume dunque che tra i due host ci sia un qualche tipo di collegamento logico, e su questo costruiremo le macchine a stati finiti.
### rdt 1.0
> Il canale di trasporto sottostante e' totalmente affidabile
> * nessun errore nei bit
> * nessuna perdita

dunque non c'e' molto da dire qui: `rdt_send` passa il pacchetto a `udt_send` e viceversa.
### rdt 2.0
> Si assume che il canale sottostante possa invertire alcuni bit.

Introduciamo l'uso dei messaggi `ACK` e `NAK`.

Codice **mittente**:
```rust
var sndpkt

on state0:rdt_send(data)
	sndpkt = makepkt(data, checksum)
	udt_send(sndpkt)
	goto state1

on state1:rdt_rcv(rcvpkt)
	if isNAK(rcvpkt):
		udt_send(snd_pkt)
	if isACK(rcvpkt)
		goto state0
```

Codice **ricevente**:
```c
on rdt_rcv(rcvpkt):
	if iscorrupt(rcvpkt):
		udt_send(NAK)
	else:
		data = extract(rcvpkt)
		udt_send(ACK)
		deliver_data(data)
```

Il protocollo usa la tecnica `stop and wait`, quindi per ogni pacchetto devo aspettare (non sembra molto efficiente...).

Inoltre il protocollo contiene degli errori fatali: se riscontro `ACK/NAK` corrotto cosa faccio?  

> [!warning] problema degli stati
> In caso di `ACK/NAK` errati il ricevente non sa in che stato e' il mittente!
### rdt 2.1
Ad ogni pacchetto associamo un numero di sequenza preso tra 0 e 1 e duplichiamo la macchina a stati finiti per ogni numero di sequenza.

Il **codice del mittente** e' il medesimo di prima, ma `makepkt` ora *richiede anche il numero di sequenza da associare al pacchetto*.

>Inoltre, quando ricevo un `ACK` o un `NAK` questo deve avere il numero di sequenza corretto.

Il codice del ricevente:
```rust
on state0:rdt_rcv(rcvpkt)
	if iscorrupt(rcvpkt) || has_seq1(rcvpkt):
		udt_send(NAK, 0)
	else:
		rdt_send(ACK, 0)
		data = extract(rcvpkt)
		deliver_data(data)
		goto state1

on state1:rdt_rcv(rcvpkt)
	if iscorrupt(rcvpkt) || has_seq0(rcvpkt):
		udt_send(NAK, 1)
	else:
		rdt_send(ACK)
		data = extract(rcvpkt)
		deliver_data(data)
		goto state0
```

### rtd 2.2
> In rdt 2.1, `NAK` e' superfluo, in quanto un'errore puo' essere rilevato quando ricevo semplicemente un `ACK` con numero di sequenza errato.

Ossia, se ricevo un `ACK` con codice di sequenza errato, il ricevente allora era ancora fermo al pacchetto precedente.

### rdt 3.0
>Ora si assume che il livello sottostante possa perdere pacchetti, come gestiamo questo caso?

Si implementa un meccanismo di timeout: quando scade si assume che il pacchetto sia perso. L'*unica componente che cambia e' il mittente dunque*.

Quello che succede e' che una volta inviato un pacchetto viene avviato un timer, e nello `stato 1` se questo timer scatta prima di riscontrare un `ACK` allora re invio il pacchetto e rimango in `stato 1`: questo succede per tutti i codici sequenza

Gestisce correttamente:
* perdita di pacchetti
* timeout prematuri
* ack persi
* ack ritardato


# Pipelining

Ci accorgiamo che le prestazioni di `rdt 3.0` sono scadenti, dato che al caso migliore inviamo 1 pacchetto correttamente e poi attendiamo 2RTT prima di spedirne un'altro. Possiamo fare di meglio?

> [!note] tasso di trasmissione effettivo
> $$
> \frac{L/R}{RTT + L/R}
> $$
> ![[Pasted image 20250909124632.png]]

Con il **pipelining** si vuole mandare $k$ pacchetti insieme, uno dopo l'altro e si aspetta poi di riscontrare gli `ACK` per ogni pacchetto.

Quindi l'utilizzo della banda e' moltiplicato per $k$!

## protocollo  Go Back N
Con il go back n si vuole implementare il pipelining attraverso l'uso di una finestra scorrevole, di grandezza $k$, che si muove sul buffer di invio.

> [!note] `ACK` comulativo
> $ACK(n)$ e' il valore dell'ultimo pacchetto riscontrato in **ordine** per i primi $n$ pacchetti. Per ordine si intende dal pacchetto 1 a $n$ senza saltarne nesuno.
> ![[Pasted image 20250909125156.png]]

Dunque, `timeout(n)` allo scadere ritrasmette il pacchetto $n$ e i numeri di sequenza piu grandi non riscontrati.

Come mi accorgo dei duplicati? come detto in rdt 3.0, il ricevente inviera' `ACK` duplicati per il numero di sequenza che ha riscontrato. 

Se ricevo un pacchetto fuori ordine:
* lo scarto
* e invio `ACK` con numero di sequenza `ACK(n)`.

### selective repeat
Invece di usare $ACK$ e timer cumulativi, per migliorare le performance in **connessioni rumorose** (es. wireless) si preferisce riscontrare gli ACK pacchetto per pacchetto.

Per implementare selective repeat a differenza di go back n c'e' bisogno di tenere in memoria i **pacchetti ricevuti fuori ordine** e di attendere quelli mancanti!

Dunque il **mittente**:
* invia il prossimo numero di sequenza se disponibile
* con `timeout(n)`: re invia il pacchetto `n-esimo`
* `ACK(n)`: riscontra il pacchetto $n$ esimo. se il pacchetto e' alla base della finestra allora puo' scorrere sul buffer.

> [!warning] Dimensione della finestra
> I numeri di sequenza sono ciclati con aritmetica modulare, per evitare problemi: $m \geq 2w$ dove $w$ e' la grandezza delle finestre di mittente e ricevente.

### `mtu` e `mss`
`mss` e' la lunghezza massima dei dati, a livello di trasporto. Si ricava a partire dalla `mtu`.![[Pasted image 20250909140751.png]]
Il valore della `mtu` puo' essere ottenuto tramite un `mtu path discovery`. Di base la `mtu` e' pari a1500 di pende dal protocollo di collegamento.

> **Nota**: la `MTU` e' la dimensione massima di un pacchetto a livello di rete. `MSS` e' la dimensione massima del payload `TCP`

> **Nota**: dunque `MSS = MTU - IP header - TCP header`, ossia con `MTU=1500`, la `MSS` e' di `1460`.
### segmento tcp
![[Pasted image 20250917164933.png]]

* `seq number`: numero di sequenza che mi aspetto di riscontrare
* `ack number`: numero di `ack` che ho riscontrato in sequenza
* `ACK`, `SYN` e `FIN` per gestire la connessione `TCP`
* `window size`: per comunicare quanti byte sono disposto ad accettare

> [!note] `ack number` e `seq number`
> * `ack number`: vuol dire che tutti i segmenti fino ad `ACK-1` sono stati riscontrati e che devo ancora riscontrare `ACK`!
> * `seq number`: si riferisce al primo byte di dati che sto inviando, e quindi mi aspetto di riscontrare `seq number+len` dal ricevente. 

> [!warning] `ID`
> non esiste un campo `ID`.


![[Pasted image 20250909141154.png]]

> Nota: `sequence nubmer` si riferisce al numero del byte nella finestra da inviare.

> Nota: `ack number` si riferisce al **byte** che devo ancora riscontrare.

Dove c'e' una parte di intestazione a lunghezza variabile, per questo la lunghezza deve essere specificata nel pacchetto stesso.

Compaiono anche i bit `RST`, `SYN`, `FIN` per il controllo della connessione.

> **Nota**: `ack piggybacked` 
![[Pasted image 20250909141548.png]]
## stimare RTT
Per selezionare un buon valore per il timeout occorre stimare l'RTT man mano che vengono inviati i messaggi, facendo un'approssimazione pesata e mobile.

* $estRTT = (1-\alpha)estRTT + \alpha sampleRTT$
* $timeout = estRTT + 4devRTT$. dove $devRTT$ e' un margine di errore calcolato sul momento.

> In altri esempi di tcp si puo' vedere che un ack puo' coprire eventualmente altri ack persi.

> [!note] ritrasmissione rapida
> Non conviene sempre aspettare il timeout. Se ottengo in risposta `3 ACK` con numero di sequenza uguale allora ritrasmetto immediatamente il pacchetto!

### buffer e flusso
Il protocollo `TCP` e' implementato usando dei  buffer nel sistema operativo: nel buffer del ricevente l'app preleva dal buffer di sistema e il protocollo di trasporto immette nel buffer.

> *Ma quanti dati e' disposto ad accettare il ricevente?* Ossia quanti ne usa l'app?

> [!note] `rwnd`
> E' un campo nell'header tcp. Specifica al destinatario quanti byte sono disposto a ricevere.
> $\text{rwnd} = \text{RcvBuffer} - (\text{LastByteRcvd} - \text{LastByteRead})$

### connessione TCP
si usa 3way handshake con **controllo sui numeri di sequenza** (scelti in modo che non siano prevedibili).

> **Nota**: la connessione server a stabilire dei parametri e ad *assicurarsi che l'altra meta sia disposta a scambiare messaggi.*

> [!note] 2 way handshake
> fallisce perche' se la richiesta di connessione arriva al server *quando il client non e' piu disponibile* (es: e' spento) allora la connessione e' aperta solo dal lato del server.

> **Nota**: con `ESTAB` si indica lo stato in cui un end-point ha stabilito dal suo lato la connessione.

Dunque:
* **Client**: manda pacchetto `TCP SYN`, con `seq=x`
* **Server**: risponde con un `SYN ACK` dove `ack=x+1` e `seq=y`. Qui ho riscontrato il `SYN`! 
* **Client**: manda `ACK` per il `SYN ACK`, e potrebbe anche poter mandare dati dell'applicazione a questo punto!

Quando il client riceve `SYN ACK` allora la connessione e' `ESTAB`. Il server passa a `ESTAB` quando riceve l'`ACK` del client.

La connessione viene chiusa con `FIN` da entrambi i lati e l'altro lato invia `ACK` per concordare la chiusura!

> [!note] TCP syn flood...
> diocane!

# congestione
Il controllo del flusso e' profondamente diverso dalla congestione: il primo evita di intasare l'**endpoint**, il secondo evita di intasare il **nucleo della rete**!

La congestione e' un problema causato da vari scenari tra cui:
* gli host saturano la capacita di trasmissione di un router causando ritardi
* a causa di **ritrasmissioni** si riempiono i buffer.
* invio di **pacchetti duplicati** (a causa di timeout).
* Se invio troppi dati, i pacchetti di un'altro host potrebbero venire scartati a causa mia.

> **Nota**: Bisogna evitare di saturare la capacita della connessione, quando cio' avviene, **aumenta esponenzialmente il ritardo**.

> **Nota**: Bisogna evitare ritrasmissioni inutili, per evitare di saturare la capacita della connessione. 

2 tipi di controllo della congestione: 
* `end-to-end`: gli end point **deducono** lo stato della rete e si regolano di conseguenza.
* `assistito dalla rete`: grazie al protocollo `ICMP` la rete puo' mandare dei `chockepacket` o **marcare** i pacchetti in transito per informare il destinatario della congestione.

### cwnd
Il campo `cwnd` viene dedotto dagli end-point e server per regolare la finestra in base alla congestione. Si utilizza il valore `min{rwnd, cwnd}` e vediamo un po di tecniche per calcolarlo!

La trasmissione e' limitata in modo che: $\text{LastByteSent} - \text{LastByteAcked} \leq min\{rwnd, cwnd\}$.

### AIMD (Incremento Additivo, Decremento Moltiplicativo)
La tecnica prevede di:
* **far crescere di** `1MSS` **OGNI RTT** per ogni messaggio la `cwnd`
* **far decrescere, dimezzando**, ogni volta che rilevo segni di congestione

> [!warning] aumento
> E' di `1MSS` per ogni RTT, ossia `1MSS/cwnd`
### TCP RENO
Come in AIMD ma:
* triplo `ACK` $\to$ dimezzo `cwnd` e vado in **fast retransmit**
* timeout $\to$ decremento `cwnd=1` e vado **slow start**.

> **Nota**: `AIMD` e' praticamente un'algoritmo **asincrono** e distribuito in **quanto** e' stato dimostrato che ottimizza i flussi congestionati in tutta la rete.

> [!note] fast retransmit
> *Reinvio immediatamente IL pacchetto non riscontrato*.

> [!note] slow start
> Ad inizio connessione, faccio crescere **esponenzialmente** la `cwnd` iniziando da `1MSS`. Appena rilevo una perdita mi fermo torno a crescere additivamente.

> [!note] `ssthresh` e `slow start`
> La `cwnd` dovrebbe crescere esponenzialmente dunque fino a `1/2` del valore raggiunto prima della perdita, ossia `ssthresh`

Quando in **slow start** supero `ssthresh` vado in `congestion avoidance`: aumento la cwnd con `MMS^2/cwnd`, ossia incremento di `1 MSS` ogni `RTT`.

Quando in congestion avoidance ottengo 3 ack duplicati vado in fast recovery: incremento`cwnd` in modo additivo ed eventualmente vado in congestion avoidance
### TCP Cubic
sia $W_{max}$ la `cwnd` misurato prima della congestione, allora:
* mi avvicino **velocemente** a $W_{max}$, molto probabilmente va ancora bene come valore limite. (di solito in rete non ci sono **cambiamenti bruschi**)
* quando sono vicino **cresco molto lentamente**
* Quando rilevo perdita scendo di un fattore moltiplicativo
![[Pasted image 20250909165330.png]]

Con $K$ l'istante stimato in cui mi avvicinerò a $W_{max}$, allora $W$ cresce come una funzione del cubo rispetto a $K$.

### TCP Vegas
Tra tutti i router, ci sara un collo di bottiglia. Ossia quel pezzo di merda che e' congestionato.

> Vogliamo che il router piu lento sia **pieno, ma non troppo**, vorremo evitare ritardi troppo grandi.

> [!note] $RTT_{min}$ 
> possiamo stimare la congestione guardando $RTT_{min}$ e possiamo indicare con $cwnd/RTT_{min}$ il troughtput che posso immettere quando non c'e' congestione

Quindi guardando $RTT$ misurato in un determinato istante, so quando sono lontano da $RTT_{min}$ e ne deduco la congestione: se conviene crescere o diminuire linearmente la `cwnd`.

### ECN
> [!note] ECN
> Sono due bit nella intestazione `IP` usati per indicare la congestione. Possono essere marcati anche i bit `CWR` ed `ECE` di `TCP`.
> L'uso di `ECN` deve esser concordato, ossia il mittente e' in grado di gestire i bit `ECN`?

### altro
> **Nota**: `TCP` e' fair, con l'andare del tempo le connessioni tendono a condividere equamente la banda.

> **Nota**: quando si verificano perdite e riesco a tornare a $W_{max}$, il troughtput medio effettivo e' di $3W/4RTT$

### QUIC: Quick UPD Internet Connections
* faccio `handshake` **sicuro** e cifrato in 1 solo rtt
* Posso gestire molteplici flussi a livello di applicazione
* 

> **Nota**: `TCP` sicura richiede 2RTT: handshake TCP + handshake TLS per la cifratura.


