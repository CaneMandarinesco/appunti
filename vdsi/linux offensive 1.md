**privilege escalation**: sfruttare software e permessi per arrivare a root

# shell ristretta 
```bash
echo $SHELL
echo $0 # posso vedere come e' stata lanciata la shell
```

**eseguire bash dentro bash ristretta**: se configurata male la shell ristretta, posso eseguire `bash`, ossia non ristretta.

**editor di testo dentro bash ristretta**: posso usare `vim` o `nano` per eseguire comandi.

**python**: `python3 -c 'import pty; pty.spawn("/bin/hash")'`

**script**: `script -qc /bin/bash /dev/null`. In questa configurazione, script lancia un comando.
* `script`: registra quello che scrivi e l'output dei comandi

`/bin/b*sh`: l'asterisco funziona come wildcard.

**raggirare restrizione sui nomi dei comandi**: `p${u}i${u}n${u}g` permette di eseguire `ping` se questo e' stato ristretto nella shell.
* `w'u'h'u'o'u'a'u'm'u'i`: le `u` tra **apici** sono letti come comandi non trovati. pero' viene eseguito `whoami`

**rieseguire il comando precedente** `!-1`: riesegue l'ultimo comando. `!-2` riesegue il penultimo

```bash
user@linux$ whoami # fallisce. allora...
user@linux$ mi
user@linux$ whoa
user@linux$ !-1!-2
# permette di eseguire whoami anche se la bash lo impedisce controllando la sintassi
```

**spazio usando variabile ambiente**: `cat${IFS}/etc/passwd`
**usare variabili ambiente come vantaggio**: `IFS=];b=/bin/bash];`

**spazio puo' essere proibito**: potrei non poter inserire lo spazio in certe shell ristrette.

**uso delle graffe**: `{ping,localhost}`

**uso esadecimale**: `$'\x2f\x62...'`
* `X=$'\x2f\x62...';$X`
* **cyberchef**: ha un tool per la conversione esadecimale.
* **xxd**: fa la conversione

**altre cose su**: https://www.verylazytech.com/linux/bypassing-bash-restrictions-rbash

# privilege escalation

1. **enumeration**: raccolgo informazioni
2. **finding attack vectors**:
3. **exploit them**: 

# enumerazione

**MOTD**: message of the day. E' un messaggio che spesso viene stampato al login e potrebbe contenere informazioni interessanti da parte dell'amministratore.

**uname**: `uname -a` 
**lsb-release**: `cat /etc/lsb-release`
**informazioni release**: `cat /etc/*release*`
**whoami**:
**cwd**:
**id**
**groups**
**ottieni utenti che possono loggare:** `cat /etc/passwd | grep sh`
**leggi history**: `cat .bash_history`
**leggi bashrc/zshrc**: `cat .bashrc` contiene comandi e direttive eseguite prima di avviare la shell.
**alias**: 


