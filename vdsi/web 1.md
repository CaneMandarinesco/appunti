**porte socket**:
* **HTTP**: 80
* **HTTPS**: 443
* **SSH**: 22

gemini: che vuol dire che la porta dipende dal protocollo di trasporto?

**NAT**: piu device possono condividere un singolo ip pubblico. 
* **source address**: il router lo modifica quando inoltra il pacchetto in internet.
* **home e corporate networks**: utilizzato spesso.

**Firewalls**: controlla quale traffico puo' entrare/uscire dalla rete.
* **allow/deny** del traffico pacchetti, basandosi su regole.
* **criteri**: *source ip, dest ip, source port, dest port, protocol, ecc...*

**DNS**: traduce i nomi in ip

**URL**:![[Pasted image 20260423012215.png]]
* **protocollo**: indica come la risorsa dovrebbe essere acceduta
* `[<username>:<password>@]`: sono opzionali, e permettono di connettersi per conto di un'utenza. 

**basic connectivity**
* `ping`: controlla se host raggiungibile e la latenza
* `traceroute`: mostra il path dei pacchetti verso la destinazione.
* `arp / ip neigh`: tabella arp
* `ip / ifconfig`: mostra interfacce network e la configurazione
* `route / ip route`:  mostra le tabelle di routing

**netcat**:
*  **TCP**: `nc <target> <port>` (connettiti) ed `nc -l -p <port>` (ascolta)
* **UDP**: `nc -u <target> <port>` (connettiti) ed `nc -l -p <port>` (ascolta)
* **disable DNS**: `-n`
* **esegui programma e ridireziona IO**: `nc -l -p <port> -e <program>`

## DNS
**DNS**: distribuito su server, gearchico,
* **delegazione**: ogni livello delega al successivo la risoluzione.
* **zone**: i dati di un dominio e un sottoinsieme dei suoi sotto-domini costituiscono una zona
* **es**: `dns.uniroma2.it`, `www.uniroma2.it`, `mail.uniroma2.it` si trovano nella zona di `uniroma2.it` nell'immagine, sotto il dominio `.it`
![[Pasted image 20260423013837.png]]

gemini spiega: Uniroma2 could have separate nameservers on each 
department, each responsible for their own 
subdomain
The parent domain would delegate responsibility for 
a subdomain to each department, making many 
zones

**attori DNS**:
* **client**: un programma che usa un dominio.
* **server**: database di dati DNS
* **resolver**: software che accetta query da un client e interroga uno o piu server DNS. ritorna al cliente una risposta alla query.

**query ricorsiva**: il server a cui faccio richiesta se ne ha bisogno manda query ad altri server per risolvere il nome.
* **no referral**: non mi viene ritornato nessun referral, ci pensa il server!

**query iterativa**: se il server a cui faccio richiesta non ha la risposta, mi da il referral ad un'altro DNS server.

**record types**: gemini che cos'e' un record type
* `A`: `IPv4`; `AAAA`: `IPv6`.
* `MX`: Mail Exchange
* `PTR`: Host name corresponding to IP address. (???)
* `NS`: Host name del server autoritativo per il nome dato.
* `CNAME`: nome canonico del nome da risolvere.
* `SOA`: identifica il server responsabile per l'informazione del dominio.
* `TXT`: testo generico.

**risoluzione in linux**, ordine delle fonti per la traduzione:
* `/etc/hosts`: permette di fare mapping manuali.
	* `127.0.0.1 localhost`, `192.168.1.10`
* **cache**
* **DNS resolver**

`dig`:
* `dig hostname`
* `dig hostname record-name`
* `dig @dns-server hostname record-name`
* `dig @dns-server hostname any`


