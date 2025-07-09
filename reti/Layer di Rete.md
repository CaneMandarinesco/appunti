
## 🧭 **Layer di Rete**

- Fornisce una **connessione logica end-to-end** tra host.
    
- Si compone di:
    
    - **Piano di controllo**: calcola le tabelle di inoltro (routing table).
        
    - **Piano dei dati**: inoltra i pacchetti secondo tali tabelle.
        

---

## 🧠 **Piano di Controllo**

- Calcola le tabelle di routing in base alle **policy** (aziendali o tecniche).
    
- Due approcci:
    
    - **Tradizionale**: ogni router calcola la propria tabella.
        
    - **SDN** (Software Defined Networking): un **controller centrale** calcola e impone le tabelle ai router.
        

---

## 🚚 **Piano dei Dati**

- Implementa l’inoltro: decide su **quale uscita** inviare ogni pacchetto in base all’IP di destinazione.
    
- Il protocollo IP è **best-effort**:
    
    - Nessuna garanzia di consegna né ordine.
        
    - Nessun controllo su ritardi.
        
- La qualità di servizio (QoS) di IP è limitata.
    

---

## 🧱 **Architettura di un Router**

- **Componenti principali**:
    
    - Porte di ingresso/uscita
        
    - **Struttura di commutazione** (veloce, ns)
        
    - **Processore di controllo** (lento, ms)
        
- **Porta di ingresso**:
    
    - Terminale (livello collegamento)
        
    - Unità intestazione (livello rete)
        
    - Buffer
        

---

## 🔀 **Tecniche di Commutazione**

1. **Su memoria**: semplice ma lenta.
    
2. **Su bus**: più efficiente ma con accesso condiviso.
    
3. **Su rete (crossbar)**: alta concorrenza, usata per alte prestazioni.
    

- Tecniche di lookup:
    
    - **Prefix matching**: si cerca la **longest prefix match**.
        
    - **TCAM**: memoria parallela a priorità.
        
- **Problema**: accodamento in testa (head-of-line blocking).
    
    - Serve **policy di scheduling e drop**:
        
        - Drop su buffer pieni
            
        - **Round Robin** e **Weighted Fair Queuing (WFQ)** per equità
            

---

## 🧾 **Datagramma IP e MTU**

- L’**intestazione IP** (20+ byte) include:
    
    - IP sorgente/destinazione, ID pacchetto, offset, flag DF, checksum, lunghezza totale.
        
- **MTU**: se il pacchetto IP è più grande della MTU, avviene **frammentazione**.
    
- **MTU Path Discovery**: si testano pacchetti con DF=1 fino a che non si riceve un ICMP "too big".
    

---

## 🌐 **Indirizzamento IP**

- Indirizzo IP = parte di **rete** + parte di **host** (CIDR notation, es. `/16`)
    
- Prima di CIDR: classi A, B, C (ora obsolete)
    

---

## 📦 **DHCP**

- Protocollo per assegnare **dinamicamente** un indirizzo IP.
    
- Scambio:
    
    1. DHCP Discover (client → broadcast)
        
    2. DHCP Offer (server)
        
    3. DHCP Request (client)
        
    4. DHCP ACK (server)
        

---

## 🧩 **NAT (Network Address Translation)**

- Mappa indirizzi privati a un solo indirizzo pubblico.
    
- Il router mantiene una **NAT table** con IP/porta sorgente ↔ IP/porta pubblica.
    
- Compromette l’architettura end-to-end, ma risolve l’esaurimento di IPv4.
---
## 🔢 **IPv6**
- Indirizzi a **128 bit**
- Header **fisso**, niente frammentazione, niente checksum → più veloce.
- Aggiunge:
    - **Flow label** per QoS
    - Campo **Hop limit** al posto del TTL
- **Tunneling** usato per attraversare link IPv4-only.

---

## 🎛️ **OpenFlow e SDN**

- **SDN** separa il piano di controllo (controller centrale) dal piano dei dati (switch).
    
