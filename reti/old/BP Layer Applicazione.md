### **Correzioni e Integrazioni ai Tuoi Appunti**

#### **1. Layer di Applicazione**
✅ **Corretto**:  
- Protocolli variano in base a QoS, TCP/UDP, realtime, paradigma (client-server/P2P), sicurezza.  
- Implementato solo agli endpoint (ma CDN/Proxy possono operare nel "nucleo").  

❌ **Da correggere**:  
- *"Il nucleo non interessa il tipo di pacchetto"* → **Falso**, i router possono usare **DSCP** (field ToS in IP) per QoS.  
- *"Creare un protocollo personalizzato"* → Possibile, ma deve rispettare standard di interoperabilità (es. porte registrate IANA).  

🔹 **Aggiungi**:  
- **Cookie**: File memorizzati lato client per tracciare sessioni (es. login). Usati per:  
  - Autenticazione (es. session ID).  
  - Personalizzazione (preferenze utente).  
  - Tracking (pubblicità).  
  - **Problemi**: Privacy, sicurezza (Cookie hijacking).  

#### **2. HTTP**  
✅ **Corretto**:  
- Protocollo stateless, richieste/respose ASCII, metodi (GET, POST, ecc.).  
- Codici di stato (1xx-5xx).  

❌ **Da correggere**:  
- *"HTTP non persistente"* → Moderno HTTP/1.1+ usa **connessioni persistenti** (più richieste su stessa connessione TCP).  
- *"PUT è idempotente"* → Vero, ma **POST no** (crea risorse multiple se ripetuto).  

🔹 **Aggiungi**:  
- **Web Cache**:  
  - Riduce latenza e traffico (es. cache browser/CDN).  
  - Usa header HTTP (`Cache-Control`, `ETag`).  
- **Proxy Server**:  
  - Filtra/accelera traffico (es. Squid).  
  - Può fare caching (forward proxy) o bilanciamento carico (reverse proxy).  

---

### **Riassunto per Domande d'Esame**  

#### **1. Email & SMTP**  
- **Componenti**: Mail server (MTA), client (MUA), protocolli (SMTP, POP3/IMAP).  
- **SMTP**:  
  - Comandi: `HELO`, `MAIL FROM`, `RCPT TO`, `DATA` (terminato da `\r\n.\r\n`).  
  - **Dot-stuffing**: Escape di `.` iniziale in `DATA` con `..`.  
- **Push (SMTP) vs Pull (HTTP)**: SMTP invia attivamente, HTTP richiede passivamente.  

#### **2. DNS**  
- **Gerarchia**: Root → TLD (.com, .it) → Autoritativi (es. google.com).  
- **Query**:  
  - **Ricorsiva**: Server risolve per conto del client.  
  - **Iterativa**: Server restituisce "chi chiedere".  
- **Record**:  
  - `A` (IPv4), `AAAA` (IPv6), `MX` (mail), `CNAME` (alias).  
- **Caching**:  
  - **Vantaggio**: Riduce carico/latenza.  
  - **Problema**: Stale records (TTL gestisce expiry).  

#### **3. P2P & BitTorrent**  
- **Vantaggi P2P**: Scalabilità (banda totale = somma upload peer).  
- **Strategie**:  
  - *Rarest first*: Previene "estinzione" di chunk.  
  - *Tit-for-tat*: Incentiva reciprocità (no free-riding).  
  - *Optimistically unchoked*: Scelta casuale per scoprire peer veloci.  
- **Tempo distribuzione**:  
  - **Client-server**: Max tra `F/u` (server→client) e `F/d` (download client).  
  - **P2P**: Max tra `F/u_min`, `F/d_min`, `NF/(u_s + Σu_peer)`.  

#### **4. Streaming & CDN**  
- **DASH**: Adatta qualità video in base a banda (chunk separati).  
- **CDN**:  
  - *Enter deep*: Server vicini all’utente (bassa latenza).  
  - *Bring home*: Meno server, più grandi (costo inferiore).  
- **Accesso CDN**: DNS redireziona al server "migliore" (geolocalizzazione).  
- **Buffering**: Compensa jitter (variazione RTT).  
  - **Gestione jitter**: Buffer + prefetch (es. YouTube).  

---

### **Punti Critici da Memorizzare**  
1. **Cookie**:  
   - `Set-Cookie` header (server → client), `Cookie` header (client → server).  
   - **Same-Origin Policy**: Limita accesso tra domini.  
2. **Web Cache**:  
   - **Validation**: `If-Modified-Since`/`ETag` per evitare re-download.  
3. **Proxy**:  
   - **Forward Proxy**: Maschera client (es. aziende).  
   - **Reverse Proxy**: Maschera server (es. load balancing).  

### **Domande Potenziali**  
1. **Perché SMTP usa dot-stuffing?**  
   - Per evitare che `.` su riga singola (fine messaggio) sia interpretato male.  
2. **Come funziona il DNS caching?**  
   - Riduce query ripetute, ma TTL impone expiry per evitare stale data.  
3. **Perché BitTorrent usa "optimistically unchoked"?**  
   - Per evitare che peer lenti siano esclusi permanentemente, mantenendo fairness.  

**Suggerimento**: Studia **header HTTP** (es. `Cache-Control`) e **algoritmi P2P** (es. choking in BitTorrent).