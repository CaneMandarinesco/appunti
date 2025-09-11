> l Layer di Rete si occupa di **calcolare** e di **effettuare** l'inoltro dei pacchetti ricevuti dal livello di trasporto, fino a destinazione.

> **Piano dei dati**: inoltro del pacchetto nel router attraverso l'uscita corretta
> **Piano di controllo**: calcolo delle tabelle di inoltro di ogni router.

La qualità del servizio di IP e' limitata, ma funziona meglio per l'infrastruttura che oggi ha internet.

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

> **RED**: sceglie in modo probabilistico se marcare o droppare i pacchetti in ingresso.

> Tail Drop: scarto i pacchetti appena arrivati.

> **Nota**: i router non implementano `TCP` o `UDP`, ma possono marcare alcuni campi di `TCP` per la congestione.

> **Nota**: anche se introduciamo le classi, *la rete dovrebbe restare neutrale*, e' uno dei capi saldi delle reti.

Un datagramma IP e' costituito dai seguenti campi:
* ToS: 2 byte descrivono la priorita del pacchetto e il tipo
* Lunghezza dell'intero datagramma
* Checksum solo dell'intestazione
* Bit DF, Offset, e ID: servono per la frammentazione del pacchetto
* ip sorgente
* ip destinazione
* sequenza variabile di byte
* Dati!

MTU: esiste un limite alla grandezza di un datagramma che puo' essere trasmesso in un collegamento, di solito e' tipo `1500`.

Se saturo MTU IP permette di frammentare i dati all'intero del pacchetto per portarlo a destinazione. Nota che dei 1500 byte standard 40 (o piu) sono usati solo dall'header TCP+IP. 

Per decompore e ricomporre il pacchetto sono usati l'ID dei pacchetti e l'offset. Se DF e' acceso allora la frammentazione non e' possibile e ricevo un messaggio ICMP dal router.

IPV6 non permette la frammentazione del pacchetto: devo trovare la MTU del collegamento a destinazione, per farlo uso un valore probabile per MTU e se ricevo messaggio di errore allora lo devo abbassare.

## Indirizzamento IP
Viene fatto usando la notazione CIDR. Internet e' una rete di rete e gli indirizzi IP sono gerarchici in quanto interfacce di rete nella stessa rete hanno la stessa sottorete. La parte di sottorete di un indirizzo ip e' scritta cosi secondo notazione CIDR: `40.2.10.80/20`: vuol dire che i primi 20 bit dei 32 costituiscono la sottorete e tutte le interfacce hanno gli stessi primi 20 bit in comune.

Prima di CIDR si usava una suddivisione in classi fissa degli IP:
* A: inizia con `01` e la maschera di rete e' di 8 bit
* B: inizia con `011` e la maschara di rete e' di 16 bit
* C: inizia con `0111` e la maschera di rete e' di 24 bit
* D per multicast mentre E riservato

### Dhcp
Ma come ottengo un indirizzo ip valido, unico nella sottorete? Semplice, lo chiedo mannaggia la puttana! 

1. mi collego in rete, ma non ho un indirizzo
2. mando una richiesta dhcp: `dhcp discover` in broadcast a tutti, con un ID personale
3. un server `dhcp` mi rispondera con una `dhcp offer` associata al mio ID
4. mando una `dhcp request` al server con l'IP che mi ha mandato
5. se ricevo un `ack` allora posso usare questo IP per identificarmi nella sottorete

dhcp puo' fornire varie informazioni: server DNS, maschera di sottorete, router gateway ecc....

La notazione CIDR, oltre che per identificare le sottoreti viene utilizzata per far sapere agli utenti della rete (o meglio al nucleo) dove dovrebbero passare i pacchetti verso un certo indirizzo.
Ogni ISP e' in grado con i suoi router di pubblicizzare un range di indirizzi espresso in notazione CIDR, le altre reti in ascolto possono scegliere se inoltrare o no i pacchetti per quel range di indirizzi verso l'ISP, e l'informazione di conseguenza si propaga in rete.
# Non sono pochi 32-bit?
Si, ed e' per questo che viene adottato un controsenso nella pila di protocolli.

