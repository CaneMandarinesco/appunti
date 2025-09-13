> l Layer di Rete si occupa di **calcolare** e di **effettuare** l'inoltro dei pacchetti ricevuti dal livello di trasporto, fino a destinazione.

> **Piano dei dati**: inoltro del pacchetto nel router attraverso l'uscita corretta
> **Piano di controllo**: calcolo delle tabelle di inoltro di ogni router.

La qualità del servizio di IP e' limitata, ma funziona bene per l'infrastruttura che oggi ha internet.

> [!note] SDN
> La Software Defined Network e' un'approccio dove il piano di controllo viene attuato da un'unico **controllore**, che installa le tabelle di inoltro calcolate nei router della rete.
> **Centralizzato, e semplice da implementare.**

> [!note] Ruter
>  * porte in ingresso 
>  * porte in uscita
>  * **struttura di commutazione**, lavora nell'ordine dei *nanosecondi*, attua il piano dei dati
>  * **processore**, lavora nell'ordine dei millisecondi, attua il piano di controllo

Le porte in ingresso invece sono costituite da: 
* **terminale in ingresso**: livello fisico
* **struttura di eleborazione** del segnale: livello di collegamento
* **buffer**: su cui posso leggere i pacchetti in arrivo

Ora come faccio ad **inoltrare** i pacchetti?
* **inoltro generalizzato**: OpenFlow, stanard con cui inoltro i pacchetti usando l'approccio match+action.
* inoltro su **destinazione**: guardo solo l'IP di destinazione.

Comunque sia prima di inoltrare un pacchetto lo devo analizzare! E lo devo fare velocemente per evitare code sui buffer di entrata! Oppure se sono troppo veloce, o ho troppi pacchetti in input potrei riempire i buffer in uscita Head Of Line blocking,.

> [!note] TCAM
> Immagazzinano **blocchi indirizzi** (espressi con notazione **CIDR**) e li associano ad una porta di uscita.
> Interpellabile in **parallelo**: ottengo una risposta in breve tramite un **priority encoder** e un **decoder del segnale**.

> **Nota**: la commutazione avviene usando il prefisso piu' lungo.

La commutazione dei pacchetti avviene in 3 modi:
* **memoria**: input copiato in memoria e poi riprelevato
* **bus**: un bus comune su cui e' possibile scrivere e leggere
* **rete di commutatori**: le porte di input e output sono collegate mediante una rete di commutatori, in modo da favori comunicazione parallela. Talvolta si usano in parallelo più reti di commutatori. Posso frammentare il pacchetto in modo da farlo andare teoricamente piu veloce!

> [!note] Tasso di trasferimento
> velocità con il quale i pacchetti passano dalla porta di input alla porta di output.

> [!warning] HOL
> Head Of The Line blocking: quando un pacchetto in testa alla coda del buffer non viene trasferito sulla porta in uscita, bloccando gli altri!

> **Nota**: Bisogna scegliere una **Drop Policy** ed una **Scheduling Policy**.

> **Nota**: buffer troppo grandi aumentano l'RTT e dunque TCP diventa **meno reattivo** sul rilevamento della congestione.

> [!note] Marcatura
> Posso marcare i bit con `ECN` o `RED`.
### schedulazione
* **FCFS**: trasmetto in ordine di arrivo
* **Priority**: traffico in arrivo accodato per classi e applico FCFS sulle classi. Puo' indurre a **starvation**.
* **Round Robin**: come priority ma ciclo tra le classi per evitare starvation.
* **Weighted Fair Queuing**: ripartisco ai pacchetti una frazione di banda in base al loro peso.

> **RED**: sceglie in modo probabilistico se **marcare o droppare** i pacchetti in ingresso.

> Tail Drop: scarto i pacchetti appena arrivati.

> **Nota**: i router non implementano `TCP` o `UDP`, ma possono marcare alcuni campi di `TCP` per la congestione.

> **Nota**: anche se introduciamo le classi, *la rete dovrebbe restare neutrale*, e' uno dei capi saldi delle reti.

