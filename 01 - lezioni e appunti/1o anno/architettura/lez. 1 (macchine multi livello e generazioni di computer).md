due tipi di grandezze:
* analogica: varia in modo infinitesimo
* digitale: le informazioni sono codificate con una sequenza di **valori discreti**, può essere trattato da un calcolatore.

### teorema del campionamento
il teorema garantisce che un campionamento di un segnale analogico e' senza perdita di informazione se la frequenza di campionamento e' pari al doppio della frequenza massima camionabile.

* ADC -> Analog To Digital Converter 
* DAC -> Digital To Analog Converter

### il calcolatore
* usa il sistema di numerazione binario
* puo' sommare numeri, controllare se una cifra e' zero, copiare dati ecc...
* contiene una CPU, che riconosce un unico linguaggio: il **linguaggio macchina**.

### problema della distanza concettuale 
* tra il linguaggio naturale e il linguaggio del computer esiste una distanza concettuale
* al livello 0 esiste una macchina quindi che interpreta il linguaggio usando il linguaggio L0 (questo livello e' costituito dal computer)
* possiamo creare una **macchina virtuale** (M1) che utilizza un linguaggio L1 che si interfaccia con il livello L0
* questi **livelli di stratificazione** (layering) servono a colmare la distanza tra linguaggio macchina e linguaggio naturale.

### creare una macchina virtuale
si puo' creare in due modi:
#### 1. traduzione
il programma in $L_{1}$ e' convertito da un programma $L_{0}$ chiamato **traduttore** in programma $L_{0}$ **equivalente**.
* il programma e' composto di istruzioni in $L_{0}$
* e' ottimizzato per $M_{0}$ e non può essere usato in nessun'altra macchina.
* il programma prima di essere eseguito deve essere tradotto interamente, quindi una volta tradotto e' possibile cestinare il programma $L_{1}$
#### 2. interpretazione
un programma scritto in $L_{0}$ chiamato **interprete** esamina istruzione per istruzione il programma $L_{1}$ e lo decodifica in linguaggio $L_{0}$

---

il nostro computer e' formato quindi da un insieme di macchine virtuali ($M_{0}, M_{1},\dots ,M_{n}$), dove il linguaggio $L_{k}$ non puo' differire piu' di tanto dal linguaggio $L_{k-1}$ (altrimenti l'approccio a strati non sarebbe pratico).   

a livello teorico, sarebbe possibile costruire una macchina che al livello $0$ sia in grado di eseguire codice C o Java, ma risulterebbe complesso e **costoso** (economicamente non conveniente).  

quindi il linguaggio $L_{0}$ e' l'unico in grado di essere eseguito direttamente sui componenti digitali mentre gli altri devono essere necessariamente tradotti.  

![[Pasted image 20240305171710.png]]

> [!note] OSS.
> non puo' accadere che del codice da un livello, per esempio,  $L_{3}$ passi al livello $L_{1}$ senza passare per $L_{2}$. se cio' fosse possibile allora ci sarebbero probabilmente dei problemi di sicurezza.

se scriviamo programmi per una macchina $M_{k}$ non e' nostro interesse, conoscere lo stato degli interpreti/traduttori sottostanti. In qualsiasi caso il linguaggio in $M_{k}$ viene eseguito.
### hardware e software
* hardware: oggetto tangibile
* software: idee astratti, algoritmi o istruzioni
* oggi, hardware e software ***sono logicamente equivalenti***, perché ogni istruzione hardware può essere simulata dal software

## macchine multilivello
### livello 0
al livello 0 troviamo i transistor (dispositivi analogici), che possono essere correttamente simulati da dispositivi digitali (con valori 0 o 1), le porte (o **gate**).  
ogni porta ha 1 o piu' input digitali e calcola in output dei valori.  
grazie alle porte e' possibile combinarle per creare una **memoria da 1 bit**, e combinando più memorie in gruppi e' possibile creare memorie da 16, 32 o 64, ovvero i **registri**.
ciascun registro puo' contenere un valore che varia fino ad un certo limite.

