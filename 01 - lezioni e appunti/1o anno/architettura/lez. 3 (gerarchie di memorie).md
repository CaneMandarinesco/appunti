## gerarchie di memorie
alla base della piramide troviamo i **registri**.
nella piramide:
* dalla cima verso il basso, **il tempo di accesso e il costo per unita' decrementa**
* al contrario, **cresce la capacita'**

> la dimensione della memoria e' inversamente proporzionale al tempo di acceso e direttamente al costo.

> i nastri magnetici sono grandi ma lenti ( e economici )

### dischi magnetici
sono fatti da piatti di alluminio che attraverso la **isteresi magnetica** possiamo modificare i bit incisi su di esso. (ogni bit e' lungo 1-2 nanometri)  
poi abbiamo la **testina** che fa ruotare il disco e un **solenoide** rileva lo stato di magnetizzazione.  
le tracce del disco sono divisi in **settori** (di **grandezza fissa**, 512 byte), e tra ogni settore c'e' uno spazio (**gap**).  

> [!note] settori
> - preambolo
> - dati
> - ecc


ogni traccia contiene un **preambolo** e serve ad allineare le testine e contiene un **CCE** (puo' essere hamming o reed-solomon). una traccia e' una sequenza di byte che viene scritta con una sola operazione

> quindi la capacita' effettiva di un disco magnetico la si puo' conoscere dopo che e' stato formattato, per via della presenza dei gap che non contengono dati.

un controllore del disco (disk controller) collega il disco al bus.  
i dischi per definire la grandezza dei settori, i gap ecc... ha bisogno di essere formattato.

> il **floppy disk** e' un disco magnetico. lo inventa l'ibm per motivi aziendali ma poi diventa perfetto per distribuire software per i personal computer.

un disco magnetico puo' avere piu dischi (generalmente 6), ognuno con ogni sua testina di lettura/scrittura.
#### hard disk 
l'hard disk magnetico (???) ha 3 tempi:
* tempo medio di **seek** (per raggiungere la locazione desiderata): $5$ ai $10 ms$
* tempo **rotazionale**, selezione del settore (il tempo del motore per muoversi/**inerzia**): $3$ a $6ms$
* burst rate: lettura della testina
* sustain rate: calcolatore che interpreta ida
inoltre essendo la velocita' di rotazione sempre la stessa, piu' si va verso l'esterno del disco e piu' i dati saranno piu' lenti da leggere. questo problema e' arginato dividendo i dischi in zone/
#### IDE
i dischi IDE sono i primi dischi magnetici. i primi avevano una velocita' di trasferimento di $4MB /s$, poi si e' riusciti ad arrivare a $16MB /s$. il sistema operativo ci puo' accedere grazie al **BIOS** (Basic Input Output System).  
versioni successive di IDE:
* EIDE: 
	* LBA: indirizzamento a settori
	* 128GB
	* due canali, per ognuno due dischi
* ATA-3
* ATAPI-4
* ecc...

con ATAPI-7 viene introdotta l'interfaccia **serial ATA** permette di trasferire 1 bit alla volta su un connettore a 7 pin con alimentazione a $0.5V$ (prima erano 80 pin a $5V$) per una velocita' minima di 150MB/s.  

#### SCSI Disks (Small Computer System Interface)
diventa standard ANSI per collegare dispositivi (HD, CD-ROM, Scanner, nastri).  
sono piu' veloci dei dischi IDE nonostante cilindri, tracce e settori siano organizzati nello stesso modo.  
un cavo scasi ha 50 contatti:
* 25 di terra accopiati con altri 25 per eliminare il rumore elettrico.
* 8 sono per i dati
* 1 per la parita
* 9 per il controllo
* 2 per alimentazione

le periferiche collegate tramite SCSI possono funzionare come iniziatari o destinatari di comunicazione, quindi si possono collegare piu dispositivi insieme (al contrario di IDE)

### RAID (Redaundant Array Of Inexpensive Disks)
come in CISC vs RISC, qui la controparte e' SLED (Single Large Expensive Disk).  
il RAID e' un server che contiene al suo interno tanti dischi che grazie ad un controllore appaiono come unico (SLED) al computer che vi accede.   
esistono diversi modi di organizzare questi dischi: esistono 5 livelli (+ altri minori)
#### RAID livello 0
i dati sono suddivisi su `k` settori (ovvero **strip**) e memorizzati in dischi diversi con modalità **round-robin**. in questo modo un blocco di dati puo' essere letto con 4 letture parallele. questo schema funziona su grandi quantita' di dati al contrario su pochi dati non e' vantaggioso. ovviamente il controllore del raid gestisce la richiesta dei dati e va a richiedere in parallelo le strip dai dischi.  
i sistemi operativi non traggono vantaggio da questo metodo perche' generalmente leggono i dati in piccole quantita' (pochi settori insieme) quindi non si puo' sfruttare il parallelismo.
![[Pasted image 20240502115541.png]]
#### RAID livello 1 (il vero RAID)
evoluzione del livello 0. se abbiamo 4 dischi viene effettuata la duplicazione di questi su altri 4 dischi aggiuntivi.  
quando scriviamo il controller raid scrive 2 volte ma quando leggo, avendo a disposizione due copie posso leggere in parallelo!  
e se un disco si rompe ho a disposizione l'altro!
![[Pasted image 20240502115548.png]]
#### RAID livello 2
divido i byte in nibble (4 bit) e aggiungo il codice di Hamming, quindi ho 7 bit di dati dove i bit 1, 2 e 4 sono di parita'.  
i bit vengono divisi tra i dischi cosi se si rompe un disco grazie alla parita' posso ricostruirlo tutto quanto per intero. pero' e' molto dispendioso perche' bisogna calcolare sempre il codice di hamming.  
i dischi per essere letti devono essere sincronizzati sulle stesse posizioni! 
![[Pasted image 20240502115721.png]]
#### RAID livello 3
versione semplificata del 2 che usa un disco per immagazzinare la  parita' (contiene i bit di parita' del codice di hamming) cosi se si rompe un disco posso ricostruirlo a partire da uno
![[Pasted image 20240502115726.png]]
#### RAID livello 4 e 5 (oppure S)
lavorano con strisce e non richiedono dischi sincronizzati

### dischi a stato solido (SSD)
basati su una memoria flash non volatile. si usurano nel tempo a causa dell'usura dei transistor e possono quindi perdere capacita' di memorizzazione. alte prestazioni fino a 3 volte un disco magnetico, più tolleranti alle vibrazioni in quanto non presentano parti meccaniche

### CD-ROM (Compat Disk Read Only Memory)

> hanno una vita stimata di 100 anni!

dischi fatti di policarbonato con uno strato di alluminio riflettente e ricoperto da una vernice resistente. questi materiali si possono degradare col calore. nel substrato di policarbonato si possono creare delle depressioni (**pit**) tra aree non incise (**land**). esiste un valore di variazione tra **pit** e **land**.  
quindi vengono lette delle **scalanature**

#### memorizzazione
i dati sono memorizzati usando un preambolo (16 byte) contenente:
* una sequenza decimale da 12 byte che permette al lettore di riconoscere l'inizio di un settore
* 3 byte per riconoscere il numero del settore ( cosi posso cercare il dato che voglio)
* 1 byte per il modo: con o senza ECC (Error Correction Code)

#### settori
due modalità: 1 (*con errore*) e 2 (*senza errore*, potrebbe non avere senso correggerlo, come formati audio/video dove un errore ogni tanto non e' un problema). 

non era possibile fin da subito immagazzinare video e audio sulla stessa traccia in un CD-ROM, c'e' stato bisogno di un "aggiornamento".

#### extra
per usare lo stesso cd-rom su diversi computer c'era bisogno di creare un filesystem universale, quindi i produttori di pc si riuniscono e fanno High Sierra, composto da 3 livelli:
1. nomi file di 8 caratteri e eventualmente 3 caratteri per l'estensione (in stile MS-DOS), solo MAIUSCOLE, numeri e underscore. directory annidate per un massimo di 8 livelli
2. nomi file di 32 caratteri
3. file non contigui in memoria

#### CD-R (registrabili)
contengono:
* *strato di pigmento* che permette di **scrivere i pit**. contengono 
* una scanalatura che permette di guidare il laser in fase di scrittura. 

durante la scrittura il raggio laser colpisce il pigmento rompendo un **legame** molecolare (viene riscaldata la superficie) che **non si puo' ripristinare**. nonostante siano utilizzati materiali e tecniche diverse rispetto al CD-ROM (dove troviamo veri e propri solchi nell'incisione), un lettore CD-ROM e' in grado di interpretare le variazioni del riflesso del laser anche su un CD-R.

### CD-ROM-XA (extra)
viene poi introdotta con il CD-ROM-XA la scrittura incrementale, si possono aggiungere i dati nel tempo.  
problema: i CD-ROM avevano un unico VTOC (Volume Table of Contents, "indice del disco") che non si poteva aggiornare con le scritture successive (quindi se si scriveva qualcosa, i dati non comparivano nell'indice (????)).  
come soluzione si decide di assegnare ad ogni traccia il suo VTOC, ma ogni traccia doveva essere scritta in modo contiguo quindi se l'hardisk impiegava troppo tempo a cercare i dati si potevano creare degli errori.  soluzione:  creare un file detto immagine della grandezza di 650MB che raggruppa in maniera contigua tutti i file da scrivere. (pero' leggere 650mb potrebbe essere faticoso per alcuni hardisk che entrano in "panico" per via del surriscaldamento).

#### CD-RW (riscrivibili)
al posto del pigmento troviamo una lega che ha due stati stabili: cristallino e amorfo, dove il laser puo' avere 3 diverse intensita:
* alta potenza: la lega viene portata allo stato amorfo (bassa riflettenza, **pit**)
* media potenza: la lega viene portata allo stato cristallino (alta riflettenza, stato naturale, **land**)
* bassa potenza: legge le variazioni nel riflesso

costano una fracca.  

> i cd-r sono ottimi per i backup perche' una volta scritti non possono essere modificati (e costano meno dei CD-RW)


#### DVD (Digital Versatile Disk)
progettati in modo simile ai CD ma variano le specifiche tecniche del laser (si usa un laser rosso), dei pit ecc...  
abbiamo una serie di miglioramenti che ci porta a poter immagazzinare fino a 4,7GB.  
il passaggio al laser rosso implica che i lettori ora devono avere 2 laser diversi.  dato che forse 4,7GB non bastano hanno definito 4 formati:
* singolo lato, singolo strato
* singolo lato doppio strato (8,5GB)
* doppio lato singolo strato (9,4GB)
* doppio lato doppio strato (17gb)
tutti questi formati nascono da vari conflitti tra le aziende che decisero di implementarli tutti.  
I DVD nascono principalmente per il mercato cinematografico

### blu-ray (extra)
i blu-ray sono cd che usano un laser blu che:
* ha una lunghezza d'onda piu' piccola
* piu' preciso nella messa a fuoco
quindi permette di avere pit e land piu piccoli e di arrivare a una capienza di 25GB

### SSD
le memorie a stato solido non presentano parti meccaniche, e per questo sono più indicate per computer portatili dove degli urti potrebbero danneggiare i suoi componenti.
* ROM: memoria scrivibile grazie ad una maschera. producendone in grande quantità si abbassano notevolmente i costi
* PROM: memoria scrivibile bruciando dei fusibili
* EPROM: memoria scrivibile e cancellabile mettendo il chip dentro una camera a raggi violetti. dopo una decina di minuti lo stato dei bit della ROM e' resettato.
* EEPROM: memoria scrivibile e cancellabile elettronicamente
* memoria flash: memoria scrivibile e cancellabile a settori

una memoria flash funziona grazie al processo di **iniezione a caldo** grazie al qualche caricando una tensione elevata in un transistor si e' in grado di far rimanere intrappolata la carica all'interno di questo. successivamente usando una tensione minore si puo' testare il transistor per interrogarne il valore.

