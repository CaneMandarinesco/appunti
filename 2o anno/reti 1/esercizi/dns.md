* `/etc/hosts`: associazioni locali personalizzate
* `/etc/resolv.conf`: chi devo contattare per il dns 
* `/etc/nssitch.conf` ordine in cui leggere le risorse per risolvere i nomi

```bash
dig +norecurse @dns.google.com uniroma2.it
```
questo comando chiede al dns di google di risolvere uniroma2.it senza usare ricorsione, quindi mi ritorna il prossimo server da contattare per ottenere il record `A`.

Il record richiesto compare nella sezione autoritativa, nella sezione delle risposte non ho niente.

```bash
dig +norecurse @8.8.8.8 uniroma2.it
```

fa la stessa cosa di prima.

> [!note] `rd` e `ra`
> `rd`: ricorsione richiesta
> `rd`: se la ricorsione e' accettata


```bash
nslookup web.uniroma2.it
```
Il comando risolve per me, il nome e ritorna l'ip.


> [!note] `NXDOMAIN`
> ottengo come risposta `NXDOMAIN` se non e'siste!


> [!note] `type=A`
> Se in `nslookup` specifico `type=A` richiedo esplicitamente che non venga ritornato un campo `AAAA` per `ipv6`.

```bash
nslookup -norecurse -nosearch -type=A web.uniroma2.it 198.97.190.53
```

Con `-nosearch` evito di provare a risolvere usando la `search` list dei domini.

```bash
dig +noall +answer @46.166.189.68 www.ibm.com
```

> [!note] `search list`
> La search list e' una lista di nomi da provare a utilizzare per risolvere domini incompleti. Se per esempio eseguo `nslookup server1`, automaticamente provo `server1.com` `server1.stocazzo.net` ecc.... Se indico `--nosearch` allora evito di provare queste combinazioni definite localmente.

> [!note] name server
> danno solo risposte iterative!

```bash
dig +trace uniroma2.it
```
Il comando mostra i passaggi iterativi per la risoluzione del server.

> **Nota**: con `ANY` ottengo tutti i campi associati ad un dominio