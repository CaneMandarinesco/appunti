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
* **numero porta di origine**: altrimenti il server non sa chi ha inviato il messaggio
* **numero porta di destinazione**: per effettuare il forward dei paccheti
* lunghezza
* checksum
## Checksum
La checksum e' un campo che permette di identificare con una certa accuratezza se sono avvenuti errori di trasmissione.

Per usare la checksum bisogna vedere il pacchetto come una sequenza di interi a 16 bit rappresentati in complemento a 1. Ogni sequenza viene sommata: inclusa la checksum, se il risultato e' $(1...1)$ ossia 0, allora il pacchetto e' stato ricevuto correttamente

Quindi il mittente calcola la somma del pacchetto da inviare come sequenza di interi a 16 bit in complemento a 1, e successivamente calcola la checksum come quel numero da sommare per ottenere $0$.

# Trasmissione affidabile
La trasmissione affidabile e' implementata come un protocollo umano: per essere sicuro che il destinatario abbia ricevuto il messaggio attendo da lui un messaggio di conferma.

Per costruire la trasmissione affidabile useremo una macchina a stati finiti che lavora sulle primitive:
* `rdt_send`: evento ricevuto dal livello di applicazione
* `udt_send`: chiamato dal mittente per inviare attraverso il canale di trasmssione sottostante
* `udt_receive`: event ricevuto dal canale di trasmissione sottostante
* `rdt_receive`: chiamato dal ricevente per inviare all'applicazione i dati

Vogliamo creare l'astrazione di un canale di comunicazione affidabile, anche se le primitve `udt` implementano un canale di comunicazione inaffidabile (livello di rete) per consegnare in modo affidabile i dati al livello di applicazione 

### rdt 1.0
In questo protocollo is assume che: il canale di trasmissione non perda pacchetti.
* **Mittente**: un'unico stato dove si attende la chiamata della primitiva dall'alto, si crea il segmento TCP e si invia al destinatario
* **Ricevente**: un'unico stato dove si attende la chiamata dal basso, e si inviano i dati all'alto.
Non c'e' bisogno di controllare la checksum perche' 

### rdt 2.0
> Si assume che il canale sottostante possa trasmettere pacchetti errati

per essere sicuri che il ricevente abbia ricevuto il pacchetto e che non sia corrotto attendo la ricezione di un messaggio `ACK` o `NAK` (si ho capito, no non ho capito).
##### mittente
* `stato 0`: attendo la chiamata dall'alto, creo il segmento tcp, lo invio e vado in `stato 1`
* `stato 1`:  attendo la chiamata dal basso, attendo `ACK` o `NAK` dal mittente, se ho un `ACK` valido torno in `stato 0`, se ho `NAK` rimango in `1` e reinvio il pacchetto che ancora devo riscontrare

#### ricevente
* `stato 0`: atteno la chiamata dal baso, ricevo il segmento e controllo se e' corrotto o no. Se e' corrotto mando `nak`, altrimenti `ack`.

Il protocollo usa la tecnica `stop and wait`, quindi per ogni pacchetto devo aspettare (non sembra molto efficiente...).
Inoltre il protocollo contiene degli errori fatali: se riscontro `ACK/NAK` corrotto cosa faccio? Se ho ricevuto un `ACK` vuol dire che devo spedire un nuovo pacchetto, se ho ricevuto un `NAK` devo ritrasmetterlo, ma non lo so con sicurezza!

### rdt 2.1
Proviamo a risolvere il problema fatale di rdt 2.0. Introduciamo i numeri di sequenza, per ora solo due: `0` e `1`. Il problema e' che mittente e destinatario non riescono a sincornizzarsi sul messaggio da inviare, per risolvere il problema ogni messaggio inviato ha un codice sequenza in allegato.

Essenzialemente si duplica la macchina a stati finiti per ogni numero di sequenza

nel mittente, codice sequenza 0:
* `stato 0`: attendo chiamata dall'altro e invio pacchetto con codice sequenza 0 al destinatario, vado poi in `stato 1`
* `stato 1`: attendo ricezione di un `ACK/NAK` con codice sequenza `0`. Se ho un `ACK0` allora passo al codice sequenza 1, altrimenti, se ho `NAK0` posso reinviare. Se il pacchetto era corrotto semplicemente ritrasmetto con codice s 

