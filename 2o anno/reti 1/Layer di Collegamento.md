> Questo livello offre:
> * **framing** dei datagrammi a seconda del protocollo
> * **Error detection**: ed eventuale meccanismo di recupero dagli errori
> * trasmissione del frame sul mezzo di trasporto
> * regolazione del flusso
> * trasmissione `half-duplex` o `full-duplex`

> **Nota**: livello di collegamento e fisico implementato nelle **interfacce di rete**. 

> **Nota**: cpu e sistema operativo implementano solo da collegamento in su.

> [!note] framing
> incapsulare datagrammi aggiungendo **instazione** e **trailer**

> [!note] accesso al collegamento
> Bisogna definire come si accede al mezzo di trasmissivo se condiviso: non si puo' parlare contemporaneamente.

> [!note] $EDC$
> Bisogna concordare $f(D)=EDC$ dove $D$ sono i bit di dati e $EDC$ e' un valore in funzione di $D$, calcolato da $f$, questa funzione e' concordata tra mittente e ricevente.

Quando si sceglie $f$ bisogna considerare che:
* *e' poco probabile ricevere bit errati*. in media ne ricevo molti giusti.
* e' piu probabile che i bit errati arrivino in **burst**
* $f$ potrebbe non rilevare alcuni pattern, dunque **errori non rilevati**.

> assumiamo che la probabilità di avere un errore e' $<< 1$.
# Controllo Parita
> [!note] singolo bit di parita
> Dati $d$ bit, voglio spedire $p$ che e' impostato ad 1 se e solo se, server per pareggiare il numero di 1 in $d$.
> *Rileva un numero dispari di errori*.

> **Nota**: posso fare la versione bidimensionale. Distendo i bit a matrice e calcolo per ogni riga e colonna i bit di parità.

> **Nota**: entrambi i metodi non rivelano alcuni pattern di errori.

Il controllo di parita va bene se:
* probabilita di errori bassa
* gli errori sono indipendenti l'uno dall'altro
* non si verificano burst di errori, altrimenti la probabilita di non rilevare l'errore arriva al $50\%$.
# Checksum
> Come in UDP
# CRC
* $D: \text{ Dati da mandare}$
* $G:$ Generatore di $r+1$, **concordato**
* $R: \text{r bit tali che } <D,R> \text{ divisibile per } G \text { mod 2 }$.
* $<D,R>=D \cdot 2^{r} \text{ XOR } R$.

> **Nota**: il mittente calcola $r$ bit CRC, ossia $R$. Il ricevente conosce $G$ e facendo la divisione con $<D,R>$ se il resto e' diverso da 0 ho errore.

> **Nota**: il metodo individua correttamente burst di $r$ bit.
> Posso scegliere $G$ in modo che non divida polinomi di errore, miglioro dunque le performance.

> **Nota**: l'aritmetica usata e' in **modulo 2 senza riporti**, dunque addizione e sottrazione hanno lo stesso risultato.

> **Nota**: l'aritmetica usata equivale a vedere i bit come **coefficienti di polinomi**.

Dunque:
1. mittente invia $T = D \cdot 2^{r} \text{ XOR } R$
2. il ricevente riceve $T' = T \text{ XOR } E$ ossia $T + E$ usando addizione CRC.
3. Essendo $T$ divisibile per $G$, allora anche $T'$ lo e' se e solo se $E$ divisibile per $G$.

> [!note] perche error detection su trasporto e collegamento?
> Il livello di collegamento puo' implementa hardware dedicato per il rilevamento degli errori: calcoli piu complessi ma anche piu veloci!


# Collegamenti
Ci sono due tipi di collegamenti:**punto-punto** e **broad-cast**.

> [!note] broadcast
> un canale diviso tra piu nodi trasmittenti e riceventi, dove *un frame trasmesso e' ricevuto da tutti*. Usato in wLAN, 4g, 5g.

