- In CSMA/CD, si trasmette per almeno **l’intervallo di collisione massimo** (2τ), ma il frame non deve necessariamente essere lungo 2τ _in bit_, si **garantisce che la collisione possa ancora essere rilevata entro quel tempo** → puoi chiarirlo meglio.

---

### 2. 🔍 Concetti mancanti o da rafforzare

| Tema                                    | Cosa puoi aggiungere o chiarire                                                                               |
| --------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| **CSMA varianti**                       | CSMA/CA (Collision Avoidance), usata in Wi-Fi, **non rileva** collisioni ma **le evita** usando RTS/CTS       |
| **Controllo di flusso e accesso**       | Livello collegamento può anche gestire **controllo di flusso locale** (es: HDLC, PPP)                         |
| **Protocolli di livello 2**             | Menziona almeno **PPP, HDLC, Ethernet** come esempi concreti di protocolli L2                                 |
| **Gestione degli errori vs correzione** | Parla brevemente del fatto che **il livello 2 di solito rileva, ma non corregge** errori (→ ARQ al livello 4) |
| **Differenza tra Switch e Hub**         | Già accennata, ma potresti evidenziare che gli switch operano in modo **full duplex**, mentre gli hub no      |
| **Spanning Tree Protocol (STP)**        | Hai accennato ai cicli, ma **non hai nominato** il protocollo che li evita                                    |
| **Frame Ethernet**                      | Specifica il significato di **Type/Length** e il campo **FCS (Frame Check Sequence)** nel CRC                 |
| **Livello fisico**                      | Una frase chiarificatrice: “Il livello fisico trasmette bit, quello di collegamento frame”                    |

## ⚠️ Cosa puoi migliorare/aggiungere per avere preparazione da 30 e lode

### 1. ❌ Errori concettuali o tecnici da correggere

- Frasi troppo colloquiali o inadeguate in ambito accademico ("fascisti di merda", "va a fanculo", "schiatta l’arbitro", ecc.) → **non usare all'esame**.
    
- L’efficienza di **ALOHA** è ≈ 18.4%, quella di **Slotted ALOHA** è ≈ 36.8% → questi valori vanno **detti esplicitamente**.
    
- In CSMA/CD, si trasmette per almeno **l’intervallo di collisione massimo** (2τ), ma il frame non deve necessariamente essere lungo 2τ _in bit_, si **garantisce che la collisione possa ancora essere rilevata entro quel tempo** → puoi chiarirlo meglio.
    

---

### 2. 🔍 Concetti mancanti o da rafforzare

|Tema|Cosa puoi aggiungere o chiarire|
|---|---|
|**CSMA varianti**|CSMA/CA (Collision Avoidance), usata in Wi-Fi, **non rileva** collisioni ma **le evita** usando RTS/CTS|
|**Controllo di flusso e accesso**|Livello collegamento può anche gestire **controllo di flusso locale** (es: HDLC, PPP)|
|**Protocolli di livello 2**|Menziona almeno **PPP, HDLC, Ethernet** come esempi concreti di protocolli L2|
|**Gestione degli errori vs correzione**|Parla brevemente del fatto che **il livello 2 di solito rileva, ma non corregge** errori (→ ARQ al livello 4)|
|**Differenza tra Switch e Hub**|Già accennata, ma potresti evidenziare che gli switch operano in modo **full duplex**, mentre gli hub no|
|**Spanning Tree Protocol (STP)**|Hai accennato ai cicli, ma **non hai nominato** il protocollo che li evita|
|**Frame Ethernet**|Specifica il significato di **Type/Length** e il campo **FCS (Frame Check Sequence)** nel CRC|
|**Livello fisico**|Una frase chiarificatrice: “Il livello fisico trasmette bit, quello di collegamento frame”|

---

## 📚 Bonus: possibili domande da esame su questi temi

1. **Qual è la differenza tra rilevamento e correzione degli errori?**
    
2. **Perché il CRC è migliore del checksum per Ethernet?**
    
3. **Spiega come funziona il Binary Exponential Backoff. Perché serve?**
    
