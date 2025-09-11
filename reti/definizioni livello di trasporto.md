**Servizi del livello di trasporto**: Connessione **logica** tra due processi, che in generale si trovano su due end-point diversi. La connessione logica e' offerta anche dal livello sottostante, ma solo tra `end-point`, occorre effettuare il multiplexing/demultiplexing dei pacchetti in transito da e verso la giusta socket. In base al protocollo scelto il livello di trasporto si assicura che i pacchetti arrivino a destinazione. 

**Implementazione del livello di trasporto**: implementato direttamente nel sistema operativo! Esiste una parte del kernel di linux deputato a gestire le connessioni `TCP`. 

**Multiplexing**: I pacchetti sono raccolti dalle socket e passati al livello di rete incapsulandoli nel segmento `TCP` o nel datagramma `UDP` aggiungendo porta sorgente e porta di destinazione.

**Demultiplexing**: I pacchetti raccolti dall'esterno vengono consegnati alle giuste socket guardando porta sorgente e porta di destinazione.

**IP**: protocollo (???) che si occupa di effettuare l'inoltro dei pacchetti da un end-point all'altro. E' un servizio best-effort: esserci degli imprevisti che portano alla perdita del paccheto

**UDP**: protocollo di trasporto senza fronzoli, non aggiunge pressoche' nulla rispetto a IP, dunque il pacchetto potrebbe essere perso. Offre solo multiplexing e demultiplexing del pacchetto e una checksum in complemento ad 1.

**Segmento UDP**:

| 16-bit         | 16-bit    |
| -------------- | --------- |
| source port    | dest port |
| checksum       | **len**   |
| dati.dati.dati | dati.dati |
Nonostante la presenza di `source port`, questo campo non viene usato nel demultiplexing, ossia non esistono socket dedicate al client. Tuttavia, e' presente per permettere al server di sapere a quale porta sull'end-point deve rispondere.

**Checksum UDP**: l'intero datagramma da mandare, privo di checksum viene visto come una sequenza di 16-bit da intepretare come interi in complemento ad 1. Bisogna calcolare il campo checksum come quel numero che sommato alle altre sequenze mi da `1111 1111 1111 1111`, ossia una delle due rappresentazioni dello 0 in complemento ad 1. Quando si ha un riporto sull'ultimo bit, questo viene portato alla prima cifra.

**TCP**: protocollo di trasporto che rispetto ad UDP, offre controllo della congestione, controllo del flusso (rispetto a quanto l'altra parte e' disposto ad accettare), ritrasmissione pacchetti persi, connessioni dedicate tra client e server.

**Trasmissione affidabile**: vogliamo costruire una macchina a stati finiti che descriva il modo in cui mittente e ricevente si sincronizzano in modo di essere sicuri che il ricevente non perda nessun messaggio.

Useremo le primitive:
* `rdt_send`, chiamata dall'alto nel mittente
* `udt_send`, chiamata verso il basso nel mittente
* `rdt_rcv`, chiamata dal basso nel ricevente
* `deliver_data`e, chiamata verso l'alto nel ricevente
E assumeremo che tra `udt_send` e `rdt_rcv` ci sia un canale di comunicazione piu o meno affidabile.

**rdt 1.0**: Assumiamo che il canale di comunicazione sia perfetto, allora le macchine a stati finiti si occupano solo di assemblare e disassemblare i pacchetti.

**rdt 2.0**: Assumiamo che il canale possa invertire dei bit, allora abbiamo bisogno di inserire due messaggi speciali nel protocollo: ACK e NAK in modo che il ricevente possa comunicare al mittente, dopo aver controllato la checksum se il messaggio arrivato e' corrotto!'

Codice del mittente:
```python
var sndpkt
on state0:rdt_send(data)
	sndpkt = make_pkt(data)
	udt_send(sndpkt)
	goto state1

on state1:rdt_rcv(pkt)
	if iscorrupt(pkt) || isnak(pkt):
		udt_send(sndpkt)
	if isack(pkt):
		goto state0
```

Codice del ricevente:
```python
on state0:rdt_rcv(pkt)
	if iscorrupt(pkt)
		rdt_send(NAK)
	else
		rdt_send(ACK)
		data = unpack(pkt)
		deliver_data(data)
```

Il problema fatale di rdt 2.0 sta nel fatto che se arriva un `ACK` corrotto, se lo ritrasmetto il ricevente accetta due volte lo stesso messaggio. Proviamo a sistemare sta cosa con rdt 2.1

**rdt 2.1:**  Aggiungiamo dei numeri di sequenza da associare ad ogni pacchetto, e cilciamo usando aritmetica modulare su ognuno di questi per ogni pacchetto arrivato correttamente a destinazione. Usiamone solo 2 per ora.

Il mittente iniziera inviando il primo pacchetto con codice 0, e attendera' di conseguenza un'ACK o NAK con codice 0. Se ho un ACK passo allo stato della macchina con codice 1 e eventualmente ritorno ad 1. TOP! Cosi so a quale pacchetto si riferiscono i NAK o gli ACK e posso gestirli se corrotti perche' mittente e ricevente si sincronizzano grazie ai numeri sequenza


**rdt 2.2**: NAK e' superfluo, puo essere eliminato per semplificare il protocollo. NAK ha lo stesso significato di due `ACK` ricevuti per lo stesso pacchetto. Il mittente al secondo `ACK` ricevuto sborrera!

**pipelining**: con rdt 3.0 al caso migliore l'utilizzo effettivo del tassoo di trasmissione e' $\frac{L/R}{L/R + RTT}$, che per RTT elevati e' pessimo. Quindi si preferisce mandare piuttosto $k$ pacchetti di seguito, in modo da compensare il valore dell'RTT.

**portocollo goback N**: come gestisco il buffer di invio usando il pipelining? Utilizziamo una finestra scorrevole sul buffer che avanza ogni qual volta riscontro in ordine i pacchetti. I pacchetti devono essere riscontrati in ordine, quelli che non lo sono vengono scartati!

**Pacchetti nella goback N**: possono essere riscontrati, in attesa di riscontro, e in attesa di essere inviati ed eventualmente potrebbero non esserci piu pacchetti da inviare nel buffer.

`ACK(n)`: numero piu alto di sequenza per cui ho un ACK nei primi $n$ pacchetti.

**rcv_base**: primo pacchetto non riscontrato
**rcv_limit**: ultimo pacchetto nel buffer 

$timer(n)$: quando scatta, ritrasmetto tutti i pacchetti <= n non riscontrati 

**selective repeat**: posso decidere di tenere in memoria gli ack non in oridine e reinviare con $timer(n)$ tutti i pacchetti non riscontrati in precedenza, faccio comunque scorrere la finestra solo quando ricevo gli ack in ordine.
Devo dunque mantenere un timer per ogni numero di sequenza, e gli ack non sono piu cumulativi!

**scegliere la grandezza della finestra e i numeri sequenza**: in generale $w \geq 2m$ dove $w$ sono i numeri di sequenza e $m$ la grandezza della finestra di mittente e ricevente. Il senso e' che vogliamo evitare il malaugurato caso in cui i numeri di sequenza possano accavallarsi.







