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

> Nota: `HTTP/1.1+` usa connessioni persistenti

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

> [!note] `set-cookie`
Quando viene fatta una richiesta `HTTP`, se non e' fornito un cookie, il server genera un identificativo per te specificando nella risposta per esempio `set-cookie: 1678`. 
> Con `cookie` il client indica il suo id.

L'utente ricorda il suo cookie per quel sito e il server associa a quell'cookie u*n'identità digitale e crea una voce per te nel suo database*.

I cookie non sono altro che **File memorizzati lato client** per:  
  - Autenticare l'utente (es. session ID).  
  - Personalizzare la navigazione (preferenze utente).  
  - Tracking (pubblicità).  
  - **Ma con problemi di**: Privacy e sicurezza (Cookie hijacking).

> [!warning] File Cookie
> Il cookie viene salvato in ram, oppure nel file system come un semplice file. L'informazione del cookie lato utente riguarda l'**id piu altri metadati**, *esclusi i dati di personalizzazione o di tracking* che usa il server, quelli li tiene in memoria lui nel suo database!

Es: un sito web di annunci `A`, ti fornisce il cookie da usare per lui. Lo stesso sito web e' inglobato in altre pagine, quindi `A` viene caricato nella tua pagina inconsapevolmente, ed `A` sa chi sei, e quali pagine hai visitato!

`A` potrebbe avere un database del tipo:
```
1678: www.amazon.com/calzini 2/15/22
1678: www.youtube.com/favijtv 2/16/22
ecc...
```
Dove `1678` e' il tuo cookie!

> Questo e' un **cookie di terze parti**: ossia ti traccia su piu siti!

## Web Cache
* e' **locale**
* **configurata** dall'utente
* **cattura** le richieste `HTTP` e *risponde al posto del server* se l'oggetto e' in cache
* se non possiede l'oggetto, *lo richiede al server d'origine*. 

Perché' introdurre una cache?
* ridurre tempi di **risposta**, dato che la cache e' vicina al client.
* **scenario**: tra i client e il server c'e' un collegamento che fa da **bottleneck**.

> **Nota**: con le opzioni `If-Modified-Since` ed `ETag` evito di riscaricare la risorsa! `get-condizionale`.
# `E-mail`: `SMPT`
Ci sono tre figure principali:
* **user agent**: scrive, e legge i messaggi, che sono memorizzati su un server.
* **mail server**: ha una `mailbox`e una `coda dei messaggi` da trasmettere, inviati da un `user agent`
* `SMPT`: Simple Mail Transfer Protocol
* `IMAP`: Internet Mail Access Protocol

Il protocollo `SMPT`:
* usa `TCP`, porta 25. quindi `3-way-handshake` e chiusura della connessione.
* **persistente**.
* orientato al **client push**
* `ASCII`

Un'interazione `SMTP` e' tipo cosi:
* `Server:` invia `200`
* `Client`: invia `HELO crepes.fr`
* `Server`: `250 Hello crepes.fr, pleased to meet you`. Qui si conclude l'**handshaking** `SMPT`.
* `Client`: `MAIL FROM <...>`
* e cosi via, fino alla chiusura della connessione

> **Nota**: i comandi piu usati sono `HELO`, `MAIL FROM`, `RCPT TO` e `DATA`.

> [!note] `dot-stuffing`
> Sequenza di `..`, usata per evitare che un singolo `.` a inizio riga sia interpretato come fine del corpo della mail.

![[Pasted image 20250906152153.png]]

Con `DATA` il client inizia a inviare il corpo del messaggio, riga per riga.

> **Nota**: Si finisce di trasmettere il corpo quando il client invia una riga contenente solo il simbolo `.`, ossia la sequenza `CRLF.CRLF`

> **Nota**: per inviare un `.` all'inizio di una riga il cliente manda una sequenza di due punti: `..`

Con `SMTP` i messaggi vengono scambiati tra **server**.
Con `IMAP` o con `HTTP` il client ottiene i messaggi dal **server**.

Con `MIME` posso includere nel corpo della mail oggetti più complessi, come codice `html`.

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
> 
> E' un **protocollo** a livello di applicazione che permette la risoluzione dei nomi, e' un sistema fondamentale per il funzionamento di internet.
> 
> E' un servizio **best effort**: fa il meglio che può, ma la risposta puo' essere errata!

DNS offre:
* traduzione hostname
* host aliasing
* mail server aliasing
* load distribution: re indirizzamento in base al carico.