4. **Qual è la differenza tra switch e router? Chi lavora a livello 2?**
    
5. **Cosa succede in una rete Ethernet se due switch sono collegati ciclicamente?**
    
6. **Qual è la differenza tra CSMA/CD e CSMA/CA?**
    
7. **Come funziona ARP? È affidabile? Cosa succede se ricevo un ARP spoofing?**
    

---

## ✅ Conclusione

📌 **Il tuo riassunto è molto buono**, sei assolutamente sulla strada giusta.  
Tuttavia, per uno studente che vuole **massimizzare il voto**, consiglio di:

- Migliorare **termini tecnici** e rimuovere espressioni inappropriate.
    
- Aggiungere **qualche concetto mancante** (vedi tabella sopra).
    
- Prepararsi a **spiegare esempi concreti** (frame, scenari di collisione, switching vs routing).
    

👉 Se vuoi, posso anche aiutarti a creare delle **domande/risposte simulate da esame orale** o una **mappa concettuale visiva** per il ripasso. Fammi sapere!
---

## ✅ Punti positivi nella tua sintesi

Hai trattato bene:

- Le **funzioni principali** del livello di collegamento.
    
- Le **tecniche di rilevamento degli errori**: parità, checksum, CRC.
    
- I principali **protocolli MAC**: ALOHA, CSMA, Polling, Token.
    
- L’**ARP** e l’inoltro tra LAN e router.
    
- Il funzionamento di **Ethernet** e degli **switch**.
    
- Il concetto e l’uso delle **VLAN**.
    

---

## ⚠️ Cosa puoi migliorare/aggiungere per avere preparazione da 30 e lode

### 1. ❌ Errori concettuali o tecnici da correggere

- Frasi troppo colloquiali o inadeguate in ambito accademico ("fascisti di merda", "va a fanculo", "schiatta l’arbitro", ecc.) → **non usare all'esame**.
    
- L’efficienza di **ALOHA** è ≈ 18.4%, quella di **Slotted ALOHA** è ≈ 36.8% → questi valori vanno **detti esplicitamente**.
    
- In CSMA/CD, si trasmette per almeno **l’intervallo di collisione massimo** (2τ), ma il frame non deve necessariamente essere lungo 2τ _in bit_, si **garantisce che la collisione possa ancora essere rilevata entro quel tempo** → puoi chiarirlo meglio.
    

---

### 2. 🔍 Concetti mancanti o da rafforzare

|Tema|Cosa puoi aggiungere o chiarire|
|---|---|
|**CSMA varianti**|CSMA/CA (Collision Avoidance), usata in Wi-Fi, **non rileva** collisioni ma **le evita** usando RTS/CTS|
|**Controllo di flusso e accesso**|Livello collegamento può anche gestire **controllo di flusso locale** (es: HDLC, PPP)|
|**Protocolli di livello 2**|Menziona almeno **PPP, HDLC, Ethernet** come esempi concreti di protocolli L2|
|**Gestione degli errori vs correzione**|Parla brevemente del fatto che **il livello 2 di solito rileva, ma non corregge** errori (→ ARQ al livello 4)|
|**Differenza tra Switch e Hub**|Già accennata, ma potresti evidenziare che gli switch operano in modo **full duplex**, mentre gli hub no|
|**Spanning Tree Protocol (STP)**|Hai accennato ai cicli, ma **non hai nominato** il protocollo che li evita|
|**Frame Ethernet**|Specifica il significato di **Type/Length** e il campo **FCS (Frame Check Sequence)** nel CRC|
|**Livello fisico**|Una frase chiarificatrice: “Il livello fisico trasmette bit, quello di collegamento frame”|

---

## 📚 Bonus: possibili domande da esame su questi temi

1. **Qual è la differenza tra rilevamento e correzione degli errori?**
    
2. **Perché il CRC è migliore del checksum per Ethernet?**
    
3. **Spiega come funziona il Binary Exponential Backoff. Perché serve?**
    
4. **Qual è la differenza tra switch e router? Chi lavora a livello 2?**
    
