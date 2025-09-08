* `/etc/hosts`: mappature locali di nomi e indirizz
* `/etc/nsswitch.conf`: ordine delle fonti

Il seguente comando manda una interrogazione al DNS server e mostra le risposte, similmente a quanto fa `nslookup`:
```bash
dig +norecurse @dns.google.com uniroma2.it
```

con il seguente comando avremo come risposta `NXDOMAIN`: ossia non essite!
```bash
nslookup asdasdasd.uniroma2.it
```

invece con:
```bash
nslookup -type=MX uniroma2.it
```
avremo tutti i record per server di posta `SMTP`, dove per ogni entry abbiamo un `preference value`.

possiamo dunque risolvere `mx-01.uniroma2.it` cosi:
```bash
nslookup -type=A mx-01.uniroma2.it
```

per ottenere i root-server:
```bash
nslookup -type=NS .
```

### query iterative
```bash
nslookup -nosearch -norecurse -type=A web.uniroma2.it 198.97.190.53
```
*dove l'indirizzo ip fornito e' di un root server* dove `-norecurse` indica che voglio una query iterativa, dunque il root server non contattera altri server.

Ottengo in risposta i **referral** per i nameserver responsabili del dominio `.it`.

Contattando il nameserver fornito, ottengo l'ip per web.uniroma2.it

> Con sezione `autoritativa` si intendo i server che gestiscono l'area di domini in questione.

Con il seguente comando ottengo il server autoritativo per `uniroma2.it`
```bash
nslookup -type=NS uniroma2.it
```

Con `dig` ottengo un *output piu verboso* rispetto a `nslookup` per la risoluzione dei nomi, di solito richiede il record di tipo `A`

Quando richiedo il tipo `A`, `dig` mi fornisce la catena di alias (ossia record di tipo `CNAME`) che mi porta al record `A`.