Formato **datagramma ip**:
![[Pasted image 20250911214010.png]]
le parti piu importanti sono:
* **lunghezza intestazione** e **ToS**: ip puo' fare delle
* **lunghezza del datagramma**
* id
* **flag** e **offset** del frammento per la ricomposizione.
* TTL e livello superiore
* checksum
* ip sorgente e destinazione
* opzioni variabili
* dati: ossia il segmento tcp o il datagramma udp.

> **Nota**: l'overhead e' di 40 byte. Quindi se `MTU=1500`, di questi, `40` sono overhead.

> [!note] MTU
> Limite della grandezza del datagramma che puoi trasmettere.

> Se il bit `DF` e' acceso, allora niente deframmentazione e ottengo un messaggio ICMP di errore.

> **Nota**: `IPv6` deframmentazione deprecata. Devo fare **Path MTU Discovery**.

## Indirizzamento IP
L'indirizzamento `IP`:
* segue la notazione `CIDR`, ossia `x.x.x.x/nn`
* E' **gerarchico**: dispositivi nella stessa rete hanno una stessa parte dell'indirizzo, detta maschera **di sottorete**.
> [!note] suddivisione fissa in classi
> 
Prima di CIDR si usava una suddivisione in classi fissa degli IP:
> * A: inizia con `01` e la maschera di rete e' di 8 bit
> * B: inizia con `011` e la maschara di rete e' di 16 bit
> * C: inizia con `0111` e la maschera di rete e' di 24 bit
> * D per **multicast** mentre E **riservato**

### Dynamic Host Configuration Protocol
Ma come ottengo un indirizzo ip valido, unico nella sottorete? *Semplice, lo chiedo mannaggia la puttana*! Con **DHCP**. 

1. mi collego in rete, ma non ho un indirizzo
2. mando una **richiesta dhcp**: `dhcp discover` in broadcast a tutti, con un **ID personale**
3. un server `dhcp` mi risponde con una `dhcp offer` associata al mio ID
4. mando una `dhcp request` al server con l'IP che mi ha mandato
5. se ricevo un `ack` allora posso usare questo IP per identificarmi nella sottorete

> **Nota**: `DHCP` usa un **ID**.

> **Nota**: `DHCP` usa **UDP**. (banale) 

> [!note] dhcp
> `dhcp discover` $\to$ `dhcp offer` $\to$ `dhcp request` $\to$ `dhcp ack`. **I primi due opzionali**! 

> **Nota**: dhcp puo' fornire varie informazioni: server DNS, maschera di sottorete, router gateway ecc....

La notazione CIDR, oltre che per identificare le sottoreti viene utilizzata per far sapere agli utenti della rete (o meglio al nucleo) dove dovrebbero passare i pacchetti verso un certo indirizzo.

> **Nota**: Ogni ISP e' in grado con i suoi router di **pubblicizzare un range di indirizzi** espresso in notazione CIDR, le altre reti in ascolto possono scegliere se inoltrare o no i pacchetti per quel range di indirizzi verso l'ISP, e l'informazione di conseguenza si propaga in rete.

> Ogni sottorete ottiene l'indirizzo `ip` dall'ISP, e quest'ultimo dall'`ICANN`.

> [!warning] indirizzi riservati nella sottorete
> Nella sottorete l'indirizzo con parte di host 0 e quello con tutti 1 sono riservati a non so cosa e al **broadcast**.
# Non sono pochi 32-bit?
Si, ed e' per questo che viene adottato un controsenso nella pila di protocolli.

> [!note] NAT: il controsenso.
Oltre ad avere DHCP, algoritmi per scartare pacchetti, frammentazione, e altre cose c'e' **NAT**: *un meccanismo che permette agli host di una rete di avere ognuno un'indirizzo distinto nella sottorete*, ma un'unico indirizzo pubblico all'esterno.

NAT si implementa semplicemente tramite una **tabella di traduzione** installata nel **router gateway**.

> **Nota**: ogni richiesta in arrivo dall'esterno deve sostituire ip di destinazione con l'ip della sottorete mentre per ogni richiesta in uscita deve sostituire ip sorgente con quello pubblico.

> **Nota**: ad ogni porta sul router corrisponde una socket su un qualche host.

# IPv6 e tunneling
![[Pasted image 20250911232907.png]]
I campi piu' importanti sono:
* **classe di traffico**
* etichetta di flusso
* **limite di hop**

