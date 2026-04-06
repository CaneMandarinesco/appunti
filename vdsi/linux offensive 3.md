**kernel exploits**: programmi che attaccano alcune versioni del kernel.

**dirty cow**: permette di sostituire l'utente root con un'altro a cui posso fare accesso.
* **instabile**: l'uso di questi exploit rende il sistema instabile.
* **crash**: potrebbe mandare in crash la macchina.
* **log**: posso essere sgamato.

**dirty cow non conviene**: i tool di difesa se ne accorgono subito che succede qualcosa di strano.

**cosa fare se ho abbastanza privilegi** (per qualsiasi strano motivo):
* **modificare** `/etc/passwd`: aggiungere un nuovo utente **senza password** con **user id root**

```bash
echo "hacker::0:0:root:/root:/bin/bash" >> /etc/passwd
```
* **modificare** `/etc/sudoers`: in modo che posso fare sudo con l'utente corrente.
```bash
eve ALL=(ALL) NOPASSWD:ALL
```
* `ALL=(ALL)`: posso eseguire su tutti gli host per conto di tutti gli utenti.
* `NOPASSWD:ALL`: tag ed a seguire il comando `ALL`
* **effective user id**: sudo lo cambia momentaneamente a seconda delle regole in `/etc/sudoers`

* **creare fake bash**: 
	* `cp /bin/bash ~/myBash`
	* `chmod 4777 ~/myBash`: mi permette di eseguire `/bin/bash` come 
	* `~/myBash -p`: preserva `SUID`. anche se ho fatto `chmod 4777` devo dire a bash di preservare.
	* **all'esecuzione**: id e group id sono quelli dell'utente, mentre quello effective e' dell'utente proprietario di `/bin/bash`

* **password cracking su** `/etc/passwd` e `/etc/shadow`


## path hijacking
**script**: ho uno script con privilegi elevati che posso eseguire che invoca un comando come per esempio `curl`
**path**: metto un path custom con ranking elevato, per esempio `/home/vickie/bin`
**script custom**: metto in `/home/vickie/bin` un programma **evil** che si chiama `curl` che viene eseguito dallo script originale, che magari ha permessi elevati.

## wildcards
* `sudo -l`: lista dei comandi che un utente puo eseguire come sudo
* **regola con wildcard**: se `/etc/sudoers` contiene `ALL:ALL NOPASSWD: /usr/bin/cat /opt/scripts/*`
* **allora posso fare**: `/opt/scripts/../../etc/sudoers`
* **sfilza di** `..`: ne metto tanti, abbastanza da arrivare a `/`. Se sono troppi non fa niente, non ho nessun errore.

#### 7zip
* `touch @tosteal`: per 7 zip e' un insieme di file di cui fare il backup.
* `echo "/etc/shadow" > tosteal`
* `7za a backup.7z -t7z -snl *`
* *si usa quando uno script con privilegi elevati fa il backup di una cartella dentro cui posso scrivere. Con `@tosteal` faccio injection di file sensibili dentro il backup.*


### python exploitation
* **python 2**, script che usano `input()`: in python 2 e' vulnerabile. Posso dare `__import__('os').system('/bin/bash')` ed eseguire cose con i privilegi del programma (magari a `SUID`).
* **python 3**: e' sicuro di default.
* **python module override**: quando python importa moduli legge in un'ordine specifico
	* 1.`current dir`
	* 2. built in
	* 3. `PYTHONPATH`
	* 4. std lib directories
	* 5. installati con pip
* **sudoers script replacement**: guardo in sudoers gli script che posso modificare.

### cronjob
* **scenario 1: posso modificare lo script** e fare lateral movement se non e' root, oppure copiare la shell e mettici SUID.
* **scenario 2**: **sola lettura**. allora devo fare path hijacking oppure sfrutto le wildcard.

## binari insicuri
* **find**: cerca binari con `suid`
* **getcap -r /**: cerca capabilities in `/`
* **nota**: gli editor di testo possono eseguire comandi e aprire shell.
* `cap_setuid` su python, gdb, ecc.... Posso creare script che fanno la chiamata a `setuid`
* `cap_dac_read_search`: posso legge quello che voglio
* `cap_dac_override`
* https://gtfobins.org