Oltre ad avere DHCP, algoritmi per scartare pacchetti, frammentazione, e altre cose c'e' NAT: un meccanismo che permette agli host di una rete di avere ognuno un'indirizzo distinto nella sottorete, me un'unico indirizzi pubblico all'esterno.

NAT si implementa semplicemente tramite una tabella di traduzione installata nel router gateway: ogni richiesta in arrivo dall'esterno deve sostituire ip di destinazione con l'ip della sottorete mentre per ogni richiesta in uscita deve sostituire ip sorgente con quello pubblico.

Si mantiene quindi in memoria questa tabella a cui attingere per la traduzione: ad ogni coppia di IP, Porta corrisponde una coppia IP, Porta per l'esterno essenzialmente ad ogni Porta del router di gateway corrisponde una socket in un host.

# IPv6 e tunneling
E' composto da:
* sorgente: 16 bit
* destinazione: 16 bit
* identificatore di flusso
* limite di hop
* bit per QoS

ipv6 non permette la frammentazione del pacchetto, ma soprattutto... se devo passare per un router IPV4, che non supporta IPV6 come faccio? 

Si usa il tunneling: quando effettuo l'instrandamento mi accorgo che dovro passare innanzitutto per un router a doppia teconlogia $A$, che sia in grado di prendere il datagramma ipv6 e di ficcarlo come payload di un datagramma ipv4 con ip destinazione il prossimo router a doppia tecnologia $B$ che mi permette di portare il pacchetto a destinazione $C$. Quando $B$ riceve il pacchetto (non e' detto dopo un solo `hop`) riprende il datagramma ipv6 e sa che lo deve spedire a $C$ (e' scritto nell'intestazione da $A$).

La notazione per i pachetti ipv6 e' 8 sequenze di 4 caratteri esadecimali separate da `:`. la notazione `x:x::x` indica che tra la seconda coppia e l'ulitma di caratteri ci sono degli zeri. 

# Inoltro generalizzato
Per farlo si usa OpenFlow. E' uno standard facile da implementare bastato sull meccanismo match -> action. E' possibile rilevare vari tipi di match su un campo IP, banalmente IP src, IP destinazione, sul tipo, sull'intestazione TCP e' scegliere un'azione tra: drop, control, forward e modify

OpenFlow e' l'astrazione di una qualsiasi middle box in rete: per esempio un sistema OpenFlow puo' essere un router, uno switch di rete, un firewall ecc...

# Algoritmi di instradamento
Ora bisogna calcolare le tabelle di inoltro da installare nel nucleo della rete: come faccio? Si modella il problema come un grafo di archi di peso unitario dove si vuole cercare lo shortest path da un nodo, e tutto questo tenendo conto delle policy degli amministratori delle reti.
Di solito si usano pesi unitari, oppure: archi inversamente proprozionali alla larghezza di banda, oppure archi proporzionali alla congestione su quel collegamento.

Gli algoritmi si classificano in:
* Globali o decentralizzati: se globale ogni router ha una visione globale della rete, se decentralizzato il router non  calcola da solo la tabella di inoltro ma piuttosto scambia e riceve informazioni dai vicini
* Sensibile al carico: se gli archi sono proporzionali al carico della rete, si preferisce di no
* Dinamici o statici: reagisco subito, in modo dinamico ai cambiamenti che possono avvenire sulla rete?

# Dijkstra
e' un'algoritmo globale, centralizzato. Ogni router esegue dijkstra per fatti suoi e calcola la sua tabella di inoltro.

La complessita temporale dell'algoritmo e' $O(nlogn)$ mentre per ogni nodo partono $O(n)$ messaggi in broadcast per informare tutti delle proprie connessioni.

L'algoritmo funziona cosi: il router $s$ stima la distanza da se stesso a 0 e gli altri a infinito. Ad ogni iterazione viene estratto il nodo con distanza stimata $s$ minore, e si guardano i nodi adiacenti a questo e si cerca di dare una stima migliore ai nodi non estratti.

L'algoritmo termina sborrando
# Distance Vector
E' un algoritmo decentralizzato basato fortemente sullo scambio di messaggi. Utilizza la tecnica di programmazione dinamica, ma nella sua versione decentralizzata la formula viene applicata ogni volta che un nodo riceve informazioni sulle distanze dal nodo adiacente.

