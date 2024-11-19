## memoria: circuito sequenziale
un circuito sequenziale e' in grado di mantenere dei dati, riescono a mantenere lo stato nel quale si trovano
## latch
un circuito sequenziale ha delle uscite retro-azionate, ovvero che l'uscita di una porta logica diventa l'entrata di un'altra. questo metodo viene usato il **latch set-reset**.

### latch set-reset
$Q$ e' lo stato nel cui il latch può esistere.  
![[Pasted image 20240408085500.png]]

* se $S=1$ mentre $Q=0$, imposta $Q=1$
* se $R$ diventa $1$,esso resetta lo stato del latch: $Q=0$

ma se $S,R=1,1$ $Q$ e' non **deterministico**, in generale in altri ambiti dell'informatica non e' un problema ma in questo caso si.
### latch sr clocked
> il clock e' un segnale che serve a sincronizzare i circuiti.  

![[Pasted image 20240408090414.png]]
ma ancora non basta, dobbiamo introdurre il:

### D Latch Clocked
![[Pasted image 20240408090618.png]]
con l'introduzione delle porte $AND$ e' impossibile avere entrambi 1 alle entrate.  
cosi siamo in grado di evitare lo stato di indeterminazione.  

### clock
e' (**idealmente**, nella realtà non e' precisa) un'**onda quadra** dove il segnale varia tra 1 e 0. 

> il flip flop viene **eccitato** nei punti dell'onda dove il valore cambia (0 -> 1 oppure 1 -> 0)! non dove il valore rimane costante.  

### generatore di impulsi
dal punto di vista circuitale e': **placeholder**

> in fisica i segnali sono soggetti alla **latenza**: per eseguire un circuito ci vuole un tempo che non e' istantaneo.  

viene usato quindi come clock.

a seconda dei casi potremmo trovare:
* CLEAR: reset forzato
* PRESET: set forzato

## come orchestro una memoria?
con un latch posso avere 1 bit, per avere per esempio un byte allora devo prendere 8 latch e metterli insieme in un  chip.
* $Vcc$ tensione elevata, $5v$, e' una differenza di potenziale
* $GND$, ground

con un latch posso creare una memoria di tipo volatile: se la spengo perde i dati.  

* abbiamo altri piedini per leggere i dati e impostarli

come individuo la locazione di memoria che mi serve?  uso gli **indirizzi di memoria**.  

### come costruisco una memoria? esempio
* 4 celle da 3 bit
* **CS**: chip select
* **indirizzi**: ($A_{1}, A_{2}$)
* **RD**: read ($=1 \text{ (lettura))} , =2 \text{ (scrittura)}$)
* **OE**: output enable

> con il **multiplexer** posso selezionare la memoria che mi serve!

### ma a livello fisico come li connetto?  
* $1 \to 5V$
* $0 \to 0V$
se mischio $1$ e $0$ ho un **cortocircuito**! ho due soluzioni:
* **open collector**: transistor a collettore aperto, 

## chip di memoria
gli schemi dei chip di memoria sono riusabili e espandibili.  
alcuni segnali possono essere attivi in (logica di **segnale**):
* logica positivia (attivi quando valgono 1)
* logica negativa (attivi quando valgono 0)

> per valore **asserito** si intende che il valore e' attivo in base alla **logica di segnale**.  

> un segnale **negato** indica lo stato non attivo rispetto alla **logica di segnale**

generalmente abbiamo bisogno di dire al computer quando attivare e come attivare i chip:
* **CS**: che chip devo selezionare?
* **Write Enable**: devo scrivere?
* **Output Enable**: devo leggere?

posso:
* selezionare la riga asserendo: $RAS$ (Row Address Strobe)
* selezionare la colonna asserendo: $CAS$ (Column Address Strobe)
* leggo o scrivo $D$ (data).

> questo riduce il numero di pin ma rende il chip piu lento!


## RAM (Random Access Memory)
* e' volatile
* puo' essere statica o dinamica
* posso accedere a un qualsiasi dato

### RAM STATICHE (SRAM)
costruite utilizzando circuiti simili ai flip-flop D che sono in grado di mantenere l'informazione, sono molto veloci e sono utilizzate per la cache

### RAM  dinamiche (DRAM)
non usano flip-flop ma un array di transistor con un piccolo condensatore che può essere caricato o scaricato e deve essere rinfrescato.  
hanno maggiori capacita delle SRAM ma richiedo circuiti piu complessi. 

