* `nc`
* `curl`

```bash
nc -C art.uniroma2.it 80
```
Dove:
* `-C`: la sequenza `\n` e' convertita in `\r'n`
* `art.uniroma2.it 80` sono host name e porta well-known

Nota, in `HTTP1.1`bisogna usare `Connection: close` per non usare connessione persistente.

```http
GET / HTTP/1.1
Host: art.uniroma2.it
Connection: close
```

Si ottiene come risposta:
```http
HTTP/1.1 200 
Server: nginx/1.25.3
Date: Sat, 02 Aug 2025 09:42:02 GMT
Content-Type: text/html;charset=UTF-8
Content-Length: 4673
Connection: close
Set-Cookie: JSESSIONID=9F55E69E6D303CA7948BD721C30AFF3D; Path=/; HttpOnly

<!DOCTYPE html>
```

Dove:
* il server usato e' `nginx`
* Ci sono `4673` byte di risposta
* C'e' un **cookie** che va bene per **qualunque path.**

E' stata omessa `-N` che permette di chiudere le estremità della connessione in caso di `EOF`, se la connessione e' persistente.

>[!note] `Connection: close`
> *E' il client che chiede di chiudere la connessione al server!*

## nslookup: virtual hosting

```bash
nslookup art.uniroma2.it
```

Si ottiene la risposta:
```
Server:		192.168.1.1
Address:	192.168.1.1#53

Non-authoritative answer:
Name:	art.uniroma2.it
Address: 160.80.84.130
```
Dove `Address:160.80.84.130` e' l'indirizzo che cercavamo.

### 1
Ora:
```bash
nc -CN 160.80.84.130 80
GET / HTTP/1.1
Connection: close
```

Si otterrà:
```http
HTTP/1.1 400 Bad Request

```

### 2
Invece con:
```bash
nc -CN 160.80.84.130 80
GET / HTTP/1.1
Host: art.uniroma2.it
Connection: close
```

### 3
```bash
nc -CN 160.80.84.130 80
GET / HTTP/1.1
Host: vocbench.uniroma2.it
Connection: close
```

### perche' sono diversi???
Si parla di **name-based virtual hosting**: ossia, lo stesso indirizzo `IP` gestisce piu `hostname` differenti, per questo serve specificare sempre il campo `Host` nella richiesta `HTTP`

## cookie non specificato
```bash
nc -CN art.uniroma2.it
GET / HTTP/1.1
Host: art.uniroma2.it

...

GET / HTTP/1.1
Host: art.uniroma2.it
Connection: close

...
```

Le due risposte ottenute hanno `Cookie` differenti, *perché non l'
ho specificato*!

## specificare un cookie

```bash
nc -CN art.uniroma2.it
Host: art.uniroma2.it
Connection: close

Reply:
...
Set-Cookie: JSESSIONID=...; Path=/; HttpOnly
...
```

```bash
nc -CN art.uniroma2.it
GET / HTTP/1.1
Host: art.uniroma2.it
Cookie: JSESSIONID=...
Connection: close
...
```

Dove nella seconda richiesta, dopo aver specificato il `JSESSIONID` fornito dalla prima risposta, non si otterranno piu opzioni `Set-Cookie` come risposta.

## pipelining
Usando `Here Document`, posso mandare due richieste insieme per ottenere due risposte differenti.
```bash
nc -C art.uniroma2.it 80 << EOF
HEAD / HTTP/1.1
Host: art.uniroma2.it

HEAD /fiorelli/ HTTP/1.1
Host: art.uniroma2.it
Connection: close
EOF
```


## ASCII
**Nota**: non per forza bisogna inviare solo testo `ASCII`, il protocollo supporta dati puramente binari che pero' possono scombussolare la console.

## curl (es 7-11)
Stampa l'entità contenuta nel messaggio `HTML`.

```bash
curl -v https://art.uniroma2.it/
```
* `-v` abilita modalita verbose, con il quale i parametri di richiesta e risposta `http` vengono stampati su `stderr` (ossia file descriptor 2).

```bash
curl -v https://art.uniroma2.it/images/ArtLogo.jpg > ArtLogo.jpg 2> 
verbose.txt
```

Con:
* `-o` specifico file in cui salvare
* `-O` il nome del file e' derivato dal nome della risorsa
* `-f` evita di produrre file vuoti o scrivere messaggi di errore se non trovo la risorsa

Con `-b` specifico un `cookie` come `chiave=valore`, oppure posso fornire un `file` contenente il `cookie`

> con `-X method` posso indicare il metodo. Per `HEAD` scrivo `--head` oppure `-I`

Se un metodo non e' supportato ricevo `405 METHOD NOT ALLOWER`.

Con `--data-urlencode` posso inviare dati in una richiesta `POST` codificando correttamente i simboli speciali.

Con `--data-binary "data..." \ -H "Content-Type: text/plain"` specifico dati in formato binario (ossia non devono essere interpretati) da inviare e con `-H` specifico un parametro nell'header.

```bash
curl --data-binary "@bruti.txt" \
	-H "Content-Type: text/plain"
	http://httpbin.org/post 
```