Il nodo che riceve l'informazione sceglie se instradare verso il nodo sorgente, perche' ha una stima migliore e conviene usare il suo collegamento, oppure lo scarta perche' non conviene. Se tutti i nodi si scartano l'algoritmo termina, ma ci sono casi in cui l'algoritmo fatica:
* cambiamenti bruschi: possono viaggiare lentamente le informazioni
* cicli che portano la stima a salire all'infinito

# Scalare il routing
e' stato assunto che tutti i router sono su una stessa rete, ma non e' cosi: per alleggerire il carico e per scalare meglio il sistema conviene avere delle sottoreti comunicati: ovvero degli Autonomous Systems.

Ogni AS  ha un ASN assegnato dallo IANAC, ed e' una collezione di router collegati tra loro che comunicano verso l'esterno grazie a dei border gateway router, che sono gestiti da un'azienda che ha delle policy da applicare a questi.

Un pacchetto che viaggia in un AS potrebbe aver bisogno di:
* uscire dall'AS, si parla quindi di InterAS routing, devo passare per forza per un gateway router.
* raggiungere un'altro nodo nell'AS: IntraAS routing.

Ci sono vari protocolli per effettuare il routing INTRA AS :
* RIP: di tipo distance vecotr
* OSPF: applica autenticazione ai messaggi, tipo dijkstra, utilizza archi pesati
Comunque si scelga un algoritmo si incappera spesso in un'opportunita: cosa faccio quando ho piu path verso un nodo che hanno stessa lunghezza? Posso sfruttarli tutti e due applicando ECMP, ma come divido i pacchetti sui due path
* per ip destinazione 
* per appartenenza di flusso
* hasing sul pacchetto? Prendere strade diverse potrebbe causare l'arrivo fuori ordine dei messaggi.

Di solito si usa OSPF con un'approccio gerarchico, ossia, per scaricare meglio il carico computazionale dei messaggi, la rete e' divisa in dorsale e aree:
* la dorsale usa dei router per comunicare con le aree
* la dorsale e' l'unica ad essere connessa all'esterno
* ogni area comunica solo cono la dorsale
* lo scambio di messaggi e' limitato alla sola area

Serve a limitare il flooding.

# BGP: inter as
per fare il routing inter as si usa di solito BGP che e' diviso in due protocolli:
* iBGP: per propagare l'informazione
* eBGP: per annunciare l'informaizone all'esterno
Le rotte possono essere propagate da eBGP e accettate se e solo se rispettano le policy.

Una rotta BGP ha i seguenti attributi:
* `NEXT_HOP`: il prossimo router che inizia l'`as_path`
* `AS_PATH`: sequenza di `AS` da attraversare

In caso di piu rotte BGP e' possibile selezionarne una in base a:
* quella che mi libera prima del paccheto, ossia con il `NEXT_HOP` piu leggero
* quella che ha `AS_PATH` piu corto
* in base ad un ID assegnato da BGP

# controllore SDN
L'architettura del controllore e' la seguente (top-down):
* livello applicativo: esecuzione di algoritmi, load balancing, firewall, ecc...
* hardware del controller
* switch di rete
l'hardware comunica con il livello applicativo (veri e propri programmi in esecuzione sul controller, spessi scritti in python o java), grazie ad un'API northbound e comunica con gli switch di rete usando un'API southbound (es: protocollo OpenFlow)

Questo sistema permette di fare i calcoli sul controller e di installare sui switch la computazione in modo modulare.

Il protocollo OpenFlow prevede tre classi di messaggi
* controller to switch: forward, modify, configure, features
* asynch
* symmetric
# ICMP
e' un porotocollo fondamentale per il layer di rete: si occupa di informare host e router sullo stato della rete.

Ogni messaggio del protocollo ha un tipo:
* `3`: destinazione non raggiungibile
* `4`: congestione
* `8/10`: per fare ping request e ping reply.
* `11`: TTL scaduto

# Network Manager
Dato che le reti sono grandi e' complesse vogliamo ottenere delle statistiche, ed interpellare e controllare gli switch di rete. Per farlo ci sono vari protocolli.

## SNMP
Database ad ogetti: interpello i MIB per ottenere informazioni. Spesso sono organizzati gerarchicamente, ogni MIB ha un identificatore e dei dati associati
