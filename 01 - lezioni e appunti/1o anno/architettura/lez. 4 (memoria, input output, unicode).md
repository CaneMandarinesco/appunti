# dispositivi interni al computer
## scheda madre
* CPU
* connettori RAM
* bus comunicazione (per moduli DIMM)
* connettori per I/O
* connettori IDE/SATA
* batteria per tenere l'orario e altre info
* interfacce di rete

i dispositivi di I/O sono costituiti da un **controllore** e il **dispositivo stesso**, talvolta i controllori per comodita' sono gia' integrati sulla scheda madre e possiamo collegare quindi il dispositivo a una porta della scheda madre.

il controllore e' in grado di governare il dispositivo a cui e' collegato ed e' in grado di accedere al bus.

## bus interno (DMA, ISR)
e' il sistema di comunicazione usato per collegare CPU, memoria e I/O.  ogni dispositivo I/O ha un **controller** che lo interfaccia al bus.  i bus possono essere **seriali o paralleli**.  
il bus interno e' tipicamente parallelo: si possono trasmettere piu bit contemporaneamente, al contrario quelli seriali hanno pochi cavi ma trasmettono i bit in serie.  
* problema semantico: chi e' che coordina il transito di informazioni (chi e' il master, chi e' lo slave)?  
* problema tecnico: come gestisco due segnali che vanno sulla stessa uscita
oltre al dispositivo **fisico** (es. hard disk che hanno dei movimenti meccanici) abbiamo quindi un **controller**:
* pilota l'hardware (per esempio la testina del drive)
* logica di controllo per il dialogo
* tipicamente si utilizzava 1 bit per filo

si e' passati alla comunicazione seriale per diminuire il numero di fili.  
e' utile abbassare anche la tensione di funzionamento:
* meno consumo (+ batteria)
* meno riscaldamento (col riscaldamento si creano imperfezioni/cambiamenti di stati nel silicio, dove passa la corrente)

tre tipi di bus:
* bus **dati**: per il passaggio delle informazioni. dove chiunque puo' immettere e leggere dati
* bus di **controllo**: per assicurare il **sincronismo** sul bus

nei computer piu vecchi esisteva anche il bus ISA (Industry Standard Architecture).

### scrittura DMA
consideriamo di voler leggere da un hard disk magnetico (tempi di lettura molto lenti). esiste una tecnica che si chiama **DMA** (Accesso Diretto in Memoria), i dati vengono trasferiti sul bus dati senza che la CPU piloti l'operazione. 

come fa la CPU a sapere che il trasferimento e' stato terminato? grazie ad un segnale fisico/software viene chiamata la routine: **ISR** (Interrupt Service Routine / Interruzione). la cpu deve interrompere quello che sta facendo (ed poi riprendere l'esecuzione) *salvando lo stato dei registri* e viene eseguita la routine ISR (gia programmata).  una volta terminata la cpu riprende l'esecuzione.

> tutto cio' viene fatto per ridurre i tempi di attesa! (-tempo di attesa = meno consumi inutili)