> [!note] protocollo di accesso multiplo
> vorremmo che fosse distribuito tra i nodi, in modo che determinino da soli come e quando trasmettere.
> Non si usa un canale fuori banda per il coordinamento!

Idealmente in un canale broad-cast vogliamo:
* **usare tutta la banda se ho un solo host**
* $1/N$ della banda se $N$ host comunicano contemporaneamente
* **equo**, semplice da implementare, **senza arbitri**, **decentralizzato**, *senza usare canali di comunicazione dedicati*.

Bisogna implementare dei protocolli per la condivisione del canale di comunicazione:
* **Channel Division**: TDMA, FDMA. Divido il canale in modo da non avere collisioni.
* Random Access: canale non diviso, potrebbero avvenire collisioni!
* Taking Turns

# Protocolli Random
## Slotted ALOHA
assunzioni:
* tutti i frame hanno la **stessa dimensione**
* il tempo e ripartito in **slot temporali uguali**
* si inizia a trasmettere a inizio slot
* tutti sincronizzati.
* tutti i nodi rilevano la collisione prima del termine dello slot.

L'accesso e' randomico: si inizia a trasmettere ad inizio slot.

> [!note] operazioni
> * Si comincia a trasmettere solo all'inizio dello slot
> * se ho collisione, ritento al prossimo slot con probabilita $p$ finche non ho successo.

> **Nota**: un solo nodo trasmette continuamente alla massima velocità ed e' altamente **decentralizzato**.

> **Nota**: *grande spreco di slot* in caso di collisioni e i nodi devono essere **sincronizzati**.

> **Nota**: applicando la distribuzione binomiale si nota che l'efficienza massima del sistema al crescere dei nodi e' del $37\%$


> [!warning] non c'e' collision detection
> I nodi si accorgono della collisione quando non ricevono **ACK** a livello di collegamento. il protocollo prevede un meccanismo di `ACK`

> [!note] ALOHA puro
Se si usa ALOHA puro (*non uso ripartizioni in slot*) l'efficienza e' solo del $18\%$.

> **Nota**: ALOHA non si accorge immediatamente della collisione, in un secondo momento ritentera a inviare il frame con probabilita $p$.

## CSMA 
Al contrario di ALOHA, facciamo il contrario dei **fascisti di merda**: prima di parlare si ascolta, senza interrompere con violenza.

> [!note] CSMA
> Se il canale e' **inattivo** $\to$ trasmetto l'intero frame, altrimenti **differisco** la trasmissione.

> [!note] CSMA/CD
> Quando rilevo collisione durante una trasmissione, smetto di trasmettere! Le collisioni sono facili da rilevare via cavo.

> **Nota**: guardando la porta di ingresso posso verificare se c'e' una collisione. Ma devo tenere conto del **ritardo**!

> **Nota**: Quando un nodo rileva una collisione manda un segnale jam, di disurbo.

![[Pasted image 20250915144708.png]]

> [!note] CSMA/CD: binary exponential backoff
> Dopo la $m$-esima collisione scegli $K \in \{0,1,2,...,2^{m}-1\}$ e aspetta il tempo di trasmissione di $K 512bit$ per ritentare.

> **Nota**: $m$ limitato a 10.

> [!note] $\tau_{max}$
> il tempo di trasmissione deve essere almeno $2\tau$ con $\tau = \tau_{max}$ ossia il ritardo massimo di propagazione nel mezzo che include un **margine di sicurezza**

> **Nota**: all'aumentare della velocita di trasmissione, diminuisce la distanza fisica massima.

> **Nota**: bisogna tenere conto anche la durata massima del segnale di `jam`.

L'efficienza in $CSMA/CD$ e':
$$
\frac{1}{1+5d_{prop}/d_{trasm}}
$$
L'efficienza tende a 1 se:
* $d_{prop}$ tende a 0: se il tempo di trasmissione e' basso.
* $d_{trasm}$ tende a infinito: ossia se un frame impegna il canale a lungo tempo.

