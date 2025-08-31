Il layer di applicazione e' molto ampio in termini di protocolli offerti e variano a seconda di:
* QoS
* TCP o UDP
* L'applicazione sostiene variazioni nel tasso di trasmissione?
* Applicazione Realtime o Differita?
* Parigma Client/Server o Peer To Peer?
* Protocollo tipo pull o push?
* Conessione sicura, criptata?

> Questo layer e' implementato solo agli **estremi della rete**! 

> **Nota**: al nucleo della rete sono collegati direttamente anche i **fornitori di servizi** (es: Netflix, Google, ecc...) cosi da essere piu vicini agli utenti! 

> Al nucleo della rete **non interessa il tipo di pacchetto**, perche' il nucleo pensa solo ad **inoltrarlo**.

Molti protocolli del Layer di Applicazione sono standardizzati: `HTTP, DNS, IMAP, POP3` ma potrei benissimo creare un mio protocollo, *ossia devo decidere come interpretare i bit*!

## paradigma server/client
In questo paradigma ci sono due attori: 
* un **client** che dialoga ,spesso facendo richieste e attendendo risposte, da un server.
* un **server** che fornisce il servizio e risponde alle richieste. 

> **Nota**: il server non conosce a priori l'indirizzo IP dei client, mentre i client devono conoscere l'IP del server in modo da inviare a questo i pacchetti.

### paradigma peer to peer
Non ci sono client o server, ma solo **pari** dato che *ogni host offre un servizio e riceve servizio dagli altri pari* (es: condivisione di un file).

> **scalabilità intrinseca**: all'aumentare dei peer aumenta il *servizio offerto* ma anche la complessità della rete e il *servizio richiesto*.

> **decentralizzato**: chi offre il servizio e' dislocato nella rete e si muove! non c'e' un **datacenter**!

### api, processi comunicanti e protocolli
La rete si occupa di far comunicare **processi** su end-**point differenti**, grazie ad una `API` di rete, ossia le `socket`. Le `socket` permettono di inviare e ricevere i messaggi del livello di applicazione.

> [!note] socket
> Rappresenta una **porta virtuale** attraverso la quale entrano ed escono bit.
> *E' implementata dal sistema operativo che implementa il protocollo di trasporto sottostante.*

Per inviare un messaggio usando le socket bisogna conoscere dove e' localizzata l'altra socket: ossia l'indirizzo ip dell'end-point e la sua porta.

> Mentre l'indirizzo ip e' variabile, **il numero di porta e' standardizzato**, per esempio i server HTTP si trovano sempre su porta 80.

Ma quale servizio di trasporto conviene usare? Si sceglie in base a:
* tolleranza alla **perdita di dati**
* tolleranza del **ritardo**
* tolleranza della variazione del **troughtput**
* e altre cose che vedremo piu avanti...
# http
* basato su **TCP**
* porta 80
* per richiedere di **oggetti**  o **risorse** Web.
* **stateless**: il server non mantiene uno storico dei messaggi inviati.
* **ASCII**: leggibile dall'utente.

> [!note] risorsa web
> E' un **file** (javascript, html, immagine, ecc...) che viene generalmente *inviato dal server come risposta* al cliente a seguito di una richiesta.
> 
> Ogni risorsa e' identificata da: `schema|hostname|path`, ossia `www.hostname.it/path/to/resource`.

> [!note] RTT
> Tempo impiegato da un pacchetto piccolo per andare dal client e tornare al server includendo tutti i ritardi.

In `HTTP` per ogni richiesta ci vogliono **almeno** `2RTT`:
* uno per la richiesta di connessione `TCP` (`handshake TCP`)
* un'altro per richiedere la risorsa
* piu' il *tempo per trasmettere la risorsa*.

Oppure posso richiedere piu oggetti in una sola richiesta e attendere l'invio di questi uno dopo l'altro.

E' possibile pero:
* aprire piu connessioni insieme
* usare una **connessione persistente**

### richiesta http
* riga di **comando**
* righe di **intestazione**
* **body**: inizia dopo una riga vuota!

> Ogni riga e' terminata da `\r\n`.

Si presenta tipo cosi:
```http
GET /path/to/url HTTP/1.1\r\n
Host: www.uniroma2.it\r\n
UserAgent: LadyBird 0.2
Accept: (media type)
Accept-Language: it
Accept-en: utf8
\r\n
data...data...data...
```
La prima riga indica il comando che oltre a `GET` puo' essere `PUT,HEAD,POST`. 

>Con `POST` si manda input utente codificato nell'URL di solito.

>`HEAD` e' come `GET` ma dice al server di mandare solo l'header, 

>`PUT` serve  per caricare una risorsa.

Nota che: `HEAD`, `GET` e `PUT` sono operazioni `IDEM POTENTI`, ossia hanno sempre lo stesso effetto nel server.

Nell'intestazione troviamo alcuni campi come:
* `Host`: ossia l'hostname a cui ci riferiamo, fondamentale per `web-cache` e `name-based virtual hosting`.
* `User-Agent`: il software (incluso sistema operativo) che sta effettuando la richiesta.
* `Accept`: che tipi di risorse accetto come risposta
* `Accept-Language`: che lingua...
* `Accept-Encoding`: che encoding...
* `Connection`: voglio chiuderla o lasciarla aperta?