5. **Cosa succede in una rete Ethernet se due switch sono collegati ciclicamente?**
    
6. **Qual è la differenza tra CSMA/CD e CSMA/CA?**
    
7. **Come funziona ARP? È affidabile? Cosa succede se ricevo un ARP spoofing?**
    

---

## ✅ Conclusione

📌 **Il tuo riassunto è molto buono**, sei assolutamente sulla strada giusta.  
Tuttavia, per uno studente che vuole **massimizzare il voto**, consiglio di:

- Migliorare **termini tecnici** e rimuovere espressioni inappropriate.
    
- Aggiungere **qualche concetto mancante** (vedi tabella sopra).
    
- Prepararsi a **spiegare esempi concreti** (frame, scenari di collisione, switching vs routing).
    

👉 Se vuoi, posso anche aiutarti a creare delle **domande/risposte simulate da esame orale** o una **mappa concettuale visiva** per il ripasso. Fammi sapere!
## ✅ **Livello di collegamento (Data Link Layer)**

- Offre **connessione logica diretta** tra due dispositivi **collegati fisicamente** (mezzo trasmissivo fisico o wireless).
    
- Funzioni principali:
    
    - **Framing**: incapsulamento dei datagrammi IP in frame.
        
    - **Error detection** (e a volte **error correction**).
        
    - **Accesso al mezzo** (es. ALOHA, CSMA).
        
- ⚠️ _Nota_: il livello di collegamento **non è necessariamente limitato a dispositivi “fisicamente vicini”**, ma solo a quelli **sullo stesso link** (es. un frame Ethernet non esce dalla LAN).
    

---

## ✅ **Implementazione**

- L’**interfaccia di rete** (es. scheda Ethernet/WiFi) implementa:
    
    - livello **fisico**
        
    - livello **data link**
        
- Il **sistema operativo** e la **CPU** gestiscono da **livello 3 (network) in su**.
    

---

## ✅ **Error Detection**

- Viene concordata una funzione `f(d) = EDC` (Error Detection Code).
    
- ⚠️ Attenzione: anche se `f(d') = edc'`, **non si ha certezza assoluta** che `d' = d`. Alcuni errori **possono sfuggire**.
    
- L’efficacia della tecnica dipende dalla qualità della funzione `f` e dal tipo di errore (es. burst, singoli bit, ecc.).
    

---

### Tecniche di rilevamento errori:

#### 🔸 **Controllo di parità (Parity Check)**

- **Singolo bit di parità** (pari/dispari): rileva **numero dispari di errori**.
    
- **Controllo bidimensionale (row + col parity)**: può correggere **un errore singolo** e rilevare alcuni errori multipli.
    
- ✅ Funziona bene quando gli errori sono rari e isolati.
    

#### 🔸 **Checksum**

- Si sommano segmenti da 16 bit (UDP-style), e si inverte il risultato.
    
- Il ricevente somma tutto: se la somma è `0xFFFF`, il pacchetto è **probabilmente corretto**.
    
- ⚠️ Meno affidabile del CRC, ma **più leggero**.
    

#### 🔸 **CRC (Cyclic Redundancy Check)**

- Vengono rappresentati dati e generatore come polinomi su GF(2).
    
- Calcola un resto (`R`) tale che il frame `<D,R>` sia divisibile per `G` (mod 2).
    
- Il ricevente **controlla che il resto sia zero**.
    
- ✅ Altissima capacità di rilevare errori strutturati o burst.
    
- ⚠️ Non corregge, **solo rileva**.
    

---

## ✅ **Tipi di collegamento**

- **Punto-punto**: solo due host (es. connessione diretta).
    
- **Broadcast**: un canale condiviso (es. Ethernet, Wi-Fi).
    
    - ⚠️ Il mezzo è condiviso → rischio **collisioni**.
        
    - Serve un **protocollo MAC** per regolare l’accesso.
        

---

## ✅ **Protocolli di accesso al mezzo**

### 📍 Obiettivi:

- Efficienza
    
- Equità
    
- Semplicità
    
- Decentralizzazione
    

