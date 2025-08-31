Questo livello offre:
* framing dei datagrammi
* Error detection: ed eventuale meccanismo di recupero dagli errori
* trasmissione del frame sul mezzo di trasporto

Il livello di collegamento e' implementato nelle interfacce di rete: l'adattatore implementa livello fisico e di collegamento, mentre la CPU e il Sistema Operativo devono implementare solo da livello di collegamento in su

Per la Error Detection ci sono varie tecniche: bisogna concordare una $f(D)=EDC$. ossia una funzione concordata tra mittente e ricevente che calcola $EDC$. Il ricevente dovra' solo controllare che $f(D')=EDC'$ (nota: potrei non accorgermi dell'errore alcune volte anche se verifico l'errore).

Quando si sceglie $f$ bisogna considerare che:
* e' poco probabile ricevere bit errati. in media ne ricevo molti giusti.
* e' piu probabile che i bit errati arrivino in burst
* $f$ potrebbe non rilevare alcuni pattern
# Controllo Parita
Se voglio spedire $d$ dati, allora calcolo $p$ bit di parita. Ossia $p$ e' impostato a uno se serve a pareggiare il numero di bit accesi in $d$. 

Ovviamente questo metodo non riesce a correggere errori e non li rileva tutti (es: dopo l'errore ho sempre pari/dispari bit accesi), per questo si preferisce fare un controllo di parita bidimensionale stendendo i $d$ dati su una griglia. Per ogni riga e colonna calcolo la parita: riesco quindi questa volta a correggere un buon numero di errori, ma non riesco a rilevarli tutti.

Comunque funziona bene perche' e' improbabile avere un numero pari di errori, e se gli errori hanno probabilita bassa di avvenire.
Questo metodo ha poco overhead quindi ci sta.

# Checksum
come visto per UDP, si vede il frame come una sequenza di numeri a 16 in complemento a 2 e viene calcolata la checksum come il numero opposto alla somma.

Il ricevente per controllare l'integrita del pacchetto deve sommare la sequenza di numeri ricevuta e controllare se la somma e' `1.111` ossia 0 in complemento a 2.

# CRC
* $D: \text{ Dati da mandare}$
* $G:$ Generatore di $r+1$ 
* $R: \text{r bit tali che } <D,R> \text{ divisibile per } G \text { mod 2 }$.
Questo vuol dire che il ricevente puo' controllare la divisione $<D,R>/G$ e vedere se il resto e' 0 per determinare l'errore.

Inoltre il metodo individua correttamente burst di $r$ bit di errore, e inoltre, scegliendo $G$ in modo che non divida i polinomi di errore e' possibile migliorare le performance.

Si parla di polinomi di errore perche' si usa l'aritmetica modulo 2 senza riporto, quindi addizione e sottrazione sono uguali allo `XOR` logico e ogni bit rappresenta il coefficiente di un incognita di un polinomio.

# Collegamenti
Ci sono due tipi di collegamenti:**punto-punto** e **broad-cast**. I collegamenti broad-cast sono soggetti a frequenti collisioni: se piu  host parlano insieme il segnale e' sporco!

Idealmente in un canale broad-cast vogliamo:
* usare tutta la banda se ho un solo host
* $1/N$ della banda se $N$ host comunicano contemporaneamente
* equo, semplice da implementare, senza arbitri, decentralizzato, senza usare canali di comunicazione dedicati.

Bisogna implementare dei protocolli per la condivisione del canale di comunicazione:
* Channel Division: TDMA, FDMA
* Random
* Turno

# Protocolli Random
## ALOHA
Questo sistema prevede la divisione del canale temporalmente. Si divide l'accesso in slot temporali, ognuno di questo e' ripartito tra gli host.

Si comincia a trasmettere solo all'inizio dello slot (quindi tutti devono sincronizzarsi), e se piu' host si accorgono che stanno trasmettendo allora se ne accorgono e ritentano al prossimo slot con probabilita' $p$: quindi casualmente. 
L'efficienza di questo sistema per $p$ uguale per tutti, e' $37\%$. Bast applicare la distribuzione binomiale, e si vede che all'aumentare del numero di host l'efficienza rispetto al tempo di utilizzo e' solo del $37\%$ ossia $1/e$.

Se si usa ALOHA puro invece l'efficienza e' solo del $18\%$, infatti senza slot aumenta la probabilita che due o piu host possano collidere.

**Importante**: ALOHA non si accorge immediatamente della collisione, in un secondo momento ritentera a inviare il frame con probabilita $p$

## CSMA 
Al contrario di ALOHA, facciamo il contrario dei fascisti di merda: prima di parlare si ascolta, senza interrompere con violenza.
Guardando la porta di ingresso posso verificare se c'e' una collisione ma devo tenere conto del ritardo:
* potrei iniziare a trasmettere prima che mi arrivi di passaggio il frame che qualcuno ha iniziato a trasmettere prima di me!

Cosa succede in questo caso? Mando un segnale di Jam per far capire all'altro che il segnale ormai e' disturbato.

Per far si che questo sistema sia corretto, ogni host deve trasmettere per almeno $2 \tau_{max}$: devo fare in tempo a ricevere il segnale dell'altro e devo propagare il segnale di jam.

Successivamente ritento aspettando il tempo per trasmettere $k 512 bits$ con $k \in {1,2,...,2^m}$ (k randomico) dove $m$ e' il numero di tentativi falliti di collisioni. Questa tecnica e' chiamata Binary Exponential Backoff, e di solito $m$ e' limitato a 10 (senno attendo troppo, esponenzialmente).

# Protocolli a Rotazione
## Turno (Polling)
Esiste un'arbitro centrale che interpella gli host, questi per risposta possono inviare un massimo di $k$ frame in rete.
Il problema di quest'approccio e' che:
* **overhead** e **delay** per lo scambio di messagi
* centralizzato, come conseguenza esiste un single point of failure. Se schiatta l'arbitro poi basta.
## Token
Sistema molto carino: i dispositivi si passano a turno un token, che puo' essere trattenuto per un tempo massimo. Chi possiede il token puo' inviare frame in rete.
Problemi: overhead per lo scambio di messaggi, alta latenza (latenza che dipende dai dispositivi tra l'altro), il token puo' essere perso (single point of failure)

# LAN 
Una Local Area Network e' una rete geograficamente limitata (una casa, un'ospedale, una scuola) fatta di dispositivi collegati tramite Ethernet o WiFi di solito.

I dispositivi in una LAN si indirizzano per mezzo del MAC Address, univocamente determinato, non gerarchico ed associato all'interfaccia di rete: quindi l'interfaccia cambia LAN, l'ip cambia ma non il MAC (anche se questo puo' essere modificato).

