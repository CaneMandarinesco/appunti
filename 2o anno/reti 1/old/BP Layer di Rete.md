
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
3. **Su rete (crossbar)**: alta concorrenza, usata per alte prestazioni
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

### Intra-AS
- **RIP**: distance vector, semplice.
- **OSPF**: link-state, usa flooding e supporta ECMP per bilanciamento.
- Architettura gerarchica (backbone + aree locali).
---
### Inter-AS:
- **BGP** (Border Gateway Protocol)
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