### ✅ Classificazioni:

- **Channel Division**: TDMA, FDMA → divisione deterministica.
    
- **Random access**: ALOHA, CSMA → basati su probabilità.
    
- **Rotazione (Polling, Token)**: accesso ordinato.
    

---

## ✅ **ALOHA**

- **Accesso casuale**: ogni host trasmette quando vuole.
    
- **Collisioni rilevate tramite timeout**, non ascolto.
    
- Si ritenta dopo un ritardo casuale (`p`).
    
- Efficienza:
    
    - **ALOHA puro**: ~18% (`1/(2e)`)
        
    - **Slotted ALOHA**: ~37% (`1/e`)
        
- ⚠️ Correzione: ALOHA **non “si accorge” direttamente della collisione**, ma **deduce dal mancato riscontro (ACK)**.
    

---

## ✅ **CSMA (Carrier Sense Multiple Access)**

- Prima di trasmettere, **si ascolta il canale**.
    
- Problema: **ritardo di propagazione** → potrei non sapere subito che qualcun altro sta trasmettendo.
    
- **Jam signal**: per avvisare gli altri della collisione.
    
- Richiede trasmissione di almeno `2τ_max` (round-trip propagation delay).
    
- ⚠️ **Linguaggio da evitare** → frasi volgari e fuori luogo. Meglio mantenere il tono tecnico.
    

### 🔸 **CSMA/CD (con Collision Detection)**

- Utilizzato in Ethernet cablata.
    
- Conflitto gestito con **binary exponential backoff**.
    

---

## ✅ **Protocolli a turno**

### 🔹 **Polling**

- Un arbitro (controller) interroga gli host.
    
- Problema: overhead + centralizzazione (single point of failure).
    

### 🔹 **Token Passing**

- Un token gira tra host.
    
- Solo chi ha il token può trasmettere.
    
- ✅ Decentralizzato, ma:
    
    - Token può essere perso
        
    - Latenza elevata
        
    - Overhead
        

---

## ✅ **LAN e MAC Address**

- **LAN**: rete locale, es. casa, ufficio.
    
- Ogni interfaccia di rete ha un **MAC address** univoco (48 bit).
    
- ⚠️ MAC non cambia tra sottoreti, ma può essere modificato.
    

### 🔸 **ARP**

- Permette di trovare il MAC a partire da un IP.
    
- Invia una richiesta broadcast, ottiene risposta unicast.
    
- Tiene traccia nella **tabella ARP** (`IP | MAC | TTL`).
    

---

## ✅ **Ethernet**

- Inizialmente su **bus**, poi con **hub**, oggi con **switch**.
    
- Frame:
    
    - **Preambolo**: 7 byte `10101010`, poi `10101011`
        
    - **MAC dest + MAC src**
        
    - **Payload**: 46–1500 byte
        
    - **CRC**
        

### 🔸 **Switch**

- Popola la sua **tabella MAC** analizzando i frame.
    
- Se non conosce la destinazione, fa **flooding**.
    
- ⚠️ **Cicli negli switch** → problema → si risolve con **Spanning Tree Protocol (STP)**.
    

---

## ✅ **VLAN (Virtual LAN)**

- Permette di **isolare il traffico** tra gruppi logici (es. studenti/prof).
    
- Gli switch possono taggare le porte per VLAN diverse.
    
- Collegamenti tra switch con **porte trunk**, che trasportano traffico di più VLAN.
    

---

## 🔚 **Conclusione**

Hai colto moltissimi concetti corretti. Le uniche **correzioni necessarie** erano:

- Il tono in alcune parti (es. CSMA)
    
- Chiarire che ALOHA **non ascolta** e rileva errori solo indirettamente
    
- Il ruolo dei protocolli random
    
- Distinzione tra **controllo** e **correzione** degli errori
    

Se vuoi, posso trasformare tutto in una mappa concettuale o uno schema da stampare.
Il livello di collegamento offre connessione logica tra due dispositivi fisicamente vicini, collegati per via di un mezzo trasmissivo: wireless o fisico.

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