> **Nota**: manca la checksum, la deframmentazione e le opzioni.

> **Nota**: la notazione corta per `IPv6` e' `ffff:ffff::ffff`.

> [!note] tunneling
> Si usa quando da `IPv6` devo passare per un router `IPv4` tramite due router a doppia tecnologia.
> Il pacchetto `IPv6` viene inserito come payload per il datagramma `IPv4` che passa nel tunnel.

> **Nota**: nel tunneling il pacchetto `IPv4` ha come destinazione l'estremità del tunnel!
# OpenFlow
Seguo l'astrazione `match+action`: ossia trovo una corrispondenza nei bit dei pacchetti e agisco di conseguenza.

> **Azioni**: `drop`, `forward`, `modify` o invia al **controllore**.

> **Disambiguita**': attenzione a pattern sovrapposti!

> **Nota**: per avere un compromesso tra funzionalita e complessita non posso fare `match` su tutti i campi di intestazione.

OpenFlow puo' astrarre:
* Router (`forward`)
* Switch (`flood`)
* Firewall (`permit`, `deny`)
* NAT (`rewrite`)

![[Pasted image 20250911234848.png]]
# Piano di controllo
Due approcci per gli algoritmi di instradamento:
* controllo router per router.
* centralizzato con software defined network.

> *Volgiamo determinare un buon percorso*: ossia di **costo minimo** a patto di rispettare le **policy** degli amministratori delle reti.

> *Si modella il problema come un grafo*, dove gli archi hanno peso **unitario** o **inversamente proporzionale** alla larghezza di banda, o alla **congestione**.

*Classificazione degli algoritmi di instradamento*
> Algoritmi **globali**: ogni router ha una visione globale della rete e calcola in autonomia.

> Algoritmi **decentralizzati**: basato sullo scambio di informazioni con i nodi vicini.

> **Sensibili al carico**: se uso archi proporzionali al carico della rete. Di solito non si usa, difficili da implementare!

> **Dinamici**: reagisco immediatamente a cambi bruschi nella rete (es: un nodo va down)

> **Statici**: meno sensibili
# Dijkstra
> Globale e centralizzato, dunque ogni router esegue una istanza di Dijkstra. Per farlo deve avere un'idea di come e' fatta la rete!

> **Nota**: alla $k$ esima iterazione, ho stimato correttamente la distanza **correttamente** verso $k$ destinazioni.

> **Nota**: complessità algoritmica pari a $O(nlogn)$
> **Nota**: complessità dei messaggi scambiati pari a $O(n^2)$, dato che ogni nodo trasmette in broadcast a tutti lo stato dei suoi collegamenti.

Alla prima iterazione: l'algoritmo ha stimato la distanza da se stesso a 0 e agli altri nodi a **infinito**.

*Ad ogni iterazione estraggo il nodo con distanza stimata minore e* guardo i nodi adiacenti per stimarli nuovamente.Nota

Di conseguenza, con le distanze calcolate, posso calcolare la tabella di inoltro.

> [!warning] oscillazioni
> Se i costi dipendono dal volume di traffico, molto probabilmente nascono **oscillazioni** nei percorsi, ossia minimi *cambiamenti potrebbero stravolgere la tabella di instradamento*.

# Distance Vector
> Decentralizzato, basato sullo scambio di messaggi. Programmazione dinamica ma applico la BF equation *ogni volta che viene modificato lo stato dei vicini*.

inizialmente posso stimare solo i vicini e notifico questi sulle distanze verso i miei vicini, ossia condivido il **vettore delle distanze**. Chi riceve il messaggio valuta se instradare verso di me o se scartare il messaggio.

Il nodo che riceve l'informazione sceglie se instradare verso il nodo sorgente *se' ha una stima migliore e conviene usare il suo collegamento*, altrimenti lo scarta. Se tutti i nodi si scartano l'algoritmo termina, ma ci sono casi in cui l'algoritmo fatica:
* **cambiamenti bruschi**: possono viaggiare lentamente le informazioni
* **cicli** che portano la stima a salire all'infinito
![[Pasted image 20250912134225.png]] ![[Pasted image 20250912134304.png]]