> l'interruzione e' un evento asincrono imprevisto (non so quando avverra' ma so cosa devo fare quando avviene)!

### DMA
poiche' il bus e' condiviso, nessun'altro deve immettere dati nel bus. per questo scopo esiste un **arbitro del bus** (chip fisico).
* I/O hanno precedenza sulla CPU.
* quando nessuno usa il bus, la CPU puo' usare i cicli del bus per accedere alla memoria.
* **cycle stealing**: se per esempio un hard disk deve accedere al bus, questo ha priorita' sulla CPU!

### BUS ISA
* IBM inventa la linea PS-2 perche' era piu veloce
* ma non la usava nessuno, i produttori continuano a usare la vecchia linea ISA -> i dispositivi non erano compatibili!

### computer due bus
architettura che gestisce periferiche **IESA** e PCI
...

### BUS PCI 
lo brevetta Intel.  
direttamente dalle slide del prof: il bus pci utilizza una rete punto-punto con varie linee seriali con linee seriali di bit a commutazione di pacchetto (PCIe 3.0 ha una larghezza di banda 17GB/sec).  
dal libro: la cpu comunica con un controllore con una linea dedicata, questo controllore e' collegato con una linea dedicata al bus della memoria (che lo collega alla memoria) e al bus PCI (che lo collega ai dispositivi PCI). quindi ora il traffico tra **CPU** e **memoria** non occupa il bus PCI.   
in sostanza pero PCI e' soltanto un *bus ISA con delle modifiche*, il cambio radicale lo abbiamo con **PCIe**.  
PCIe e' una rete punto a punto che usa linee seriali di bit e commutazione di pacchetto, piu' simile a internet che ad un bus tradizionale.  
anche se PCIe usa una comunicazione di tipo seriale (trasmette i bit in serie), e' piu' efficiente rispetto al mandare i bit in parallelo. in serie si puo' sfruttare una frequenza piu' alta che compensa la mancanza di parallelismo.

# dispositivi Input Output
sono dispositivi inseriscono o mostrano dispositivi dal/al mondo esterno.  
## tastiera
quando viene premuto un tasto **viene generato un interrupt** che viene catturato dal sistema operativo. i tasti shift, ctrl, alt, ecc... sono gestiti esclusivamente via software.

## extra: touch screen
nasce nel 1968, invece i touch screen capaci di rilevare il famoso **pinch-to-zoom** nascono gia nel 1985.  esistono touch screen trasparenti e opachi, e quelli trasparenti sono realizzati con 3 diverse tecnologie:
* raggi laser/infrarossi: disposti in modo da formare una griglia
* resistivi: abbiamo uno strato superiore e uno inferiore, quando i due strati si toccano in un punto specifico l'hardware fa in modo di riconoscere dove.

queste tecnologie funzionano bene con un solo dito dato che puo' accadere in questi dispositivi il fenomeno del ghosting. abbiamo un'altra tecnologia:
* touchscreen capacitivo a proiezione, di cui esistono diverse tipologie.  tipicamente sfruttano le proprieta' del condensatore, in un tuttitouchscreen capacitivo abbiamo una griglia di fili (sottili 50 micron) divisi da uno strato isolante (creando un condensatore), e il dito facendo contatto con lo schermo modifica la capacitanza di **tutti** i punti toccati (dato che il corpo umano contiene cariche elettriche) e il computer e' poi in grado di rilevare dove e' avvenuta la pressione.  sopra questo strato troviamo una lastra di vetro: lo spessore determina la sensibilita' dello schermo.

## monitor
uno schermo piatto LCD e' fatto di molecole organiche viscose (**cristalli liquidi**) che si possono muovere come in un liquido. la variazione di un campo elettrico cambia l'allineamento delle molecole e delle proprieta' ottiche.  la tecnologia LCD e' molto complessa dato che esistono tantissimi tipi di schermi, andiamo a considerare la tecnologia TN dove abbiamo:
* placca posteriore: solchi orizzontali
* placca frontale: solchi verticali
le molecole tendono a allinearsi a questi solchi e per cambiare placca basta effettuare una rotazione nello spazio intermedio.  la posizione delle molecole modifica l'intensita' della luce del pixel.  
esiste inoltre un'altra tecnologia: TFT (thin film transistor) dove ogni pixel puo' essere modificato grazie ad un commutatore (detto TFT)

## RAM nella scheda video (VRAM)
la maggior parte degli schermi ha un refresh di 60/100 Hz (cicli al secondo) e accedono a una memoria speciale (VRAM) che si trova nel controller video.  
la vram contiene una **bitmap** che rappresenta l'immagine sullo schermo (tal volta ne troviamo 2).  
il colore di ogni pixel e' identificato da 3 byte, ciascuno per uno dei colori primari.  poiche' la manda minima per rappresentare dati video e' piu alta dello standard EISA  o PCI per connettere la scheda video alla CPU.  
alcuni processori invece che scrivere nella VRAM direttamente le intensita di ogni singola colore (RGB, rosso blu e verde), piuttosto utilizzano un unico numero a 8-bit per indicare un colore da una **palette**  
### mouse (dispositivo di puntamento)
diversi tipi:
*  inizialmente meccanico, avevamo una rotella che agiva su due potenziometri che calcolavano la distanza percorsa. successivamente le rotelle vengono sostituite da una pallina
* **ottico**, rileva il movimento grazie ad una "fotocamera" che rileva le imperfezioni della superficie (i primi mouse ottici richiedevano un tappetino con una griglia stampata per contare quante righe sono state passate al fotorilevatore)
* opto-meccanico, rileva il movimento grazie a dei rotori con dei forellini che calcolano la distanza di movimento
indipendentemente dal tipo un mouse spedisce, via comunicazione seriale, sempre 3 byte:
1. coordinata x
2. coordinata y
3. stato dei pulsanti

### extra: wiimote
per rilevare il **movimento** del controller in uno spazio a 3 dimensioni abbiamo un accelerometro a 3 assi. questo accelerometro contiene 3 piccole masse che si muovono indipendentemente su queste assi, ovviamente col tempo l'

> altri dispositivi di puntamento sono il trackpad e la trackball

### stampanti
tecnologie:
* a matrice
* getto d'inchiostro
* laser
#### impatto a matrice
usano una matrice di punzoni elettromagnetici che scorre longitudinalmente mentre il foglio scorre sotto. 
* monocromatiche
* lente
* rumorose
* bassa qualita'

#### inkjet (getto d'inchiostro): casalinghe
similmente all'impatto a matrice un foglio scorre sotto la matrice che puo' scorrere in orizzontale. vengono espulse delle goccioline minuscole (tanto minuscole), la cui grandezza e' regolata dal voltaggio. esistono due tecniche: piezoelettriche e termiche (l'inchiostro evapora ed esce dall'ugello raffreddandosi)
#### laser
producono dei fumi tossici ma:
* alta qualita
* veloci
* silenziosi
* costi bassi
tamburo (**drum**) di precisione rotante, abbiamo bisogno di **alta temperatura** (generato da alta tensione 1000V), il laser modella il tamburo modificandone le proprieta' elettriche in modo da avere aree in cui si accumula o meno il toner (che e' elettrostaticamente sensibile).  un raschietto pulisce il tamburo dai residui di toner che non sono stati impressi sul foglio.   per pilotare le stampanti esistono dei veri e propri linguaggi di programmazione idonei allo scopo (PostScript, PCL, PDF, linguaggi che descrivono le pagine da stampare).  
#### extra: scala di grigi e colori
ma come riproduco la scala di grigi? divido l'immagine in mezzitoni, ovvero sezioni con piu o meno pixel neri e c'e' un numero prestabilito di neri che ogni cella puo contenere.