### risposta http
La risposta http e' fatta tipo cosi:
```http
HTTP /1.1 200 OK
Date: ieri pomeriggio
Server: 
Accept-Ranges:
Content-Type:
Content-Length:

...dbody...
```

Nell'header troviamo:
* `Date`: data e ora di risposta.
* `Server`: software che esegue il server
* `Last-Modified`: ultima modifica alla risorsa secondo il server
* `Accept-Ranges`: boh
* `Content-Length`: lunghezza in byte dell'entità inviata.
* `Content-Type`: tipo di risorsa!

dove i codici sono:
* `1xx`: informativi
* `2xx`: richiesta soddisfata senza problemi
* `3xx`: il client deve fare altre azioni per soddisfare la richiestae (es. risorsa spostata)
* `4xx`: errore da parte del client, richiesta formulata male
* `5xx`: errore da parte del **server**, qualcosa e' andato storto!

esempi di codici:
* `200`: `OK`
* `301`: `Moved Permanently`
* `400`: `Bad Request`
* `404`: `Not Found`
* `406`: `Not Acceptable`
* `505`: `HTTP Version Not Supported`
## cookie
Si usano per mantenere uno storico delle transazioni usando:
* riga di intestazione nella risposta (`Set-Cookie`)
* riga di intestazione nella richiesta
* *cookie mantenuto sistema dell'utente*
* **database** sul sito

Quando viene fatta una richiesta `HTTP`, se non e' fornito un cookie, il server genera un identificativo per te specificando nella risposta per esempio `set-cookie: 1678`. 

L'utente ricorda il suo cookie per quel sito e il server associa a quell'cookie u*n'identità digitale e crea una voce per te nel suo database*.

I cookie sono usati per:
* gestire autorizzazioni
* ricordare il carrello degli acquisti
* **ricordare preferenze utente**

Es: un sito web di annunci `A`, ti fornisce il cookie da usare per lui. Lo stesso sito web e' inglobato in altre pagine, quindi `A` viene caricato nella tua pagina inconsapevolmente, ed `A` sa chi sei, e quali pagine hai visitato!

`A` potrebbe avere un database del tipo:
```
1678: www.amazon.com/calzini 2/15/22
1678: www.youtube.com/favijtv 2/16/22
ecc...
```
Dove `1678` e' il tuo cookie!

Questo e' un **cookie di terze parti**: ossia ti traccia su piu siti!

## Web Cache
* e' **locale**
* **configurata** dall'utente
* **cattura** le richieste `HTTP` e *risponde al posto del server* se l'oggetto e' in cache
* se non possiede l'oggetto, *lo richiede al server d'origine*. 

Perché' introdurre una cache?
* ridurre tempi di **risposta**, dato che la cache e' vicina al client.
* **scenario**: tra i client e il server c'e' un collegamento che fa da **bottleneck**.
# `E-mail`: `SMPT`
Ci sono tre figure principali:
* **user agent**: scrive, e legge i messaggi, che sono memorizzati su un server.
* **mail server**: ha una `mailbox`e una `coda dei messaggi` da trasmettere, inviati da un `user agent`
* `SMPT`: Simple Mail Transfer Protocol

Il protocollo `SMPT`:
* usa `TCP`, porta 25. quindi `3-way-handshake` e chiusura della connessione.
* **persistente**.
* orientato al **client push**
* `ASCII`

Un'interazione `SMTP` e' tipo cosi:
* `Server:` invia `200`
* `Client`: invia `HELO crepes.fr`
* `Server`: `250 Hello crepes.fr, pleased to meet you`
* `Client`: `MAIL FROM <...>`
* e cosi via...

Con `DATA` il client inizia a inviare il corpo del messaggio, riga per riga.

> **Nota**: Si finisce di trasmettere il corpo quando il client invia una riga contenente solo il simbolo `.`, ossia la sequenza `CRLF.CRLF`

> **Nota**: per inviare un `.` all'inizio di una riga il cliente manda una sequenza di due punti: `..`

Con `SMTP` i messaggi vengono scambiati tra **server**.
Con `IMAP` o con `HTTP` il client ottiene i messaggi dal **server**.

# `DNS`
Vogliamo poter risolvere gli `hostname` in indirizzi ip, per esempio in `/etc/hosts`:
```hosts
185.300.10.1 host1
185.300.10.2 host2 ziocane
185.300.10.3 ...
...
```

Negli anni 70, c'era `HOSTS.TXT`, un'unico file da scaricare per poter risolvere gli host name ma e' una soluzione **poco scalabile** e scomoda per miliardi di dispositivi.

> [!note] DNS
> E' un database distribuito su piu server organizzati **gerarchicamente**.
> E' un **protocollo** a livello di applicazione che permette la risoluzione dei nomi, e' un sistema fondamentale per il funzionamento di internet.

DNS offre:
* traduzione hostname
* host aliasing
* mail server aliasing
* load distribution: re indirizzamento in base al carico.

L'organizzazione gerarchica prevede:
* un'insieme di server **root** 
* dei server **Top Level Domain** (`.com; .it; .en; ecc...`)
* dei server **Autoritativi**: esiste un server autoritativo per i siti di `amazon`, per `google`, ecc.... 

> **Nota**: I DNS Autoritativi, *Sono propri di ciascuna organizzazione, e forniscono i mapping ufficiali da host-name a IP.*

> I `root DNS` sono gestiti dal'`ICANN`.

La richiesta DNS va al `DNS locale` (indicato di default)


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