- **OpenFlow**:
    
    - Protocollo per comunicazione controller ↔ switch.
        
    - Implementa **match-action**: se pacchetto matcha una regola → esegui azione.
        
    - Azioni: forward, drop, modify, send to controller.
        
    - Tipi di messaggi: controller-to-switch, async, symmetric.
        

---

## 🌍 **Routing Scalabile**

- Internet è diviso in **Autonomous Systems (AS)**, ciascuno con un **ASN**.
    
- Due tipi di routing:
    
    - **Intra-AS**: interno all’AS → ottimizzato per efficienza
        
    - **Inter-AS**: tra AS → rispetta policy commerciali
        

---

### Intra-AS:

- **RIP**: distance vector, semplice.
    
- **OSPF**: link-state, usa flooding e supporta ECMP per bilanciamento.
    
- Architettura gerarchica (backbone + aree locali).
    

---

### Inter-AS:

- **BGP** (Border Gateway Protocol):
    
    - **eBGP** tra AS diversi, **iBGP** all’interno.
        
    - Rotte descritte da:
        
        - `AS_PATH`: sequenza AS
            
        - `NEXT_HOP`: primo router AS successivo
            
    - Le policy decidono quali rotte **accettare** e **pubblicizzare**.
        
- **Criteri di selezione rotta BGP** (semplificato):
    
    1. Local preference
        
    2. AS path più corto
        
    3. Origin type
        
    4. MED
        
    5. eBGP preferito su iBGP
        
    6. ID router (tie-break finale)
        

---

### 🧠 Bonus concetto:

- Intra-AS → **efficienza**
    
- Inter-AS → **policy e accordi commerciali**
    
---
![[layer_di_rete.excalidraw]]
Il Layer di Rete offre connessione logica diretta tra due end-point. Questa connessione avviene grazie al piano dei dati e al piano di controllo, insieme si occupano di inoltrare correttamente i datagrammi a destinazione.

Il piano di controllo si occupa di calcolare le tabelle di inoltre che il piano dei dati deve seguire. Queste tabelle vengono calcolate con degli algoritmo basate sulle policy dell'azienda che gestisce i router. Ogni router potrebbe calcolare autonomamente la sua routing table oppure si usa la tecnica **SDN**, ossia un controllore remoto che installa nei router le tabelle di inoltro da questo calcolate.

Il piano dei dati attua il piano di controllo: per ogni pacchetto in arrivo il router deve determinare verso quale uscita reindirizzare il pacchetto.

Il protocollo IP e' un servizio best effort:
* non garantisce che i datagrammi siano consegnati
* non garantisce che i datagrammi arrivino in ordine
* il suo comportamento non e' influenzato da ritardi come TCP.

Quindi la QoS di IP e' scarsa, eppure alternative ad IP con QoS piu' alta non sono state adottate nel tempo! (citazione a Boris: a noi piace la merda!). 