L'organizzazione gerarchica prevede:
* un'insieme di server **root** 
* dei server **Top Level Domain** (`.com; .it; .en; ecc...`)
* dei server **Autoritativi**: esiste un server autoritativo per i siti di `amazon`, per `google`. E' gestito dall'azienda stessa che detiene il sito, oppure da un service provider.

> **Nota**: I DNS Autoritativi, *Sono propri di ciascuna organizzazione, e forniscono i mapping ufficiali da host-name a IP.*

> I `root DNS` sono gestiti dal'`ICANN`.

> [!note] `DNS` locale
> E' indicato dall'ISP e viene contattato di default, non appartiene necessariamente alla gerarchia `DNS`.

Le query `DNS` possono essere **iterative**: ogni server mi risponde dicendo chi devo contattare per risolvere l'hostname, finche' non ottengo come risposta l'indirizzo ip.

Altrimenti possono essere **ricorsive**: ossia e' il server a cui richiedo di risolvere l'hostname a fare richiesta ad altri server per la risoluzione se non sa rispondermi.

> **Nota**: i server possono fare `caching` per ritornare immediatamente il mapping. Ogni voce come al solito ha `TTL`

Nel database `DNS` sono contenuti i record di risorsa: **RR** nella forma `(name, value, type, ttl)`, e si differenziano per `type`:
* `A`: `name, value` sono rispettivamente `hostname, ip`
* `NS`: `name, value` sono rispettivamente *dominio di appartenenza e server da contattare*
* `CNAME`: in caso di **alias**.
* `MX`: `value` e' il server di posta associato a `name`.

Con `nslookup` e `dig` posso interrogare i server dns da riga di comando.

> La sezione `authority` contiene i record `NS` che indicano i server autoritativi relativi alla risposta. Per contattarli poi devo guardare nella sezione `additional` che contiene i record `A/AAAA`.

> Quando interrogo un **root server** con `nslookup -norecurse -nosearch -debug -type=A we.uniroma2.it` non ottengo direttamente l'IP in quanto non sono autoritativi per `web.uniroma2.it` e ottengo i referral per `.it`.

> Se la risposta e' autoritativa per il server contattato allora `AA=1` (Authoritative Answer)

> Se il dominio non esiste ottengo `NXDOMAIN`

> Nota: il google dns e' un resolver ricorsivo pubblico. Risponde solo con ricorsione dunque o se ha la risposta in cache, dunque se mando una richiesta senza ricorsione ottengo solo il risultato dalla cache.

> Il valore di preference nei record ha preferenza dal piu basso al piu alto.

> `resolv.conf` indica i nameserver da contattare

> `dig +trace` fa tutte le richieste iterative per risolvere il nome/.   

Domande e risposte hanno lo stesso formato:
![[Pasted image 20250906153648.png]]
* `identificazione`: un id per associare domande e risposte.
* `flag`: il messaggio e' domanda o risposta? voglio la ricorsione? sono un `DNS` autoritativo?
* `sezione delle risposte`: contiene l'**RR**
* `sezione autoritativa`: contiene record per i server autoritativi!

Per aggiungere il mio sito al `DNS` devo, per esempio `diocane.com`:
* aggiungere il record `(diocane.com, dns1.diocane.com, NS)` nel `TLD` per `.com`.
* aggiungere il record `(dns1.diocane.com, 212.212.212.212, A)` 

# architettura p2p
In questo paradigma:
* non c'e' un server sempre attivo.
* sistemi periferici arbitrari che comunicano direttamente tra di loro.
* i sistemi periferici cambiano continuamente ip.

Nel paradigma client-server, per distribuire un file di $F$ bit da un server a $N$ peer:
$$
D_{c-s} \geq max \{NF/u_{s,}F/d_{min}\}
$$
ossia il massimo tra:
* tempo che impiega il serve a fare l'upload verso $N$ client di $F$ bit con $u_s$ banda.
* tempo massimo che impiega un client a scaricare $F$ bit.

Invece in peer2peer, supponendo che $F$ sia inizialmente immagazzinato dentro un ipotetico server:
$$
D_{p2p} \geq max \{ \frac{F}{u_{s}}, \frac{F}{d_{min}}, NF/(u_{s} + \sum\limits u_i)\}
$$ 
ossia il massimo tra:
* tempo di upload del server
* tempo di download piu lento
* il tempo che impiegano *i client come aggregato a inviare in rete*. Aumenta linearmente in $N$, ma all'aumentare di $N$ aumenta anche la capacita di upload

