* `/etc/hosts`: mappature locali
* `/etc/resolv.conf`: individuare il `nameserver` da contattare, viene fornita una lista di `nameserver` locali. `/etc/nsswitch.conf`: in che ordine devo interrogare le fonti?

in `/etc/nsswitch.conf`, `host: files dns` indica che `/etc/hosts` ha la priorità sul protocollo `dns`

```bash
dig +norecurse @dns.google.com uniroma2.it

; <<>> DiG 9.20.11 <<>> +norecurse @dns.google.com uniroma2.it
; (4 servers found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 35400
;; flags: qr ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;uniroma2.it.			IN	A

;; ANSWER SECTION:
uniroma2.it.		2202	IN	A	160.80.1.247

;; Query time: 21 msec
;; SERVER: 8.8.4.4#53(dns.google.com) (UDP)
;; WHEN: Sat Aug 02 13:06:34 CEST 2025
;; MSG SIZE  rcvd: 56
```
> Non ricevo **referral** se faccio richiesta non ricorsiva al dns di google.

> **referral**: hostname da contattare per risolvere il dns se chi ho contattato non riesce.

> i **nameserver root** non gestiscono la ricorsione ma restituiscono dei referral.

```bash
dig @a.root-servers.net uniroma2.it
; <<>> DiG 9.20.11 <<>> @a.root-servers.net uniroma2.it
; (2 servers found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 51998
;; flags: qr rd; QUERY: 1, ANSWER: 0, AUTHORITY: 6, ADDITIONAL: 13
;; WARNING: recursion requested but not available

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;uniroma2.it.			IN	A

;; AUTHORITY SECTION:
it.			172800	IN	NS	a.dns.it.
it.			172800	IN	NS	m.dns.it.
it.			172800	IN	NS	dns.nic.it.
it.			172800	IN	NS	v.dns.it.
it.			172800	IN	NS	nameserver.cnr.it.
it.			172800	IN	NS	r.dns.it.

;; ADDITIONAL SECTION:
a.dns.it.		172800	IN	A	194.0.16.215
a.dns.it.		172800	IN	AAAA	2001:678:12:0:194:0:16:215
m.dns.it.		172800	IN	A	217.29.76.4
m.dns.it.		172800	IN	AAAA	2001:1ac0:0:200:0:a5d1:6004:2
dns.nic.it.		172800	IN	A	192.12.192.5
dns.nic.it.		172800	IN	AAAA	2a00:d40:1:1::5
v.dns.it.		172800	IN	A	194.0.25.44
v.dns.it.		172800	IN	AAAA	2001:678:20::44
nameserver.cnr.it.	172800	IN	A	194.119.192.34
nameserver.cnr.it.	172800	IN	AAAA	2a00:1620:c0:220:194:119:192:34
r.dns.it.		172800	IN	A	193.206.141.46
r.dns.it.		172800	IN	AAAA	2001:760:ffff:ffff::ca

;; Query time: 25 msec
;; SERVER: 198.41.0.4#53(a.root-servers.net) (UDP)
;; WHEN: Sat Aug 02 13:11:10 CEST 2025
;; MSG SIZE  rcvd: 423
```

* flag `rd`: recursion desired
* flag `ra`: ricorsione non disponibile

```bash
╰─ ❯❯ nslookup web.uniroma2.it
Server:		192.168.1.1
Address:	192.168.1.1#53

Non-authoritative answer:
Name:	web.uniroma2.it
Address: 160.80.1.247
```
 * `192.168.1.1#53` e' il nameserver invocato per la risoluzione.
 * `Non-authoritative answer` vuol dire che la risposta viene per esempio da una cache

## risolvere la posta!

```bash
╰─ ❯❯ nslookup -type=MX uniroma2.it
Server:		192.168.1.1
Address:	192.168.1.1#53

Non-authoritative answer:
uniroma2.it	mail exchanger = 25 mx-03.uniroma2.it.
uniroma2.it	mail exchanger = 20 mx-01.uniroma2.it.
uniroma2.it	mail exchanger = 20 mx-05.uniroma2.it.
uniroma2.it	mail exchanger = 10 mxb-00727301.gslb.pphosted.com.
uniroma2.it	mail exchanger = 25 mx-04.uniroma2.it.
uniroma2.it	mail exchanger = 10 mxa-00727301.gslb.pphosted.com.
uniroma2.it	mail exchanger = 20 mx-02.uniroma2.it.

Authoritative answers can be found from:
```