## architettura di un router
Un router e' composto da:
* porte in ingresso
* porte in uscita
* **struttura di commutazione** (attua piano dei dati, agisce nell'ordine dei nanosecondi)
* **processore** (attua piano di controllo, agisce nell'ordine dei millisecondi)

Una porta di ingresso invece e' costituita da:
* terminale in ingresso (implementa livello di collegamento)
* unita di elaborazione intestazione (implementa livello di rete)
* **buffer**

La commutazione dei pacchetti puo essere:
* **generalizzata**: analizzo vari campi per decidere dove va un pacchetto
* **destinazione**: guardo l'ip di destinazione.

L'inoltro su destinazione viene implementato guardando in una tabella, ad ogni riga e' specificato un range di indrizzi (o un indirizzo prefissato), e si cerca il range che piu' lungo che combacia con l'ip di destinazione. A questo prefisso e' associata una porta di uscita calcolato dalla SDN.

Oppure si usa una **TCAM**: una memoria che grazie a dei comparatori che lavorano in parallelo e ad un priority encoder riesce a prelevare in poco tempo la porta di destinazione.

La commutazione avviene in 3 modi:
* **Sulla Memoria**: il processore scrive in memoria i dati dalla porta di ingresso e poi li scrive sulla porta in uscita. Semplice ma lento, usa la classi architettura di un computer, devo solo indirizzare aree di memoria.
* Commutazione di Bus: un bus comune che viene chiuso e dedicato per inviare dati verso un'unica porta di uscita.
* Commutazione di Rete: piu' compliato ma permette di inviare contemporaneamente piu' dati insieme. Si implementa grazie a crossbar switch nxm oppure organizzati come una rete, in modo da favorire il parallelismo e diminuire il numero di switch usati. Spesso si usano piu reti di switch in parallelo

Puo' avvenire una cosa brutta: l'accodamento in testa (in input). Ossia arrivano piu' dati di quanti ne riesco a processare, per questo bisogna definire delle policy di  Drop e di Scheduling dei pacchetti: quali sono i piu' importanti da inviare prima? E sporattutto quanto deve essere grande il buffer?

1. Se il buffer e' troppo grande allora aumenta RTT, e quindi sarebbe meglio scartarlo piuttosto che aspettare troppo tempo.
2. Posso classificare i pacchetti per fare drop e scheduling basato su priorita.
3. Round Robin: In Round robin sulle classi, seleziono in base alla priorita quale pacchetto inviare.
4. Weighted Fair Queing: $\frac{w_{i}}{w_{1}+...+w_{n}}$, con questa formula voglio sapere qual'e' la classe che in rapporto al carico totale ha piu lavoro da svolgere. Eseguo quindi in Round Robin sulle classi ma con questo approccio.

## datagramma IP
Il protocollo di rete opera su due piani differenti e il routing viene effettuato guardando i dati del datagramma IP. Questo e' strutturato in sequenze di 32 bit. Almeno i primi 20 byte sono di intestazione, questa contiene:
* ip sorgente
* ip destinazione
* id del pacchetto: 16 bit
* bit don't frag
* bit offset
* checksum instestazione
* **lunghezza totale del datagramma**
* sezione di lunghezza variabile

## MTU
la MTU e' la quantita di dati che un collegamento puo' trasportare, se il datagramma ip e' troppo grande allora deve essere frammentato!
La frammentazione avviene guardando i campi: `DF, ID, OFFSET`.

Se si vuole evitare il Fragment dei dati imposto `DF` e devo assicurarmi che il datagramma non sia piu grande della MTU.

**MTU Path Discovery**: mando dei paccheti con DF=1 per sondare la MTU finche' non ricevo un pacchetto ICMP dal router. Il problema e' che i pacchetti non sempre seguono lo stesso percorso! Inoltre potrei perdere pacchetto ICMP.

# Indirizzo IP
L'indirizzo IP e' gerarchico ossia, e costituito da una parte che identifica la sottorete a cui questo appartiene, ed una parte che identifica l'host. Si usa la notazione CIDR per specificare la parte di rete, es `18.255.1.0\16` vuol dire che i primi `16` bit identificano la sottorete, e i restanti l'host nella sottorete.

Prima della notazione CIDR gli IP erano staticamente divisi in sottoreti:
* `A`: `255.*.*.*` ossia `\8`
* `B`: `255.255.*.*` ossia `\16`
* `C`: `255.255.255.*` ossia `\24`
* `D`: non ha una maschera di rete 
* `E`: riservato
## DHCP
E' un protocollo che permette di ottenere da un server installato su un router, un indirizzo IP valido per conettersi alla sottorete e iniziare a dialogare. E' utile per ottenere una configurazione dinamica (magari non mi serve specificare un indirizzo) dell'IP che e' utile per dispositivi mobili. Proprio perche' i dispositivi possono scollegarsi dalla rete gli IP assegnati hanno un lifetime.

Si presuppone che ci sia un server  DHCP in ascolto in rete. Di solito succede questo:
* `client: DHCP DISCOVER`, messaggio mandato in broadcast, accompagnato con un `ID`.
* `server: DHCP OFFER`, dopo aver rilevato il messagio in broadcast, viene offerto un `IP` valido. Nella risposta ellega `ID` letto.
* `client: DHCP REQUEST`, mando al server l'IP che mi ha fornito. Gli dico che voglio usarlo.
* `SERVER: DHCP ACK`, accetto!

I primi due step sono opzionali, posso specificare al server un indirizzo IP a mia scelta.
Il server puo' fornire altre informazioni come: maschera di sottorete e router first hop.

## Come la Rete ottiene prefisso di rete

Un ISP ottiene un range di indirizzi IP dall'ICANN. successivamente l'ISP puo' suddividere il suo range e ripartirlo in organizzazioni, ognuna di queste pubblicizzera in rete path verso i propri indirizzi.

## NAT
Problema: 32bit non so pochi? Si. Con il NAT si aggira il problema.
* i dispositivi all'interno della stessa sottorete hanno indirizzi ip differenti
* gli stessi dispositivi visti fuori dalla sottorete, sono tutti indirizzabili usando un unico IP.

Come funziona? NAT Table: il router di accesso deve implementare questa tabella dove ad ogni coppia di `ip sorgente/porta sorgente` corrisponde `ip destinazione/porta destinazione`, dove l'ip di destinazione e' relativo alla sottorete, di conseguenza il router per ogni pacchetto deve potenzialmente dover cambiare i campi di porta e ip fino al livello di trasporto.

Controverso: il router deve implementare ora il livello di trasporto per modificarlo, e soprattutto e' un intermediario in una connessione che dovrebbe essere end-to-end, pero' funziona alla grande.

## IPv6
Soluzione seria ai pochi IP, IPv6. A differenza di IPv4 ha un intestazione fissa, invece del campo TTL abbiamo il campo HOP. I campi ip src e ip dest sono a 128 bit. La frammentazione e' stata abbandonata, non e' possibile frammentare, e la checksum uguale in modo da ridurre il tempo di latenza dovuto a controlli e frammentazione.

Un'altra cosa figa che fa e' dare un etichetta al pacchetto, detto etichetta di flusso e dare un codice di priorita a quest'ultimo.

IPv6 tutta via oggi non e' utilizzato ancora da tutti i dispositivi, quindi cosa succede quando un pacchetto IPV6 deve attraversare un collegamento che supporta solo ipv4? Si usa il **tunneling**: agli estremi del collegamento ci devono essere dei router a doppia tecnologia che prendano IPv6 e lo mettono come payload dentro a IPv4. Questo pacchetto viene spedito verso il primo router a doppia teconlogia disponibile cosi che questo possa estrarre dal payload il pacchetto ipv6.
## Openflow
E' un'API per implementare il meccanismo match-action. Questo meccanismo e' un'astrazione di moltissimi dispositivi MiddleBox che si trovano in rete come: switch e firewall. 

E' possibile definire delle regole di tipo match-action, di immagazzinarle dentro ad una tabella dei flussi. Ad ogni pacchetto in arrivo vengono verificate le condizioni di match e quella con priorita piu alta fara' scattare la action associata.

Le azioni possibili sono 4: inoltro, drop, control e modify.
I tipi di matching che possiamo controllare sono limitati in modo da rendere lo standard semplice da usare, banalmente e' possibile controllare IP destinazione e sorgente, indirizzi MAC, HOP, TTL ecc... per decidere che cazzo fare col pacchetto.
## MiddleBox

# Link State e Distance Vector
# Scalare il routing
Obiettivo del routing: effettuarlo verso miliardi di dispositivi. Quando abbiamo parlato degli algoritmi abbiamo assunto che la rete fosse piatta, ma in realta e' piu comodo avere un'insieme di reti distinte ma connesse. Un **Autonomous System** e' identificato da un ASN, ed e' una collezione di router collegati che comunicano verso l'esterno grazie ad un **router gateway**. 
Ogni AS ha interesse a:
* Collegarsi agli altri AS applicando pero' delle precise regole aziendali
* Effettuare il routing all'interno dei pacchetti
Nell'AS hanno luogo:
* Intra-AS routing: ovvero routing dell'informazione all'interno dell'AS
* Inter-AS routing: ovvero routing dell'informazione verso l'esterno.
Entrambe si ottengono popolando la tabella di inoltro di ogni router (puo' essere la stessa o due distinte). Quando un gateway router ottiene una rotta dall'esterno, e dopo aver verificato se rispetta le policy, la accetta e la propaga all'interno, informando i router di questa rotta verso l'esterno!

I protocolli INTRA-AS piu famosi sono:
* RIP: distance vector
* OSPF: Link state, usa il flooding per propagare l'informazione sulle rotte ed implementa messaggi cifrati per autenticare i messaggi.

In Open Shortest Path First viene applicata la tecnica ECMP: quando ho due path uguali verso la stessa destinazione che faccio? Li uso entrambi per bilanciare il carico sulla rete, e decido dove mandare i pacchetti (next hop) guardando
* destinazione
* flusso
* per singolo pacchetto: svantaggioso per TCP, avrei molti pacchetti consegnati fuori ordine,

OSPF usa un'organizzazione gerarchica a due livelli: 
* dorsale: area che contiene il router di bordo
* aree locali: sono connesse alla dorsale, e non comunicano direttamente tra di loro, l'informazione passa sempre attraverso un router di dorsale.
Questo approccio serve a migliorare le performance dell'instradamento: il flooding e' limitato solo ai router nella stessa area.
I Router Area Borders collegano aree con la dorsale.

Per effettuare Inter-AS routing si usa: BGP (Border Gateway Protocol).
BPG ottiene le rotte dall'esterno e le comunica usando eBGP. eBGP viene solo usato nei router di bordo! Mentre iBGP e' usato per propagare l'informazione ricevuta all'interno dell'AS.

Nella comunicazione due peer BGP si scambiano messaggi con una TCP semipermanente.

Una rotta BGP ha i seguenti attributi:
* `NEXT_HOP`: ip del prossimo router che inizia l'AS PATH
* `AS_PATH`: sequenza di AS attraversati per arrivare a destinazione

L'eBGP se configurato opportunamente puo' scegliere: quali rotte propagare verso l'esterno, e quali rotte propagare verso l'interno, rispettando cosi le Policy.

Ma come sceglo la rotta migliore? Con archi inversamente proporzionali alla larghezza di banda del collegamento:
* Preferenza locale
* ID BGP
* AS-PATH piu breve
* Hot Potato!

Nota: Intra-AS si concentra sull'efficienza, Inter-AS si concentra sulle policy diocane!

### SDN
Come funziona il dialogo tra piano di dati e controllo? Prima si era soliti usare in router monolitico che implementava i vari strati di Internet in modo chiuso, e le interfacce hardware erano chiuse. Oggi invece si utilizza hardware open, interfacce aperte e protocolli aperti come OpenFlow.

Inoltre si predilige la SDN, offero si utilizza un unico controller SDN per implementare il piano di controllo, questo permette una gestione centralizzata degli switch, grazie all'uso di OpenFlow per determinare le tabelle di inoltro.

SDN e' strutturato in tal modo:
* switch di rete implementano il piano dei dati
* controllore SDN: contiene informazioni sulla rete e vari processori, per esempio per il calcolo delle tabelle di routing
* applicazioni SDN: esempio algoritmi di instradamento

Il Protocollo OpenFlow (diverso dalla API OpenFlow) permette di scambiare messaggi tra switch e controllore, ci sono 3 tipi di messaggi:
* **controller-to-switch**: inserire, modificare la tabella di inoltro, notificare dove inviare un determinato pacchetto, aggiornare la tabella, richiedere le feature dello switch
* **asynch**
* **symmetric**

Come esempio: uno switch notifica il controllore che un link ha cambiato stato. Allora dopo che il controllore ha aggiornato le sue strutture dati eseguira l'algoritmo di Dijkstra per calcolare le nuove tabelle di inoltro facendo uso di un controllore apposito. Successivamente il controllore puo' installare remotamente le tabelle calcolate.

