Ecco una **revisione critica** dei tuoi appunti, con correzioni e un **riassunto strutturato** per l'esame:

---

### **Correzioni alle Imprecisioni**  
1. **Livello di trasporto vs rete**:  
   - ❌ *"Il trasporto end-to-end è implementato da IP"* → **NO**, è compito del **livello di trasporto** (TCP/UDP). IP è **best-effort**.  
   - ✅ *"IP inoltra pacchetti, TCP garantisce consegna affidabile"*.  

2. **Controllo congestione**:  
   - ❌ *"Controllo congestione router"* → **NO**, TCP regola la congestione **end-to-end** (non gestisce router direttamente).  
   - ✅ *"TCP usa `cwnd` per evitare di saturare la rete"*.  

3. **UDP e streaming**:  
   - ❌ *"UDP non si regola! Congestiona la rete"* → **Dipende!** Applicazioni moderne (es. QUIC, WebRTC) usano UDP con **congestion control** a livello applicativo.  

4. **RDT 3.0**:  
   - ❌ *"L'utilizzo è dato da $\frac{L/R}{RTT + L/R}$"* → Formula corretta, ma **non è il caso migliore** (è l’utilizzo effettivo di stop-and-wait).  

5. **TCP Cubic**:  
   - ❌ *"Quando sono vicino a $W_{max}$ cresco lentamente"* → **Cresce in modo cubico**, non lineare.  

---

### **Riassunto Chiave per l'Esame**  

#### **1. Livello di Trasporto**  
- **Funzioni**:  
  - Multiplexing/demultiplexing (porte).  
  - Affidabilità (TCP) o best-effort (UDP).  
  - Controllo di flusso (`rwnd`) e congestione (`cwnd`).  

#### **2. TCP vs UDP**  
| **TCP** | **UDP** |  
|---------|--------|  
| Connessione logica (3-way handshake) | Senza connessione |  
| Affidabile, in ordine | Non affidabile, nessun ordinamento |  
| Controllo congestione (AIMD, Cubic) | No controllo congestione (ma può essere aggiunto a livello app) |  
| Header complesso (20+ byte) | Header semplice (8 byte) |  

#### **3. Affidabilità (RDT)**  
- **RDT 1.0**: Canale perfetto.  
- **RDT 2.0**: ACK/NAK per errori (stop-and-wait).  
- **RDT 2.1/2.2**: Numeri di sequenza (0 e 1).  
- **RDT 3.0**: Timeout per perdite.  

#### **4. Pipelining e Protocolli**  
- **Go-Back-N**:  
  - Finestra scorrevole, ACK cumulativi.  
  - Ritrasmissione di tutti i pacchetti da quello perso.  
- **Selective Repeat**:  
  - ACK individuali, bufferizza pacchetti fuori ordine.  
  - Ritrasmette solo i pacchetti persi.  

#### **5. Controllo Congestione TCP**  
- **AIMD**:  
  - **Additive Increase**: `cwnd += 1 MSS` ogni RTT.  
  - **Multiplicative Decrease**: `cwnd /= 2` dopo perdita.  
- **Slow Start**: `cwnd` raddoppia ogni RTT fino a `ssthresh`.  
- **Fast Retransmit**: 3 ACK duplicati → ritrasmissione immediata.  
- **TCP Cubic**:  
  - Crescita cubica dopo perdita.  
  - Meno aggressivo di AIMD in reti ad alta velocità.  

#### **6. Fairness di TCP**  
- **Fairness**: Flussi TCP competono per la banda, convergendo a un uso equo.  
- **Problemi**:  
  - Flussi con RTT più bassi ottengono più banda.  
  - UDP può sovraccaricare TCP (nessun controllo congestione).  

---

### **Cose da Ricordare**  
- **RTTₘᵢₙ** usato in BBR per evitare bufferbloat.  
- **`cwnd = min(rwnd, cwnd)`** decide la finestra effettiva.  
- **ICMP** notifica errori (es. "Destination Unreachable"), ma non gestisce congestione.  

### **Domande Potenziali**  
1. Perché UDP è usato per lo streaming nonostante sia inaffidabile?  
   - **Risposta**: Basso overhead, ritardi minimi, e applicazioni (es. VoIP) tollerano perdite.  
2. Come funziona la stima del timeout in TCP?  
   - **Risposta**: `Timeout = EstimatedRTT + 4*DevRTT` (media mobile pesata).  
3. Perché Selective Repeat è più efficiente di Go-Back-N?  
   - **Risposta**: Ritrasmette solo i pacchetti persi, non tutta la finestra.  

--- 

**Suggerimento**: Studia gli **header TCP/UDP** (campi e flag) e fai esercizi su **calcolo di `cwnd`** in diverse fasi (slow start, congestion avoidance).