similmente per codice sequenza `k`
* ...

Il ricevente:
* `stato 0`: attendo la ricezione di un pacchetto con codice sequenza `0`. Se ricevo codice sequenza sbagliato invio `ACK`, vuol dire che il mittente si era perso `ACK` mandato in precedenza. Se ricevo codice sequenza giusto invio `ACK` se non corrotto altrimenti `nak`. Se ho ricevuto correttamente un pacchetto posso andare nello stato
* `stato 1`: stessa cosa di prima ma con codice sequenza `1`. Se ricevo correttamente torno a `0`.

### rtd 2.2
Ci si accorge che basta un solo tipo di messaggio per riscontrare: ossia l'`ACK`.
In rdt 2.1, `NAK` e' superfluo a causa dell'introduzione dei codici sequenza, ha lo stesso significato di due `ACK` di fila per lo stesso codice di sequenza, quindi puo' essere eliminato semplificando la macchina a stati finiti. I messaggi vengono ritrasmessi quando nello stato `1` il mittente riceve `ACK` con codice sequenza 0 e viceversa.

### rdt 3.0
Ora si assume che il livello sottostante possa perdere pacchetti, come gestiamo questo caso?
Il ricevente rimane lo stesso di rdt 2.2, ma il mittente deve implementare un meccanismo di timeout: scaduto il timer si da per assunto che il pacchetto sia stato perso. Ovviamente pero' il pacchetto potrebbe arrivare dopo il timeout dando vita quindi a ritrasmissioni di pacchetti inutili: bisogna scegliere bene il timeout!

Quello che succede e' che una volta inviato un pacchetto viene avviato un timer, e nello `stato 1` se questo timer scatta prima di riscontrare un `ACK` allora reinvio il pacchetto e rimango in `stato 1`: questo succede per tutti i codici sequenza


# Pipelining

Ci accorgiamo che le prestazioni di `rdt 3.0` sono scadenti, dato che al caso migliore inviamo 1 pacchetto correttamente e poi attendiamo 2RTT prima di spedirne un'altro. Possiamo fare di meglio?

L'utilizzo e' dato dalla formula: $\frac{L/R}{RTT + L/R}$ ossia il rapporto tra il tasso di trasmissione e l'RTT. Spesso l'RTT e' molto alto rispetto a L/R, quindi bisogna trovare una tecnica migliore!

Con il pipelining si vuole mandare $k$ pacchetti insieme, uno dopo l'altro e si aspetta poi di riscontrare gli `ACK` per ogni pacchetto.

Quindi l'utilizzo della banda e' moltiplicato per $k$!

## protocollo  Go Back N
Con il go back n si vuole implementare il pipelining attraverso l'uso di una finestra scorrevole, di grandezza $k$, che si muove sul buffer di invio.
Per tenere conto dei pacchetti riscontrati si usa l'`ACK` cumulativo: ossia $ACK(n)$ e' il valore dell'ultimo pacchetto riscontrato in sequenza (nel senso che ho riscontrato in ordine i pacchetti $1,...,n$ senza saltarne nessuno), per i primi $n$ pacchetti. 
Una volta che ho riscontrato tutti gli $n$ pacchetti posso far scorrere la finestra.

Come mi accorgo dei duplicati? come detto in rdt 3.0, il ricevente inviera' duplicati per il numero di sequenza che ha riscontrato. 

### selective repeat
Invece di usare $ACK$ e timer cumulativi, per migliorare le performance in conessioni rumorose (es. wireless) si preferisce riscontrare gli ACK pacchetto per pacchetto.

Per implementare selective repeat a differenza di go back n c'e' bisogno di tenere in memoria i buffer ricevuti fuori ordine e di attendere quelli mancanti!

## stimare RTT
Per selezionare un buon valore per il timeout occorre stimare l'RTT man mano che vengono inviati i messaggi, facendo un'approssimazione pesata e mobile.