dove `mail exchanger = xxx` indica la **preferenza** a `16-bit`, si sceglie quello con valore piu basso.

Per risolvere la mail di posta (con l'opzione `A` per ottenere associazione `hostname - ip`)
```bash
nslookup -type=A mx-01.uniroma2.it
Server:		192.168.1.1
Address:	192.168.1.1#53

Non-authoritative answer:
Name:	mx-01.uniroma2.it
Address: 160.80.6.34
```

## es 5
```bash
nslookup -type=NS .
Server:		192.168.1.1
Address:	192.168.1.1#53

Non-authoritative answer:
.	nameserver = k.root-servers.net.
.	nameserver = e.root-servers.net.
.	nameserver = h.root-servers.net.
.	nameserver = g.root-servers.net.
.	nameserver = j.root-servers.net.
.	nameserver = b.root-servers.net.
.	nameserver = i.root-servers.net.
.	nameserver = l.root-servers.net.
.	nameserver = f.root-servers.net.
.	nameserver = m.root-servers.net.
.	nameserver = a.root-servers.net.
.	nameserver = d.root-servers.net.
.	nameserver = c.root-servers.net.

Authoritative answers can be found from:
```

Per ottenere i nameserver per il percorso radice `.`, ossia i 13 nameserver!

```bash
nslookup -debug -norecurse -nosearch -type=A web.uniroma2.it h.root-servers.net 
```
Dove con `-debug` ci accorgiamo che sono stati **ritornati dei server autoritativi**, e in `ADDITIONAL RECORDS` troviamo le associazioni `ip-hostname`. 

La traduzione per `web.uniroma2.it` ovviamente non ha avuto successo, dobbiamo chiedere a qualche altro `dns autoritativo`

```bash
nslookup -norecurse -nosearch -debug -type=A web.uniroma2.it 194.0.16.215
Server:		194.0.16.215
Address:	194.0.16.215#53

------------
    QUESTIONS:
	web.uniroma2.it, type = A, class = IN
    ANSWERS:
    AUTHORITY RECORDS:
    ->  uniroma2.it
	nameserver = dns.uniroma2.it.
	ttl = 3600
    ->  uniroma2.it
	nameserver = dns1.uniroma2.it.
	ttl = 3600
    ->  uniroma2.it
	nameserver = ns1.garr.net.
	ttl = 3600
    ADDITIONAL RECORDS:
    ->  dns1.uniroma2.it
	internet address = 160.80.5.8
	ttl = 3600
    ->  dns.uniroma2.it
	internet address = 160.80.1.3
	ttl = 3600
------------
Non-authoritative answer:
*** Can't find web.uniroma2.it: No answer
```

Ora con:
```bash
nslookup -norecurse -nosearch web.uniroma2.it 160.80.5.8
```
Viene ritornato l'indirizzo `160.80.12.247`.

> `3600` indica un `TTL` di 1 ora.

## es 6
Con:
`nslookup -type=NS uniroma2.it` ottengo il nameserver autoritativo per tradurre `uniroma2.it`

Invece `nslookup -type=NS web.uniroma2.it` fornisce una risposta diversa, dice che `uniroma2.it` e' autoritativo per `web.uniroma2.it`.

```bash
╰─ ❯❯ nslookup -type=NS web.uniroma2.it
Server:		192.168.1.1
Address:	192.168.1.1#53

Non-authoritative answer:
*** Can't find web.uniroma2.it: No answer

Authoritative answers can be found from:
uniroma2.it
	origin = dns.uniroma2.it
	mail addr = postmaster.uniroma2.it
	serial = 2025074100
	refresh = 86400
	retry = 7200
	expire = 604800
	minimum = 1800
```

> `Nota`: `uniroma2.it` e' un dominio e il suo server autoritativo gestisce la traduzione anche per `web.uniroma2.it`!