> **Nota**: CSMA/CD ha un'alto overhead se ho molti utenti che trasmettono contemporaneamente.

# Protocolli a Rotazione

> [!note] polling
> Un'arbitro centrale interpella gli host che possono mandare massimo $k$ frame in rete.

Il polling ha dei problemi:
* **overhead e delay** per scambio di messaggi
* **centralizzato**, dunque single point of failure. se schiatta l'arbitro nessuno parla.

> **Nota**: l'arbitro deduce quando un client finisce di trasmettere dall'assenza di segnale

> **Nota**: no collisioni e slot inutilizzati

> [!note] Token
> Sistema carino, i dispositivi si passano a turno un **token**, che viene trattenuto per un certo tempo massimo.
> Se ho il token, trasmetto.
> Il token viene passato a cerchio! (`token-ring`)

Problemi del `token`:
* latenza che dipende dagli host
* il token potrebbe essere perso
# LAN 
> [!note] LAN
> Una Local Area Network e' una rete geograficamente limitata (una casa, un'ospedale, una scuola) fatta di dispositivi collegati tramite **Ethernet** o **WiFi** di solito.

> [!note] MAC address
> Indirizzo usato localmente per potrare i frame da un'interfaccia all'altra. La connessione e' fisicamente adiacente. E' a $48bit$.

> **Nota**: l'ip e' gerarchico, il MAC no, e' sempre quello ma puo' essere modificato.

> **Nota**: Il MAC e' assegnato dalle aziende che comprano range di indirizzi MAC.

> [!note] Tabella ARP (Addr Resolution Protocol)
> Associazione tra `IP` e indirizzo `MAC` con un `TTL`, ma come la popolo?

> [!warning] `MAC` e `IP`
> rispetto a `IP`, il `MAC` scala meglio su LAN enormi. Processare l'indirizzo `MAC` e' semplice, non c'e' bisogno di dover leggere l'intestazione `IP` e l'hardware e' ottimizzato per processare proprio gli indirizzi `MAC`.
> Quando processo `IP` c'e' di mezzo `NAT`, `firewall` ecc... molto **overhead**!

Se so di essere nella stessa sottorete dell'`IP` di destinazione:
1. mando una richiesta `ARP` con `MAC=FF-FF-FF-FF-FF-FF` specificando l'`IP` da risolvere.Ma come faccio a conoscere il'MAC di destinazione a partire dall'IP? Se so di essere nella stessa sottorete dell'IP posso usare ARP (Address Resolution Protocol): interpello in broadcast tutti i dispositivi nella LAN (tramite messaggi ARP) per ottenere i loro MAC address. Successivamente, quando riscontro ip e mac address posso tenerli in memoria nella tabella ARP fatta cosi: ￼￼MAC | IP | TTL￼￼.C'e' sempre un TTL: il dispositivo puo' andaresene a fanculo.
2. L'host in questione risponde mandando il suo `MAC`
3. Il `MAC` viene mantenuto in memoria nella tabella

> [!note] `arp spoofing` e `arp poisoning`
> Posso immettere richieste `arp` fittizie in modo da generare un `DoS` (associo tanti `ip` allo stesso `mac`) oppure posso fare un `mitm` intercettando i pacchetti con un `mac` fittizio, di proprieta' dell'attaccante.


Se devo uscire dalla LAN?
1. Ipotizziamo che $A$ conosce ip di $B$ e dell'interfaccia di $R$ nella sua sottorete e il suo `MAC`.
2. $A$ crea datagramma `IP` verso $B$
3. $A$ crea frame che incapsula $B$ e con destinazione MAC di $R$.
4. $R$ riceve il frame, capisce che deve inoltrarlo verso un'altra sottorete e che deve mettere come `MAC` di destinazione quello di $B$.
5. $B$ riceve il frame!

