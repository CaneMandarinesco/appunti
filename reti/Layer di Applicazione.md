Il layer di applicazione e' molto ampio in termini di protocolli offerti e variano a seconda di:
* QoS
* TCP o UDP
* L'applicazione sostiene variazioni nel tasso di trasmissione?
* Applicazione Realtime o Differita?
* Parigma Client/Server o Peer To Peer?
* Protocollo tipo pull o push?
* Conessione sicura, criptata?

Questo layer e' implementato solo agli estremi della rete! (Ma anche nel nucleo dato che spesso i fornitori di  servizi per essere efficienti si collocano nel nucleo della rete).

Questo vuol dire che al nucleo della rete non interessa il tipo di pacchetto, perche' il nucleo pensa solo ad inoltrarlo.

Molti protocolli del Layer di Applicazione sono standardizzati: `HTTP, DNS, IMAP, POP3` ma potrei benissimo creare un mio protocollo -> devo decidere come interpretare i bit!

## paradigma server/client
In questo paradigma ci sono due attori: un client che dialoga ,spesso facendo richieste e attendendo risposte, con un server.
In questo paradigma il server non conosce a priori l'indirizzo IP dei client, mentre i client devono conoscere l'IP del server in modo da inviare a questo i pacchetti.

### paradigma peer to peer
Non ci sono client o server, ma solo pari dato che ogni host offre un servizio e riceve servizio dagli altri pari (es: condivisione di un file).

Quindi se un servizio aumenta il numero di peer che partecipano contribuendo alla rete: la scalabilita e' intrinseca. Aumentano i peer = aumenta la quantita di servizio offerto = aumenta la quantita di servizio richiesto. Questo approccio e' inoltre decentralizzato: non c'e' un server principale (o un datacenter) che detiene un'unica copia dei file.

# api
Come realizzo un'applicazione che dialoga con la rete? Si utilizzano le `socket`, ossia un'astrazione che rappresenta una porta virtuale nel computer, attraverso la quale entrano ed escono bit. L'astrazione della `socket` e' offerta ed implementata di solito dal sistema operativo, che si occupa anche di implementare i protocolli `TCP` e `UDP` per il trasporto sottostanti.

# http
E' un protocollo basato su TCP, hostato generalmente su porta 80, senza stato. L'oggetto di richieste e risposte in HTTP sono le risorse web: un'astrazione che comprende immagini, file html, file css, file javascript che vengono gestiti normalmente da un browser.

Ogni risorsa e' identificata da un URL: `www.hostname.it/path/to/resource` costituito da `schema|hostnam|path alla risorsa`.

### rtt
dato che http non e' persistente, la connessione viene chiusa dopo ogni richiesta, quindi ci vogliono almeno `2RTT` per ottenere una risposta, `1RTT` per le prime due fasi dell'handshake. `1RTT` per confermare la connessione da parte del cliente inviando in allegato la richiesta e per ottenere la risposta. Quindi per richiedere 10 oggetti ho bisogno di 20 rtt.

Oppure posso richiedere piu oggetti in una sola richiesta e attendere l'invio di questi uno dopo l'altro.

### richiesta http
Una richiesta http e' formata da una riga di comando, righe di intestazione e un body, il tutto codificato in ASCII, quindi http e' human readable

Si presenta tipo cosi:
```http
GET /path/to/url HTTP/1.1
Host: www.uniroma2.it
UserAgent: LadyBird 0.2
Accept: (media type)
Accept-Language: it
Accept-en: utf8

data...data...data...
```
La prima riga indica il comando che oltre a `GET` puo' essere `PUT,HEAD,POST`. Dove con `POST` si manda input utente codificato nell'URL di solito, `HEAD` e' come `GET` ma dice al server di mandare solo l'header, `PUT` serve  per caricare una risorsa.
* `HEAD`, `GET`, `PUT` sono operazioni `IDEM POTENTI`, ossia hanno sempre lo stesso effetto nel server.
* Ogni riga e' terminata da `\r\n`
* Una riga vuota, ossia contenente solo `\r\n` indica l'inizio del body.