Come funziona la trasmissione di file?
* **tracker**: tiene traccia dei peer attivi che partecipano al `torrent`, ossia il gruppo di peer che condivide un file.
* **chunk**: il file e' diviso in segmenti
* **peer nuovo**: richiede al tracker una lista di peer a cui richiedere chunk. Mentre scarica, invia ad altri quelli che ha
* **seeder**: peer che rimangono in rete per condividere il file.

tattiche da usare nella condivisione:
* **rarest first**: scarico prima i chunk piu rari in circolazione.
* **ranking**: stilo una classifica dei migliori chunk con cui comunicare.
* **tit-for-tat**: `A` tramite il suo ranking decide di mandare i chunk ai peer vicini che trasmettono alla velocità maggiore, ma allora gli altri peer sono **soffocati** da `A`! Dunque, bisogna fare in modo che ogni peer abbia la possibilità di salire nella classifica di altri nodi.
# streaming 
 Nel contesto odierno, lo streaming di contenuti multimediali usa un botto di traffico: come faccio a soddisfare questa grande richiesta senza mandare tutto a fanculo?

> [!note] Codifica dei frame efficiente.
> Si può utilizzare la **ridondanza** che c'e' **tra un frame e l'altro** per mandare solo le parte di video che e' effettivamente cambiata, oppure la ridondanza puo' presentarsi nell'**immagine stessa** (più pixel vicini dello stesso colore).

> **VBR**: Variable Bit Rate. Un formato video e' supporta VBR se puo' essere codificato riducendo la ridondanza.

> **Jitter**: in rete la larghezza di banda varia nel tempo e i pacchetti possono essere persi o ritrasmessi piu volte. Si controlla utilizzando un **buffer** lato utente.

Lo streaming puo' essere:
* `UDP`: *ossia invio pacchetti fino ad **egualiare** il bit rate del video*, ma la rete deve essere in grado di sostenere questo carico.
* `HTTP`: *trasmetto alla massima velocita, eventualmente fino a **riempire** il buffer del client*, poi con il controllo del flusso diminuisco. Potendo inviare tanti dati se mediamente trasmetto tanto, posso compensare un jitter alto.
* `DASH`: Dynamic Adaptive Streaming over **H**TTP.

> [!note] DASH: manifest file
> Ogni versione del video viene divisa in **blocchi**, ognuno di questi blocchi e' indirizzabile a partire dal `manifest file` del video, ossia un file `XML` che contiene gli url per raggiungere i vari blocchi.
> 
> Dunque il **client** puo' *richiedere i blocchi che gli servono e al bit-rate (versione) che puo' sostenere*.

Come distribuisco questi contenuti? Come posiziono fisicamente i server?

Installo `CDN` (Content Distribution Networks) geograficamente distribuite:
* `enter deep`: installo vicino alle reti di accesso degli utenti
* `bring home`: in prossimità degli `IPX`.

> Una `CDN` e' un **cluster di server** e database che contengono tutte gli stessi media.

# esercizi
* `nc`: usato per aprire connessioni `TCP` e quindi posso scrivere manualmente una richiesta `HTTP`.
* `curl`: e' un client `HTTP`, e mostra l'entità richiesta.

Nelle richieste `HTTP`:
* Devo sempre specificare `Host:` per un indirizzo `ip` ci potrebbero essere diversi server/siti web associati, ma identificati da un determinato `host`.
* il server mi dice che `cookie` usare se non lo ho specificato nella richiesta con `Cookie:`
* Il server specifica il cookie con `Set-Cookie:`
* Nella prima riga specifico la risorsa! `GET /path/to/res HTTP/1.1`
* usando `<<EOF`, ossia l'here document posso inviare piu richieste in **pipeline**
* `curl -v` mostra dati aggiuntivi come gli header e indirizzi `ip`.
* `curl -F "opt1=diocaro" -F "opt2=diobono www.diocane.org"` per mandare una `POST`
* `curl -O fotoporno.jpg` per salvare il body in un file
* `curl -v -b cookie.txt -c cookie.txt www.pornhub.com` per salvare e riutilizzare il cookie in `cookie.txt`


Note generali su `DNS`:
* `DNS Resolver`: dns locale dell'isp che risolve gli hostname
* Ad un dns potrebbero essere associati piu `ip` per ovviare al fatto che uno potrebbe essere momentaneamente non disponibile (`down` o alto carico di traffico)
* `GeoDNS`: risolvo l'indirizzo ip in base alla posizione geografica di chi fa richiesta