> **Nota**: $A$ conosce l'indirizzo`IP` di $R$ grazie a `DHCP` il `MAC` di $R$ con `ARP`.
# Ethernet 
E' una delle prime tecnologie inventate, efficiente ed economica per il trasferimento via cavo, si e' evoluta, ha *molti standard* con *diverse velocita* di trasferimento e diversi metodi di connessione:
* **Bus**: ethernet era un cavo coassiale condiviso
* **Hub**: i dispositivi erano connessi a stella verso un unico HUB che fungeva da ripetixtore. Fa sempre `flooding`.
* **Switched**:.

> [!note] switch
> Si occupa di ridirezionare il traffico facendo autoapprendimento.

Un **pacchetto Ethernet** e' fatto di:
* **preambolo**: una sequenza di 7 byte 10101010 seguiti da 10101011.
* **payload**: di massimo 1500 byte e almeno 46 byte. *altrimenti deve essere aggiunto del padding*
* MAC sorgente e destinazione
* **CRC**: non corregge, ma scarta!

> **Nota**: il preambolo serve a sincronizzare e "svegliare" chi ascolta.

> **Nota**: se riceve un frame con `MAC` corrispondente al mio, allora lo passo al livello superiore.

> **Nota**: la fine della trasmissione e' dedotta dalla mancanza di segnale.

> [!note] affidabilita di ethernet
> **Senza connessione**: nessun handshake tra le `NIC`
> **Non affidabile**: nessun `ACK` e `NAK`
> Usa: `CSMA/CD` con exponential backoff.

> **Nota**: ethernet ha standard con velocita e limiti di lunghezza differenti. Il cavo cat 5 ha un limite di 100 metri

# switch ethernet
>[!note] switch
> Ha un ruolo **attivo**, fa `store-and-forward` del frame, esamina l'indirizzo MAC e inoltra **selettivamente** verso uno o piu collegamenti in uscita usando `CSMA/CD` in caso di ethernet.
> E' **trasparente**: gli host sono inconsapevoli della sua presenza.

> **Nota**: lo switch e' `plug-and-play`, ossia lo accendi e popola da solo la **tabella di commutazione** imparando da quali pacchetti provengono da ogni porta.

> **Nota**: se ho collegamenti **ciclici** tra switch, il flooding non si ferma piu.

La connessione tra due nodi attraverso uno switch puo' essere:
* **full-duplex**: la connessione tra due nodi che comunicano e' dedicata
* **half-duplex**: ogni estremita dello switch e' un dominio di collisione a se stante, si usa se ci sono piu nodi che comunicano con lo stesso host.

*Cosa succede se non so dove mandare un frame perche' non ho ancora la sua associazione*? Faccio flooding senza cambiare MAC di destinazione.

> **Nota**: la tabella di commutazione ha un `TTL`

> **Nota**: il meccanismo di flooding, funziona anche con piu' switch interconnessi.

> [!note] Switch e router
> Entrambi fanno store and forward ma:
> * switch opera sul livello di collegamento, router su rete.
> * switch fa autoapprendimento, router calcola tabelle di inoltro.
> * il router opera con l'obiettivo di trovare percorsi ottimali e senza cicli, scarta i pacchetti se `TTL=0`
> * gli switch sono connessi a **spanning tree** per evitare che un frame rimanga in circolazione per sempre.

> **Nota**: negli switch c'e' un'enorme traffico `ARP`, mentre i router ragionano per instradamento gerarchico e aggregazione degli indirizzi.

# VLAN
Man mano che la LAN cresce vogliamo mantenere dei sottogruppi all'interno di questa per:
* **scalare** meglio il traffico `arp`
* ridurre **problemi di efficienza**, sicurezza e privacy.
* gestire lo **spostamento fisico** di un utente da switch all'altro, e mantenere una connessione logica sullo switch originale.

> [!note] VLAN
> Ogni switch ha un'insieme di **porte raggruppate**, ogni gruppo costituisce una **LAN virtuale** che opera sulla struttura fisica della **LAN**

> [!note] porta trunk
> trasporta frame tra `VLANK` differenti


