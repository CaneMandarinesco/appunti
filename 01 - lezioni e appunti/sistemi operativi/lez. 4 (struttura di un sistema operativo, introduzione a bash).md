* **programma principale** invoca le chiamate di sistema
* **kernel**: blocco monolitico, tutto il software e' contenuto in una cartella. Si evita che a livello utente sia impossibile di eseguire operazioni di sistema. Contiene l'implementazione delle procedure di servizio.

Perché la maggior parte dei sistemi operativi sono monolitici:
* e' più facile avere un unico programma piuttosto che diversi servizi connessi tra loro. 
* se voglio disaccoppiare il software, potrebbe essere necessario renderlo meno efficiente (attivare e disattivare eventuali moduli). Dovrei aver bisogno di gestire invio e ricezione di messaggi tra moduli (aggiunge un livello di complessità: quello di comunicazione).
* se non fosse monolitico, alla fine dovrei compilare in un unico eseguibile e collegare tutti i moduli tra loro.
* in un sistema operativo non monolitico, potenzialmente, ogni programma o modulo potrebbe chiamare altre procedure di altri moduli. E' più vulnerabile e sensibile.
* se cambio processo, ho un cambio di contesto ogni volta che eseguo procedure di moduli diversi.
* struttura a tre strati: user, os e hardware
* alcune cose sono cambiali a **caldo**: posso caricare driver e file system a caldo (per esempio: monto un )

> un kernel può essere monolitico, ma avere dei moduli all'interno. Ma non sono completamente modulari: uno può dipendere dall'altro (non si puo' disaccoppiare) e se ne cambio uno, spesso ho bisogno di riavviare.

> quando aggiungo una scheda video e installo il suo driver, devo riavviare per aggiornare la tabella delle procedure disponibili, che sono delicate in quante eseguibili solo dall'os.

### librerie condivise
In UNIX trovo le `shared libs`, in Windows i DLL che sono visibili a tutti.

### struttura del sistema operativo monolitico
* ogni strato puo' comunicare con il successivo o con il precedente. non puo' saltare di livello
* Struttura: Kernel <--> System Calls <--> Shared Libs <--> User Programs

Quindi, tutte le funzionalità sono concentrate nel Kernel. Questa organizzazione pero' e' poco scalabile

### Macchine Virtuali
Permettono di eseguire piu

### Struttura della virtualizzazione
Virtualizzazione di tipo 1:
* viene eseguito direttamente sull'hardware (come Xen) 

Virtualizzazione di tipo 2:
* un Hypervisor gestisce le macchine virtuali, traduce gli input che vengono dall'alto (esempio: indirizzi di memoria) per il sistema operativo base.
* le chiamate di sistema per l'hardware vengono tradotte: **Machine Simulator**
* molti livelli di traduzione: poco performante
* in linux: esistono dei moduli che permettono di saltare la traduzione di alcuni moduli (kernel modules), compromesso tra tipo 1 e 2.

### container
Virtualizzazione leggera, uso un sistema operativo usando il kernel dell'host.
I container sono separati l'uno dall'altro. Le librerie che servono ad una app si devono trovare dentro lo stesso container.
Devo controllare la compatibilità del container con il kernel che sto eseguendo.
Inoltre un container e' completamente volatile. 

Per esempio: Google usa container. 

> Il sistema operativo controlla tutto. Le app in esecuzione in un container pensano di essere eseguite dentro ad un sistema operativo.
### exokernel 
e' un tipo di virtualizzazione. 
* permette di separare il controllo delle risorse: ogni macchina virtuale ha un quantitativo di hardware limitato. 
* esegue direttamente su hardware

### unikernel 
sistemi operativi minimali, non sono general-purpose, ma mette a disposizione le librerie per eseguire poche applicazioni, esempio: un web server. Se tolgo funzionalita al mio sistema operativo, mi espongo meno alle vulnerabilita.

Sono varianti dei container, mettono a disposizione un sistema controllato di chiamate e app che posso usare.

### microkernel client/server
i processi dialogano attraverso messaggi, anche le chiamate di sistema sono eseguite cosi. sistema di messaggistica. Viene applicato su un kernel molto scarno, altrimenti il sistema di messaggi non sarebbe sostenibile.

in questa organizzazione: se si rompe un modulo, gli altri continuano a funzionare.

# introduzione a bash
* bash e' una delle righe di comando disponibili (esiste sh, bsh, zsh, ecc...)

> "small programs that do one thing well"

uso tanti piccoli programmi per fare qualcosa di complesso.k