invece per la stampa a colori si mischiano i 3 colori ***sottrattivi***: **Ciano, Magenta e Giallo (Yellow)** e dato che e' difficile ottenere un nero perfetto a partire da questi 3, si e' aggiunto il nero (blacK): da questi 4 colori abbiamo le stampanti **CMYK**. ora il problema non banale e' che:
* i monitor utilizzano luce **trasmessa** (un fascio di luce), mentre le stampanti usano luce **riflessa** (da un materiale)
* i CRT generano 256 intensità di colori mentre le stampanti usano i **mezzi-toni**
* i monitor hanno un fondo nero, la carta di solito e' bianca
* i colori RGB e CMYK sono diversi.

quindi c'e' bisogno di creare uno standard per calibrare bene gli strumenti tra loro!
## apparati per telecomunicazioni
inizialmente si usava il doppino telefonico (progettato per inviare audio). grazie ad un segnale sinusoidale (**portante**, la base dei sistemi di comunicazione) che ha una frequenza compresa tra 1kHz e 2Khz si puo trasmettere un segnale fatto di 0 e 1 senza gravi problemi causati dalla distorsione del segnale. variando l'ampiezza/fase/frequenza di questo segnale e' possibile trasmettere i bit, ovvero il processo di **MODulazione** del segnale, al contrario avviene la DEMulazione (???)
### modem
* modulazione di **frequenza**: ampiezza costante mentre varia la frequenza della portante in corrispondenza dei valori 0 e 1.  se potessimo ascoltare questo segnale si sentirebbero due note diverse in corrispondenza dello 0 e dell'1
* modulazione di **fase**: la fase viene invertita di 180 gradi per segnalare un cambiamento (in alcuni sistemi altre variazioni indicano la variazione di coppi e di bit, modulazione a coppia di bit)
* modulazione di **ampiezza**

con il **baud** si indica il numero potenziale di variazioni al secondo del segnale, che e' diverso dal **bit rate** (bit trasmessi al secondo)!

di solito vengono trasmesse sequenze di bit dove abbiamo un byte che indica l'inizio del segnale e un byte che indica la fine del segnale.  
i modem possono essere:
*  **full-duplex**: trasmettono in entrambe le direzioni a frequenze diverse
* **half-duplex**: trasmette in una direzione **in un dato istante**
* **simplex**: trasmettono in una sola direzione 