Il MAC e' assegnato dalle aziende che comprano range di indirizzi MAC.

Il MAC e' a 48 bit.

Ma come faccio a conoscere l'MAC di destinazione a partire dall'IP? Se so di essere nella stessa sottorete dell'IP posso usare ARP (Address Resolution Protocol): interpello in broadcast tutti i dispositivi nella LAN (tramite messaggi ARP) per ottenere i loro MAC address. Successivamente, quando riscontro ip e mac address posso tenerli in memoria nella tabella ARP fatta cosi: `MAC | IP | TTL`.C'e' sempre un TTL: il dispositivo puo' andaresene a fanculo.

Che succede se invece devo uscire dalla LAN? Es: un frame da A deve andare a B. Innanzitutto A si accorge guardando IP che sono in sottoreti diverse, quindi s a che deve inririzziare il pacchetto A verso il router di bordo $R$ che fa da tramite tra $A$ e $B$. Quindi $A$ manda un frame con destinazione $R$, mentre il datagramma IP contenuto all'interno ha come destinazione $B$. Successivamente $B$ riceve il frame e' fara lo stesso giochetto di $A$ per far arrivare il datagramma a destinazione.

# Ethernet 
E' una delle prime tecnologie inventate, si e' evuluta, ha molti standard con diverse velocita di trasferimento e diversi metodi di connessione:
* Bus: ethernet era un cavo coassiale condiviso
* Hub: i dispositivi erano connessi a stella verso un unico HUB che fungeva da ripetitore
* **Switched**: oggi uno switch di rete si occupa di ridirezionare correttamente i dati verso la giusta porta facendo autoapprendimento.
Un pacchetto Ethernet e' fatto di:
* **preambolo**: una sequenza di 7 byte 10101010 seguiti da 10101011. Serve a sincronizzare gli host in ascolto
* payload: di massimo 1500 byte e almeno 46 byte. altrimenti deve essere aggiunto del padding
* MAC sorgente e destinazione
* CRC: non corregge, ma scarta!

Lo switch di rete e' un dispositivo attivo ma trasparente all'host: L'host non sa che lo switch manipola i suoi frame, perche' in modo trasparente lo switch cerca di capire verso quale delle sue uscite ridirezionare il pacchetto.

Lo switch di rete per imparare dove inviare i pacchetti ha una specie di tabella ARP, fatta di `MAC addr | interfacia | TTL`. La tabella viene popolata man mano che lo switch riceve frame in input (basta analizzare il frame per capire a che mac e' associata quella interfaccia), se pero' ricevuto il frame non so a chi mandarlo devo fare **flooding**: lo mando in broadcast a tutti quanti.

E' importante nella rete di switch non avere collegamenti ciclici: altrimenti il flooding non si ferma piu.

# VLAN
Potrebbe essere nosso interesse, man mano che la complessita della LAN cresce, mantenere una sorta di connessione logica verso un sottogruppo della LAN. Per esempio potrei avere un'organizzazione in una scuola divisa tra Studenti e Professori e vorrei separarli. Ma soprattutto Uno studente potrebbe voler connettersi da un punto di accesso riservato ad un professore. 

Per risolvere questo problema, e' possibile assegnare ogni porta di uno switch  ad un gruppo virtuale e di connetterli in modo da isolare il traffico.

Ma cosa facciamo se ho due switch di rete entrambi divisi in porte per Studenti e Professori? Uso una porta Trunk per collegare i due switch e permettere lo scambio di informazioni