* $estRTT = (1-\alpha)estRTT + \alpha sampleRTT$
* $timeout = estRTT + 4devRTT$. dove $devRTT$ e' un margine di errore calcolato sul momento.

### connessione TCP
si usa 3way handshake con controllo sui numeri di sequenza (scelti in modo che non siano prevedibili).

* `client`: richiesta al server su una porta che accoglie le connesione con`seq=x`
* `server`: manda come risposta syn con `ack=x+1`, `seq=y`
* `client`: manda conferma di connessione con `seq=x+2` e `ACK=y+1`.

A questo punto lo stato della connessione e' ESTAB per entrambi gli host

#### controllo flusso
il controllo del flusso avviene grazie al campo `rwnd` che si trova nell'header tcp. specifica al destinatario, il numero di byte che sono disposto ad accettare nel buffer.
Il controllo del flusso e' necessario perche' l'applicazione potrebbe processare meno dati di quanti ne riceva TCP.

# congestione
Il controllo del flusso e' profondamente diverso dalla congestione: il primo evita di intasare l'endpoint, il secondo evita di intasare il nucleo della rete!

La congestione e' un problema causato da vari scenari tra cui:
* 2 mittenti trasmettono usando $R/2$ del tasso di trasmissione di un router, piu' si satura la connessione e piu **aumenta il ritardo**
* 2 mittenti trasmettono e intasano il buffer: **pacchetti persi**
* in caso di percorsi multi hop, un'host $A$ potrebbe saturare il tasso di trasmissione su un router a discapito di un'host $B$ che e' piu' lontano ->i pacchetti di $B$ si perdono e potenzialmente nessuno arriva a destinazione.
All'aumentare del ritardo e' probabile che si verifichino dei timeout e quindi avro ritrasmissioni su una connessione gia intasata, e se perdo pacchetti otterro lo stesso risultato.

2 tipi di controllo della congestione: 
* `end-to-end`: gli end point deducono lo stato della rete e si regolano di conseguenza.
* `assistito dalla rete`: grazie al protocollo `ICMP` la rete puo' mandare messaggi agli host per segnalare congestione.

### cwnd
la gestione della congestione avviene grazie al campo `cwnd` che viene regolato degli end-point in modo da non intasare la rete.
Ma abbiamo gia il campo `rwnd` che gestisce la finestra, quindi quello che fa l'host e' usare `min{rwnd, cwnd}` per la grandezza della finestra. Il senso e' che se sono disposto a ricevere piu' byte di quanti ne permette il controllo della congestione, allora uso come limite `cwnd`.

Ora vediamo come stimare la `cwnd`

## AIMD
La tecnica prevede di:
* far crescere: di 1MSS per ogni messaggio la `cwnd`
* decrementare: dimezzando ogni volta che rilevo segni di congestione
Da qui il nome dell'algoritmo: incremento additivo e decrescita moltiplicativa

Si possono applicare varie tecniche per migliorare le performance:
* `slow start`: dato che crescere da `0` in modo additivo e' lento all'inizio, si preferisce aumentare solow start duplicandolo di volta in volta (crescita esponenziale) fino a raggiungere congestione.
* `fast retransmit` + `fast recovery`: quando rilevo 3ack di fila ritrasmetto i pacchetti da riscontrare, 

## TCP Cubic
sia $W_{max}$ l'mss misurato prima della congestione, allora:
* mi avvicino velocemente a $W_{max}$
* quando sono vicino cresco molto lentamente
* Quando rilevo perdita scendo di un fattore moltiplicativo

### Collo di bottiglia
Si puo' applicare una tecnica centrata a sfruttare al meglio, senza danneggiare gli altri il collo di bottiglia della connessione (ossia il router congestionato).

Si tiene in memoria l'RTT minimo misurato per poter stimare con $RTT_{min} - RTT$ la congestione sul collo di bottiglia.

L'obiettivo e' tenere il router sempre impegnato, ma senza congestionare causando perdite di pacchetti.


## TCP e' Fair?
Il controllo della congestione, garantisce anche un'uso fair della rete: quando un host rileva congestione, diminuisce il troughtput dando l'occasione agli altri router di aumentarlo