> nota: qualsiasi mezzo e' dispersivo (ovvero le onde elttriche/sonore ecc... possono incontrare degli ostacoli)

nasce la **DSL** (Digital Subscriber Line) e' una tecnologia che permette di superare le limitazioni dei modem (56 kbp), l'offerta piu popolare e' l'ADSL
...

### asymmetric digital subscriber line (ADSL)
* il canale 0 trasmette il servizio telefonico tradizionale (Plain Old Telephone service)
* i canali 1-5 non sono usati (per evitare interferenze con i canali sopra)
* 250 canali di cui 2 usati per il controllo in upstream e downstream.
* ogni canale corrisponde ad una banda di frequenze utilizzabili

una rete ADSL e' formata da:
* NID (Network Interface Device), segnala dove inizia la lina dell'utente e dove finisce la linea di proprieta' dell'azienda telefonica.
* filtro: divide il segnale in due uscite, una per le frequenze fino a 4000Hz (voce) e il resto per i dati.
* modem adsl: e' un processore analogico che funziona in modo da interpretare in parallelo 250 segnali. (come se fossero 250 modem)

### internet via cavo (+ dalla lista numerata e' extra)
al contrario dell'ADSL dove ogni utente ha il suo cavo, con l'internet via cavo abbiamo un unico cavo che dal ISP va alla centralina del nostro quartiere e tutti gli utenti dell'ISP sono allacciati alla centralina, cio' significa che piu utenti condividono lo stesso cavo.  e' importante notare che i modem dal momento che sono accesi fino a che non si spengono mantengono la connessione attiva all'ISP.  
un modem quando viene acceso:
1. cerca un pacchetto che viene spedito periodicamente dall'ISP (contente varie informazioni tecniche)
2. il modem comunica la sua presenza
3. il modem determina la sua distanza (**ranging**) spedendo un pacchetto di test, utile regolare in modo appropriato l'uso dei minislot
4. richiede all'ISP l'assegnazione di un indirizzo ip dinamico
5. stabilire le chiavi di **cifratura** (poiche' i dati viaggiano su un unico cavo! algoritmo Diffie-Hellman permette cio anche se apparentemente e' impossibile)

i **minislot** sono una divisione temporale dei canali **in uscita**, e in questi minislot il modem puo' trasferire i dati.  per conoscere i minislot al modem disponibili prima di inviare i dati il modem deve inviare una richiesta, successivamente l'ISP fornisce i minislot assegnati da usare.  se non ci sono minislot liberi il modem dovra' aspettare un tempo casuale (che si raddoppia ad ogni tentativo fallito). 

per i dati **in entrata** non c'e' bisogno di minislot dato che esiste un solo mittente per i dati (ovvero l'ISP). di solito si usa una dimensione di 204 byte per i dati in entrata, dato che in generale sono usati in quantita maggiore rispetto all'uscita dagli utenti normali.  

---

# codifica dei caratteri!
* ASCI (standard americano)
* UNICODE
* UTF-8
per la codifica si usano i numeri naturali

### ASCII
codice molto semplice che usa 7 bit: $2^7 = 128$ combinazioni.  questo sistema contiene anche caratteri **non-stampabili** ma che servono alla trasmissione dei dati.  
questo sistema funziona bene per le lettere anglosassone *ma male per altre lingue* che usano caratteri non presenti sulla mappa ASCII (come rappresento simboli come $\Omega \, \, \Gamma$ ecc... ??).  
da un consorzio nasce: **UNICODE**.  

### UNICODE
ogni carattere e' codificato con 16-bit (65 mila e passa combinazioni).  risolve vari problemi ma manca:
* ordinamento degli ideogrammi (alcune lingue si basano sull'ordine degli ideogrammi)
* nascita di **nuovi ideogrammi**: esempio nomi di persona nuovi
* usa la stessa codifica per caratteri che sembrano uguali ma che in realta' hanno differenti significati.

* per esempio un **dizionario giapponese** ha 50k simboli kanjj
* unicode spreca 16 bit per codificare anche ASCII

### UTF-8
il primo bit della codifica UTF-8 indica se sto rappresentando un carattere ASCI, altrimenti gli altri 7 bit indicano il numero di byte da leggere. infatti grazie a UTF-8 posso usare piu byte per rappresentare un carattere, ne posso rapressentare tanti eliminando i problemi di UNICODE. inoltre e' retrocompatibile!