### risposta http
La risposta http e' fatta tipo cosi:
```http
HTTP /1.1 200 OK
Server: 
Date: ieri pomeriggio
Accept-Ranges:
Content-Type:
Content-Length:

...dbody...
```

dove i codici sono:
* `1xx`: informativi
* `2xx`: richiesta soddisfata senza problemi
* `3xx`: il client deve fare altre azioni per soddisfare la richiestae (es. risorsa spostata)
* `4xx`: errore da parte del client, richiesta formulata male
* `5xx`: errore da parte del server, qualcosa e' andato storto!

# cookie: facciamo diventare http con storia 


# domande

## **DOMANDA 1 - EMAIL & SMTP**

Spiega il processo completo di invio di un'email da Alice a Bob, includendo:

- I componenti coinvolti
- Le fasi del protocollo SMTP
- La differenza tra SMTP e HTTP in termini di push/pull
- Cos'è il "dot-stuffing" e perché è necessario?

In Email, in particolare il protocollo SMPT le componenti coinvolte sono i server di posta e non gli utenti che detengono la casella di posta. Una casella di posta deve essere sempre operativa per poter accettare sempre i messaggi e questo meccanismo e' proprio del del server che riceve richieste e le soddisfa. Gli utenti per accedere alla casella di posta e comandare il server devono fare accesso a quest'ultimo tramite un'altro protocollo: `POP3, IMAP, HTTP` (un sito web sul server di posta). Il protocollo `SMPT` e' una connessione TCP permenente (a differenza di HTTP che spesso chiude la connessione appena e' stata soddisfatta la richiesta): il cliente invia comandi tipo `HELO, MAIL FROM, RCP TO` mentre il server ripsonde con codici e descrizioni. I messaggi sono codificati usano ASCII, quindi il protocollo e' umanamente leggibile e interpretabile.

Es di iterazione tra server di posta `A` e server di posta `B`:
* `A` instaura connessione con `B` mandando comando `HELO`
* si spera che `B` risponda con un OK 
* `A` manda il comando `MAIL FROM`
* dopo un `OK` (che corrisponde ad un codice specifico) invia `RCPT TO`
* `B` accetta e chiede ad `A` di inserire il corpo dell'email, dove la sequenza `\n\r.\n\r` indica la fine del messaggio: ossia una riga con un solo punto
* `A` comincia ad inviare riga per riga il messaggio e termina la comunicazione con un `.`
* `A` puo' scegliere se inviare un nuovo messaggio (non come http che ti manda subito a fanculo) oppure di chiudere la connessione
Successivamente all'invio, l'utente che detiene la casella di posta  di destinazione, puo' connettersi al server`B` usando `IMAP`.

`SMTP` e `HTTP` differiscono per il modello di servizio
* in `HTTP` il client spesso richiede al server delle risorse (`pull`)
* in `SMTP` spesso il client (che e' un server) manda messaggi di posta ad un'altro server (`push`)
  
Per `dot-stuffing` si intende la gestione del `.`. Quando si vuole inserire un `.` nel corpo del messaggio, basta inserire una sequenza di due punti, che verra' interpretata come un solo punto. 
## **DOMANDA 2 - DNS**

Descrivi l'architettura gerarchica del DNS:

- Quali sono i tre livelli principali di server DNS?
- Spiega la differenza tra interrogazione iterativa e ricorsiva con un esempio
- Perché il caching è importante e quali problemi può causare?
- Elenca i 4 tipi principali di record DNS e il loro scopo

Risposta
Il DNS e' un protocollo dove le richieste vengono gestite in modo gerarchico: 
* DNS Root Server: sono 13 diversi e replicati in giro per il mondo a centinaia
* DNS Top Level Domain Server: DNS per dominio (es `.net`, `.edu`, `.org`), gestito da un'azienda.
* DNS Server Autoritativo: ogni azienda ha un suo server DNS autoritativo oppure mi appoggio a un 3o che mi offre servizio DNS

Quando faccio una richiesta di traduzione prima contatto un Root Server indicatomi dall'ISP: questo provvedera a dirmi quale DNS Top Level Domain devo contattare per risolvere l'indirizzo. Poi contatto il DNS Autoritativo fornito dal Top Level Domain, questo contiene una tabella dove sicuramente compare l'indirizzo che volevo tradurre.

Questo sistema sebbene sembra contorto e' scalabile. Prima del DNS esisteva un unico file `HOSTS.txt` (come accade nel file unix `/etc/hosts`) dove sono conservate tutte le associazioni `server - IP`, ma questa soluzione non e' scalabile e non e' per niente comodo: oggi `HOSTS.txt` dovrebbe essere enorme, motivo per il quale e' stato abbandonato. 

I root server sebbene replicati a centinaia formano un Single Point of Failure: se qualcuno decide di attaccare i DNS Radice, allora il servizio DNS va giu e non posso piu' connettermi ad internet, per questo deve essere protetto DNS!

Le interrogazioni al server DNS possono essere fatte in modo ricorsivo o iterativo. Di solito quando faccio una richiesta DNS c'e' una cache che lavora al posto mio: controlla se ha in memoria un'associazione valida e me la fornisce, altrimenti effettua la traduzione dell'indirizzo per me contattando i DNS server giusti, ogni intermediario nella traduzione puo' scegliere se:
* ritornare in modo iterativo il prossimo DNS da contattare o l'indirizzo IP (interrogazione iterativa)
* contattare per conto mio il prossimo DNS, e fornirmi quindi 

I DNS hanno tabelle che contengono 4 tipi di associazioni:
* A: traduzione dominio -> IP
* NS: dominio del dns da contattare per il dominio fornito
* CNAME: alias dell'indirizzo IP
* MX: alias per la posta elettronica

Il caching e' utile ma tuttavia potrebbe ritornare associazioni obsolete, o mettersi di mezzo a servizi di traduzione DNS basati sull'utilizzo del servizio o sulla regione (es Reti per Streaming) Ma credo ci siano altri problemi che non ricordo 

La struttura del messaggio DNS e' unica per risposta e richiesta e contiene tra i vari campi un ID per associare richiesta con risposta, si puo' specificare se desidero o no la ricorsione.
## **DOMANDA 3 - P2P & BitTorrent**

Analizza il sistema BitTorrent:

- Qual è il vantaggio della distribuzione P2P rispetto al client-server in termini di scalabilità?
- Spiega le strategie "rarest first" e "tit-for-tat"
- Cosa significa "optimistically unchoked" e perché è utile?
- Calcola: se un server ha banda 10u e ogni client ha banda u, quanto tempo serve per distribuire un file F a N=10 client nei due approcci?

Nell'approccio client-server, con $s$ nodo server, $c$ nodo client, $\micro_{s}$ capacita di upload massima del server in rete, $d_{c}$ capacita del client in download, il tempo che impiegano $N$ client per scaricare un file di $L$ bits e' almeno: il tempo per trasmettere ad  $N$ cliente il file e il tempo piu lungo che impiega un client a scaricarlo.

Invece con peer to peer. il tempo che impiaga la rete a trasmettere il file ad $N$ cliente e' almeno: il tempo minimo che impiega un nodo a caricare in rete, il tempo massimo che impiega un nodo per scaricare, il tempo che impiegano a trasmettere $N$ copie del file usando $K$ peer.

Il fatto e' in peer to peer: all'aumentare dei peer, aumenta il numero di potenziali usufruitori del servizio ($N$), ma aumenta anche il numero di utenti disposti a cedere il servizio ($K$).

In Bit Torrent in particolare un file di grandi dimensioni viene diviso in chunk di dimensione fissa. Per poter condividere il file deve esistere almeno un nodo che ha in memoria tutti i chunk. Quando un peer vuole scaricare i chunk vicini, richiede ad un server (`tracker`) una lista di peer online (piu' o meno vicini a me) e per ognuno di questi posso consultare una lista di chunk in loro possesso per poi richiedere a loro quelli che mi servono.

Si possono adottare le seguenti strategie:
* rarest first: scarico prima i chunk piu' rari, quelli che circolano di meno in rete cosi posso offrirli io agli altri!
* tit for tat: meccanismo nel quale un nodo scambia chunk con altri nodi in base ad una raking list, basata su velocita e numero di chunk inviati da questo. Quello che puo' succedere e' che un gruppo di nodi potrebbe essere ognuno nella top dell'altro e lasciare gli altri nodi soffocati. Per questo motivo tipo ogni 30 secondi si sceglie di inviare chunk a caso ad altri nodi.
* nel ranking dei nodi posso dare priorita ai nodi che condividono piu chunk per spronare gli utenti a condividere chunk invece di essere egosti e scaricare soltanto

Non so che si intende per optimistically unchoked.
Secondo il paradigma client server:
* Il server deve inviare in rete 10 copie del file a tasso $10 u$, quindi: $10 F /10 u$ ossia $\frac{F}{u}$
* I client hanno banda $u$ quindi impiegano: $\frac{F}{u}$ per ricevere.
* In conclusione il tempo minimo per trasmettere e' $\frac{F}{u}$
Nel paradigma peer to peer il tempo di trasmissione e' almeno :
* $max\left\{ \frac{L}{\micro_{min}}, \frac{L}{d_{min}}, \frac{LN}{\text{upload totale}}  \right\}$ che in questo caso e' $\frac{L}{\micro_{min}}$


## **DOMANDA 4 - STREAMING & CDN**

Descrivi l'ecosistema dello streaming video:

- Cos'è DASH e quali vantaggi offre?
- Spiega le due strategie principali delle CDN: "enter deep" vs "bring home"
- Come funziona il processo di accesso ai contenuti CDN (esempio Netflix)?
- Perché serve il buffering e come si gestisce il jitter di rete?

Lo streaming DASH permette di effettuare lo streaming di un file scaricandolo chunk per chunk. I chunk sono raccolti in un file `manifest` dove l'applicazione puo' scegliere i chunk in  base alla risoluzione sostenibile dalla rete (quindi e' adattabile). Puo' essere facilmente distribuito.

Le CDN e' un insieme di database e server per lo streaming, posizionati nella rete in modo da alleggerire il carico del nucleo, e diminuire la latenza delle applicazioni. Ci sono due tattiche principali da adottare:
* installare le CDN dentro alle reti di accesso `enter deep`
* instalalre le CDN in prossimita delle reti di accesso `enter home`
I server della CDN contengono gli stessi contenuti replicati piu e piu volte quindi bisogna scegliere quale server usare tra i tanti disponibili.
Per guidare l'utente nel scegliere il giusto server si puo' sfruttare il DNS: il server autoritativo per il sito risponde forndendo l'indirizzo IP del server piu' vicino e che ha meno latenza all'host.

Il buffering serve per raggirare il jitter: ossia la variazione del ritardo. Il jitter puo' essere piu' o meno alto, e questo vuol dire che non riesco a ricevere sempre on demand i dati per visualizzare l'istante di video per un determinato istante, quindi si preferisce fare il prefetch del video dentro un buffer.

Quando il buffer si svuota richiedo altri dati, e visualizzo il video dal buffer. Questo mitiga l'effetto del jitter: se il buffer e' abbastanza grande e riesco a scaricare piu dati di quanti ne riproduco in tempo reale sto al top!