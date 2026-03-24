**How many TCP ports are open?** A: 3
```bash
nmap -v -A -T4 htb.machine -sT
```
* `-sT`: instaura una connessione TCP prima di elencare la porta.
Eseguire il comando mostra 3 porte aperte.