> **Nota**: alla $k$ esima iterazione quando cambio qualcosa nel routing influenzo percorsi lunghi al piu $k$ nodi

> [!warning] poisoned reverse
> Per evitare questo tipo di problemi, prendendo in relazione la 2a foto: $z$ comunica a $y$ che ha distanza $\infty$ da $x$.

Per evitare il conteggio all'infinito rappresentiamo infinito con archi di valore 16.
# Scalare il routing
Abbiamo assunto che tutti i router siano sulla stessa rete ma non e' cosi. La rete e' ripartita in AS (Autonomous System), ossia una sotto-rete di router **sotto la stessa amministrazione** in grado di comunicare con altri AS. 

> **Nota**: ogni AS ha un ASN assegnato dallo IANAC.

> [!note] border gateway
 permette la comunicazione verso l'esterno rispettando le policy degli altri AS.

> [!note] routing
> * intra-AS routing: instradamento all'interno dell'AS.
 >* inter-AS routing: instradamento tra uno o piu AS.

> **Nota**: si utilizza un solo protocollo nell'instradamento intra-AS.

> **Nota**: l'instradamento inter-AS si occupa di capire quali rotte sono raggiungibili guardano gli AS adiacenti.

Ci sono vari protocolli per effettuare il routing INTRA AS :
* RIP: di tipo distance vecotr
* OSPF: applica autenticazione ai messaggi, tipo dijkstra, utilizza archi pesati

> [!note] ospf
> usa **flooding** per l'inoltro dei messaggi (dunque in broadcast) a tutti i router nell'AS, usando direttamente `IP`.
> usa costo degli archi inversamente proporzionale alla lunghezza di bandan e con Dijkstra calcola le tabelle di inoltro.
> I messaggi ospf sono **autenticati**.

> [!note] ECMP (Equale Cost Multi Path)
> In presenza di piu percorsi di costo uguale, faccio `load balancing` tra i `next hop` guardando:
> * **flusso**: hasing sull'intestazione (guardo ip src e ip dst e le porte)
> * hasing sulla **destinazione**
> * per **pacchetto**: ma puo' creare problemi a `tcp` che si aspetta la conse

> [!note] OSPF gerarchico
> Di solito si usa OSPF con un'approccio gerarchico, per scaricare meglio il carico dei messaggi.
> * Ogni nodo conosce la topologia della sua areaa
> * C'e' un'**unica dorsale** dove si trova il router di bordo
> * Ci sono delle **aree locali** collegate alla dorsale
> *  Ogni area locale ha un **router di confine d'area**.
> * *Le aree non sono collegate direttamente*.
# BGP: inter as
Per l'instradamento inter-domain si usa `BGP`:
* permette ad un `AS` di annunciare rotte
* permette con `eBGP` di ottenere informazioni sui sistemi confinanti.
* permette con `iBGP` di propagare l'informazione all'interno dell'AS

> [!note] sessione BGP
> Due router si scambiano messaggi con una `tcp` semipermanente

> [!note] rotta BGP
 Una rotta BGP ha i seguenti attributi:
> * `NEXT_HOP`: il prossimo router che inizia l'`as_path`
> * `AS_PATH`: sequenza di `AS` da attraversare

In caso di piu rotte BGP e' possibile selezionarne una in base a:
* quella che mi libera prima del paccheto, ossia con il `NEXT_HOP` piu leggero
* quella che ha `AS_PATH` piu corto
* in base ad un ID assegnato da BGP

> **Nota**: ogni router di bordo evitare di pubblicitare alcune rotte ricevute se non rispettano le policy.

> **Nota**: `eBGP` prende le rotte dall'esterno e le inoltra con `iBGP` all'interno.

> **Nota**: una rete `dual-homed` e' connessa a piu provider.

> **Nota**: l'organizzazione ad AS nasconde all'esterno come questo sia fatto. Dico solo chi riesco a raggiungere e con quale costo.

> **Nota**: `inter-AS` si concentra sulla policy, mentre `intra-AS` sulle performance.
# controllore SDN
Un'unico controller remote calcola e installa le tabelle in tutti i router.

> **Nota**: generalmente programmare un sistema centralizzato e' piu semplice di uno distribuito.