## memoria non volatile
* le ROM sono memorie a sola lettura
* i dati in una ROM sono inseriti in fase di costruzione
* la **EPROM** (Ersable ROM) si puo' essere riprogrammata usando luce ultravioletta.
* la **EEPROM** (Electrically Erasable ROM) funziona come una PROM e puo' essere cancellata tramite un impulso elettrico
* **memoria flash** e' un tipo speciale di **EPROM**

# dobbiamo costruire la ci pi u
* i segnali della CPU si possono dividere in 3 **categorie**:
	* indirizzo
	* dati
	* controllo
* a categoria di segnale corrisponde un bus

### come vengono eseguite le istruzioni????
F-D-E:
* fetch: preleva, scrivi sul bus indirizzi
* decode: decodifica, leggi sul bus dati
* execute: esegui, scrivi sul bus di controllo

#### interrupt e arbitraggio
* nel bus serve un **arbitro** che decide chi manda i dati sul bus.  
* se la cpu riceve un **interrupt**, la cpu interrompe la normale esecuzione e fa cose speciali.  

> coprocessore: aiuta il processore

# temporizzazione del bus
due tipi:
* **sincroni**: usano un segnale di clock che temporizza l'attività del bus
* **asincroni**

nei sincroni:
$$
F = \frac{1}{T}
$$
dove $F$ e' la frequenza, e $T$ e' il tempo. 

> [!note] temperature
> se in un filo stretto passa troppa corrente si riscalda.
> se passa $n^2$ corrente, il riscaldamento e' di $n^3$ (infatti i dissipatori hanno una forma a 3 dimensioni)

## esempio di bus sincrono
* ad ogni ciclo di clock corrisponde un'operazione (**temporizzato**)
* cpu (master): scrive l'indirizzo di memoria
* cpu: comunica al sistema l'operazione da svolgere (per esempio scrive su $MREQ$)
* cpu: comunica che vuole scrivere ($WR$)
* $WAIT$: quando ha finito lo slave?
* ad ogni ciclo o si aspetta o si fa qualcosa!

## bus asincrono
* **handshake** tra master e slave: si scambiano dei segnali, `MSYN` e `SSYN`.
* il bus si adatta alla velocita dei dispositivi (non sempre e' efficiente)
* e' a disposizione delle richieste
## arbitraggio
esempio: la cpu chiede alla stampante di fare qualcosa, c'e' bisogno di un arbitro (come un'autostrada).

l'arbitraggio puo' essere:
* centralizzato: presente nel chip della cpu
* decentralizzato

### arbitraggio centralizzato
* quando l'arbitro riceve una richiesta, la concede asserendo una linea di **concessione del bus**
* priorita' al dispositivo piu vicino
* `REQ`: OR-CABLATA, tutti usano la stessa linea per richiedere, all'arbitro interessa vedere se c'e' qualcuno che la vuole!
* `CONCESSIONE`: collegamento a festone, chi ha la priorita' piu alta intercetta il segnale e interrompe la propagazione, a questo punto ottiene il controllo del bus.

### arbitraggio con piu linee di priorita
non c'e' l'arbitro, i dispositivi hanno una linea di priorità (`REQ`) dedicata ad ognuno di essi e sono tutti in grado di controllare la priorita' degli altri cosi sanno come gestire la linea di `CONCESSIONE` sempre a festone.

### decentralizzato
i dispositivi si arbitrano da soli, controllando le priorita' degli altri dispositivi.  
svantaggi:
* il numero di dispositivi non puo' superare il numero di linee
* ad ogni dispositivo una linea 
* troppi collegamenti.

abbiamo:
* 

## interrupt handling
ipotizziamo di dover mandare un immagine sullo schermo:
* la cpu deve trasferire le istruzioni alla gpu

la cpu mentre la gpu elabora l'immagine, la cpu comunque e' in esecuzione. la gpu e' in grado di dire alla cpu quando ha finito di elaborare un immagine (o di svolgere) e' in grado di notificare la cpu con un interrupt.

per gestire gli interrupt le cpu hanno un chip dedicato agli interrupt.  

una cpu puo' ricevere piu interrupt contemporaneamente: si puo accedere al vettore degli interrupt (????)
