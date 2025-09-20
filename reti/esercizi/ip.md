https://uniroma2.sharepoint.com/sites/CROCE-8065625-SISTEMI_OPERATIVI_E_RETI_1/Documenti%20condivisi/Lezioni/reti/SOR2024-2025_25_Esercizio_invio_host.pdf?CT=1758113927022&OR=ItemsView&wdOrigin=TEAMSFILE.FILEBROWSER.DOCUMENTLIBRARY

* `ip link` permette di gestire interfacce di rete
* `ip address`: per gestire i protocol address
* `ip route`: per la tabella di instradamento
* `ip neighbour`: il comando `ip nieght` permette di gestire le tabelle `ARP`

```bash
$ ip link
```

*per ottenere informazioni sulle interfacce di rete dove*:
* `1: lo: <LOOPBACK, UP, LOWER_UP>` indica che l'interfaccia e' attiva **amministrativamente**, e' di `LOOPBACK` ed e' collegata al livello fisico. 
* `mtu 65536` indica la `mtu` dell'interfaccia
* `qdisc noqueue` indica la disciplina di coda 
* `state UNKNOW/UP` indica che e' **effettivamente operativa** o non se ne conosce lo stato
* `2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP>` indica che supporta `BROADCAST` e `MULTICAST`.
* `qlen 1000` indica la lunghezza della coda dei pacchetti
* `qdisc mq` indica la gestione della coda a code multiple.
* `link/ether 00:15:... brd ff:ff:...` indica indirizzo mac e mac di broadcast.

> **Nota**: un collegamento `wifi` puo' essere `UP` ma con `state` non `UP` se non collegata ad un access point.

```bash
ip address
```
mostra gli indirizzi `ip` associati ad ogni interfaccia

```bash
ip route
```
mostra verso dove instradare determinati pacchetti, esiste una destinazione di default. E' specificato con `src addr` l'indirizzo sorgente da utilizzare quando si inoltra.

> **Nota**: Grazie a questo comando *possiamo conoscere l'indirizzo* `ip` del router!

```bash
ip route get 8.8.8.8
```
Con questo comando ottengo verso dove viene instradato il pacchetto!

```bash
ip neigh
```
Mostra la tabella `ARP`
> **Nota**: combinando `ip neigh` con `ip route` posso capire qual'e' il `mac` del router!


### esercizio
Considera il seguente comando
```bash
$ip route
default via 172.20.0.1 dev eth0 proto kernel
172.20.0.0/20 dev eth0 proto kernel scope link src 172.20.4.226
172.30.0.0/20 dev eth1 proto kernel scope link src 172.30.1.5
172.50.0.0/20 via 172.30.0.1 dev eth1
172.50.8.0/24 via 172.20.0.10 dev eth0
```

1. Che informazione fornisce?
2. Qual'e' l'output di `ip route get` per: `172.20.5.10`, `172.20.26.10`, `172.50.0.7`.
3. Quali sono gli indirizzi di origine e destinazione a livello di rete e collegamento per gli indirizzi sopra?

(**1**) il router si trova sicuramente a `172.20.0.1`, ed e' raggiungibile tramite l'interfaccia `eth0`.

* `172.20.0.0/20` e' la sotto rete localizzata su `eth0`, dunque tutti i pacchetti passano di qui con `ip src` `172.20.4.226`
* `172.30.0.0/20` invece si trova su `eth1`, dunque tutti i pacchetti verso questa sottorete vanno per `eth1` con `ip src 172.30.1.5`
* La rete `172.50.0.0/20` e' raggiungibile tramite il router `172.30.0.1` con uscita in `eth1` (nota l'uso di della keyword `via`)
* Invece per i pacchetti per la rete `172.50.8.0/24` sono instradati verso `172.20.0.1` che si trova in `eth0`.
(**2**) `ip route get` per:
* `172.20.5.10` $\to$ esce in `eth0` con src ...
* `172.20.26.10` esce verso `172.20.0.1` su `eth0`
* `172.50.0.7` esce da `eth1` verso `172.20.0.10`.

> **Nota**: `via` indica che per raggiungere una determinata sottorete, devo contattare un router specifico in una qualche interfaccia.