### livello 1: micro-architettura
e' presente una memoria locale, formata da un gruppo di registri (da 8 a 32) e una **ALU** (Arithmetic Logic Unit), capace di effettuare semplici operazioni aritmetica. i registri sono connessi alla ALU tramite un **percorso dati** che permette di selezionare su quali registri effettuare le operazioni e dove immagazzinare il risultato.  
queste operazioni talvolta sono controllate da un programma chiamato **micro-programma**, oppure sono controllate dall'hardware.  
se il controllo avviene via software, allora il **microprogramma** e' un interprete per il livello 2.

### livello 2: ISA (Instruction Set Architecture Level)
Livello di architettura dell'insieme d'istruzioni, ogni produttore di computer pubblica un manuale che illustra le istruzioni presenti in questo livello.

### livello 3 (sistema operativo)
Puo' condividere alcune istruzioni con il livello 2 (e' lecito), abbiamo una diversa organizzazione della memoria ed e' possibile eseguire programmi in parallelo.  
*le istruzioni nel livello 3 che sono identiche al livello 2, vengono eseguite direttamente sul microprogramma* e non dal sistema operativo, mentre altre le interpreta il sistema operativo stesso.  
per questo motivo, tale livello e' detto **ibrido**

### livello 4 (linguaggio assemblativo)
e' un punto di spaccatura. troviamo programmi traduttori (assemblatori)

proprietà dei livelli inferiori a 3:
* non concepiti per essere usati dal programmatore medio
* concepiti per eseguire interpreti e traduttori
* generalmente sono costituiti da interpreti
* i programmi sono scritti in sequenza numeriche 

proprietà dei livelli superiori a 4:
* concepiti per risolvere problemi applicativi
* troviamo piu spesso traduttori
* **i linguaggi contengono parole o abbreviazioni linguistiche**

il livello 4 e' quindi una rappresentazione simbolica di uno dei linguaggi sottostanti, permette di scrivere programmi per i livelli 1, 2 e 3 in modo piu' efficiente.
il livello 4 e' il linguaggio assemblativo

### livello 5 (linguaggi ad alto livello)
consiste di linguaggi (ad alto livello!) come: C,C++,Java,Python,PHP ecc...
Questi programmi possono essere tradotti al livello 3 o al livello 4 utilizzando un **compilatore**, e in casi meno frequenti possono essere anche interpretati.  
Java per esempio e' un linguaggio che viene compilato in un linguaggio ISA (java byte code) che poi viene interpretato

per riassumere: ciascun livello di un computer rappresenta una diversa astrazione ed e' caratterizzata da oggetti e operazioni diversi. ***si e' cosi in grado di ridurre la complessità del computer.***

si definisce infine come ***architettura***: l'insieme delle caratteristiche di un livello i quali utenti possono vedere, come il quantitativo di memoria usata.
#### invenzione microprogrammazione
nel 1951 nasce la prima macchina con **interprete che eseguiva il livello ISA** (***microprogramma***) riducendo cosi l'hardware necessario per eseguire istruzioni piu' complesse.
#### invenzione sistema operativo
nel 1960 nasce un computer con un software sempre attivo che gestisce l'hardware (**sistema operativo**). l'idea nasce dal fatto che eseguire e eseguire il debug di codice fortran ai tempi era molto complicato (si utilizzavano schede perforate, le macchine erano costose quindi si usava prenotare un computer e inoltre il debug era complicato dato che l'unico output erano delle luci). il sistema operativo leggeva ed eseguiva automaticamente le schede perforate, automatizzando parte del lavoro del programmatore.
con il passare del tempo il sistema operativo diventa sempre piu' complicato fino a diventare un livello a se stante, inoltre nascono anche macchinismi per la condivisione di tempo della macchina (cosi che piu' programmatori potessero utilizzare lo stesso computer)
#### migrazione verso il microcodice
nel 1970 il microprogramma si arricchisce con istruzioni e tecniche. abbiamo varie istruzioni di basso livello che controllano direttamente il microprocessore. prima queste erano eseguite direttamente sull'hardware, ma adesso il software puo' simulare l'hardware. fa da tramite tra CPU e ISA