> **Nota**: con l'inoltro generalizzato posso facilmente decidere qualsiasi tipo di rotta io voglia. Con Distance Vector e' piu difficile.

> **Nota**: l'architettura con controller SDN e' utile per il traffic engeneering.

L'architettura del controllore e' la seguente (top-down):
* livello applicativo, **applicazioni di rete**: esecuzione di algoritmi, load balancing, firewall, ecc...
* SDN controller e il suo sistema operativo
* switch di rete

L'SDN comunica:
* verso l'alto con una API north-bound
* verso il basso con una south-bound (es. OpenFlow)

![[Pasted image 20250912151114.png]]

**Protocollo OpenFlow**: OF oltre che definire i comandi per implementare le `whitebox` mette a disposizione 3 tipi di messaggi che costituiscono il suo Protocollo:
* `controller-to-switch`
* `asynchronous` (switch to controller)
* `symmetric`

Con i seguenti messaggi:
* `features`: interrogo uno switch sulle sue caratteristiche
* `configure`: interrogo e imposto lo switch
* `modify-state`: modifico la tabella OpenFlow
* `packet-out`: specifica la porta da cui esce il pacchetto

> [!warning] esempio
> Quando avviene un guasto e salta un collegamento: un nodo avvisa il controllore con un messaggio OpenFlow. Il sistema operativo del controllore aggiorna le sue informazioni e l'algoritmo di dijkstra viene chiamato dall'API del sistema operativo per eseguire.
> ![[Pasted image 20250912152126.png]]
> L'applicazione che calcola dijkstra accede alle `flow tables` del sistema operativo in modo centralizzato. 
> L'API southbound del sistema operativo si occupa di installarle nei router della rete.

> [!note] Intent-based Networking
> L'amministratore puo' dire al `CDN` che si vogliono raggiungere degli obiettivi: come per esempio una certa latenza tra due nodi, dunque il sistema operativo autonomamente si adopera per farlo.
# ICMP
Usato tra host e router per comunicare informazioni di rete come errori e richieste di ping.

> **Nota**: `ICMP` e' trasportato direttamente dentro `IP` e tecnicamente sarebbe e parte del livello di trasporto ma non e' cosi.

Un mesaggio `ICMP` e fatto di:
* **tipo, codice** e checksum
* intestazione
* intestazione e primi 8 byte del datagramma ip di riferimento

> **Nota**: tipo 3 usato per comunicare **errori** come destinazione non raggiungibile, frammentazione richiesta ecc...

> **Nota**: tipo 11, codice 0 e' `TTL expired`, usato da `traceroute`

> **Nota**: tipo 0, codice 0 e' `ping reply`, `(8,0)` e' `ping request`

> **Nota**: `ICMPv6` e' per `IPv6`. Non esiste l'errore `frammentazione richiesta` ma viene detto: `packet too big`.

> [!note] traceroute
> invia una serie di pacchetti `ICMP` con `TTL` crescente fino ad arrivare a destinazione.
# Network Manager
Le reti, sono grandi, complesse con migliaia di dispositivi, e voglio poterle gestire.

> [!note] Server di gestione
> Raccolta, elaborazione e analisi delle informazioni. Invio di comandi.

Ogni dispositivo di rete gestito ha dei dati che possono essere interpellati da un protocollo di gestione di rete

Posso accedere a questi dati:
* `CLI/UI Web`
* `SNMP/MIB` interrogo un database per richiedere gli oggetti `MIB` usando `SNMP` (Simple Network Management Protocol)
* `NETCONF/YANG`: `YANG` come linguaggio di modellazione dei dati e `NETCONF` per la comunicazione di azioni e dati tra i dispositivi.

> **Nota**: `SNMP` funziona su `UDP`.

 > Nota: la comunicazione avviene con **paradigma richiesta o risposta** da parte del server, o **trap** verso il server.

I `MIB` sono usati come oggetti di un database, dunque esiste un linguaggio di definizione dei dati.

> [!note] MIB
> Ad ogni oggetto corrisponde un `ID`, organizzati in modo gerarchico!
> I valori possono essere **scalari o tabulari**.


## `NETCONF`
Nasce per configurare in maniera attiva i dispositivi di rete, opera 