#### eliminazione della microprogrammazione
viene eliminata la microprogrammazione perche' e' lenta. il microcodice diventa la nuova microprogrammazione

## pietre miliari (generazioni di computer)
### generazione zero
* macchina **pascalina** (1600): fa somme e sottrazioni
* macchina **Leibniz**: fa anche moltiplicazioni e divisioni
* macchina **analitica** di babbage.
* primi computer a relè (1930)
* ENIGMA
* COLOSSUS: primo computer digitale

tutti questi tentativi di creare dei computer non furono dei completi successi per via delle limitazioni tecnologiche.

* mark I (alan turing), computer a rele'
non siamo ancora nel pieno dell'elettronica!

### prima generazione: valvole
* ENIAC, computer a valvole termoioniche, si programmava attraverso migliaia di leve e di cavi. da qui nasce l'interesse per i calcolatori elettronici.

* macchina di **von neumann** (implementata nel EDSAC) (neumann e' un genio pazzo in culo), architettura alla base di oggi, costituita da 5 componenti fondamentali:
	* memoria
	* ALU (Airthmetic Logic Unit)
	* Control Unit
	* Input
	* Output
* in questo periodo i calcolatori cominciano ad approdare sul mercato

### seconda generazione: elettronica, 1955-1965
* TX-0: primo computer a transistor e con un bus (Omnibus)
* **transistor**: utilizzano i transistor e implementa il concetto di **bus**
![[Pasted image 20240305210339.png]]

grazie al bus, ogni componente può dialogare con gli altri, utilizzando una linea comune. questo porta vari vantaggi: se dobbiamo aggiungere un dispositivo di I/O, possiamo aggiungerlo semplicemente collegandolo sulla linea comune  
ciò introduce il **problema del cortocircuito**: come posso collegare vari elementi insieme e **riconoscere chi sta mandando i segnali**? chi e' che ascolta e che parla? se due oggetti parlano allo stesso momento avviene un cortocircuito. (problema del sincronismo)
### terza generazione 1965-1980
***circuiti integrati***: permettono di inserire in un chip decine di transistor aumentando velocità del processore e abbattendone i costi
i processori nuovi cominciano a supportare le stesse funzioni dei processori vecchi: nasce l'idea di ***famiglia di computer***.
nascono i primi computer in grado di emularne altri, nasce la **multiprogrammazione** (ovvero tenere piu programmi in memoria, per esempio mentre attendo l'input, intanto eseguo qualcosa).
### quarta generazione (VLSI)
very large scale integration (VLSI), si possono inserire milioni di transistor in un singolo chip.
nascono le prime cpu intel (8080/8088/80386), e grazie all'abbattimento dei costi: i personal computer (computer casalinghi)
* apple 1 e 2
* pc ibm: vendite spropositate rispetto ai competitor
nascono i computer portatili e si sperimenta l'architettura RISC.  
### quinta generazione
computer integrati in elettrodomestici, orologi, carte di credito ecc... a basso consumo
* grid pad
* apple newton
* PDA

### legge di Moore
il numero di transistor raddoppia ogni 18 mesi ($60\% \text{ annuo}$), in modo costante. a un certo punto ci si e' arrestati per problemi di natura energetica e dissipazione del calore.

### server e clusters
* server: computer potenti
* cluster: insieme di server connessi a una rete

### mainframe
* venivano usati in sistemi bancari, prestazioni incredibili, sicurezza elevata e costi elevatissimi

### storia di intel (non da studiare)
i processori nuovi sono in grado di emulare le funzioni dei vecchi processori, o le integrano.  
la velocità di clock di un processore non e' indice della velocità del processore! apparentemente potrebbe essere cosi' ma e' errato.  
esistono architetture che hanno un basso clock ma emettono dati alla stessa velocità di un'altra architettura che ha un alto clock.  
a cosa serve la memoria dentro al chip? ci si riferisce alla **cache**, memoria vicina alla cpu che contiene gli elementi che a cui viene effettuato l'accesso con piu frequenza (e' molto piu' veloce della memoria centrale)  
nel corso della storia intel aggiunse le istruzioni **MMX** (MultiMedia eXtensions) per processare audio e video piu' efficientemente.

* con il Pentium Pro viene introdotta la cache a 2 livelli:
	* 2 memorie da 8kb per memorizzare le istruzioni piu frequenti
	* una memoria secondaria da 256kb

* con il Pentium II abbiamo sia cache a 2 livelli che MMX.  
* con il Pentium III vengono aggiunte le istruzioni SSE (Streaming SIMD Extensions)
* con il Pentium 4 nasce l'**hypertreading**. permette di dividere il lavoro in due flussi di controllo da eseguire in parallelo.

> [!note] OSS.
> i processori Xeon e Celeron hanno architetture simili a Pentium

* Pentium M (mobile): per computer portatili, efficienza energetica e supporto allo standard wifi

>[!note] OSS. 
>questi processori sono retrocompatibili!

> [!note] OSS.
> con l'aumentare del numero di transistor aumenta il consumo elettrico e insieme all'aumento della frequenza di clock aumenta il calore da dissipare

## arm
nasce dalla acorn, che si ispira al progetto RISC, da qui il nome  Acorn RISC Machine (ARM), in seguito Advanced RISC Machine.  
collaborano con apple e mettono il loro processore nell'Apple Newton.  
successivamente collbaorano con Digital Equipment Corporation per creare una versione di ARM piu' efficiente per dispositivi mobili realizzando StrongARM (1 Watt) con due cache da 16kb per immagazzinare istruzioni e dati.
nasce poi ARM7 (1994 ???) utilizzato ancora oggi

## avr (extra)
utilizzata in arduino, sono dei microcontrollori, si dividono in 3 classi:
![[Pasted image 20240305221926.png]]
abbiamo tre tipi di memoria:
* FLASH: programmabile con un interfaccia esterna, dove sono memorizzati i dati del programma
* RAM Flash: tipo non volatile, mantiene al suo interno i dati anche quando il sistema e' spento
* EEPROM: come la RAM Flash non e' volatile 
* RAM: memoria per contenere le variabili
i microcontrollori sono costruiti in modo da essere economici e allo stesso tempo contenere tante features (orologi, timer, interfacce seriali, oscillatori...)

### unita' metriche
le principali unita' metriche sono multipli e potenze di 10, quando ci si riferisce ai dati si utilizza la base 2.

# problemi
## 1. si spieghino a parole i termini: traduttore, interprete e macchina virtuale
Un traduttore e' un programma che si occupa di trasformare per esempio un linguaggio proprio del livello L3 in linguaggio per un livello sottostante, per esempio L2. il programma trasformato e un programma fatto e finito, quindi si puo scartare il codice originale del programma. un esempio di traduttore e' un compilatore per c/c++.
L'interprete invece e' un programma che esegue per esempio in L2 ed esegue codice L3

## 2. qual'e' la differenza tra interpretazione e traduzione
l'interprete al contrario della traduzione non restituisce una traduzione completa del codice ma piuttosto, prende una istruzione in L1 e la traduce in L0 sul momento. (es. python, microprogramma)

## 3. ha significato che un compilatore generi output per il livello di micro architettura invece che per il livello ISA? pro e contro di tale ipotesi
??? 

## 4. e' possibile immaginare un computer multi livello in cui i dispositivi e i livelli logico digitali non siano livelli bassi?
a livello teorico e' possibile. scegliamo un linguaggio L0 e un linguaggio L1 che viene eseguito direttamente su un hardware generico, e ipotizziamo che questo hardware sia costruito in modo da interpretare il linguaggio L1 in L0. a livello pratico "dovrebbe" possible costruire una macchina del genere ma non sarebbe in grado di eseguire il codice L0 perche' non esiste un hardware che esegue il programma L0