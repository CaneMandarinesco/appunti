## pentium 4 (sul libro non c'e', non bisogna sapere tutto)
discende dall'8080, arriva fino a 3,2ghz, scambia dati a 64-bit, processore per il passaggio da 32 a 64 bit.  
* 55 milioni di transistor (generalmente non e' importante conoscere il numero di transistor)
* 3,2 GHz frequenza di clock
* profondita' della pipeline: dipende dal tipo di processore
* 2 unita' alu parallele
* puo' essere "virtualizzata" come se ci fossero due cpu fisiche
* 8kb di SRAM, cache L1
* 1MB di chache L2

> le cpu tiene le cache consistenti controllando la memoria sul bus

* due bus sincroni primari (generalmente il bus sincrono ha meno problemi)
* bus pci per i/o
* consumo tra 63W/82W

> [!note] STATO IDLE (aggiuntivo)
> quando apriamo un programma, a livello di astrazione il sistema operativo crea un processo. con il time sharing ogni processo ha il suo tempo per eseguire le sue istruzioni. se la cpu non ha niente da fare non si puo spengere, consuma troppa corrente spegnere e riaccendere la cpu, c'e' bisogno di tenerla accesa.

### pin e segnali
* 478 pin
* 85 alimentazioni
* 180 massa
* 15 liberi per usi futuri
* segnali di arbitraggio
	* a
	* ads
	* req
...
### pipeline
>[!note] Troughput
> il troughput misura il numero di operazioni in uscita

## intel core i7
...

3 livelli di cache, formalmente la cache e' una memoria aggiuntiva (rispetto ai registri) del processore:
* due cache L1 32KB
* una cache L2 256kb
* una cache L3 a 15MB

## ultrasparc III
CPU RICS prodotta da sun microsystem
* 1,2 GHz
* 50W di consumo

## microcontrollore 8051
* si riesce direttamente a connettere a (LED, switch, bottoni, display) grazie alle linee di IO
...

## esempi di bus
* pci (anni 90/2000)
* pcie: 16gb/sec
* usb (universal serial bus)

vecchi bus:
* isa: velocita di $8,33MHz$
* eisa

esisteva un bus per la gpu:
* agp (accelerated graphics port)
### usb
tecnologia standard nata nel 1998.  si usa per dispostivi lenti (come dispositivi di archiviazione, mouse e tastiera che dipendono dall'uomo).
* hub principale connesso al bus di sistema
* quando viene connessa una periferica avviene un interrupt (generato dall'hardware)
* il resto e' gestito dal sistema operativo
* bisogna controllare larghezza di banda (per fare qualcosa)
* ogni device ha un id nel bus (da 1 a 127)

l'hub effettua collegamenti punto-punto, i dati vengono inviati con i frame (pacchetti di dati con varie informazioni).  quattro tipi di frame:
* controllo
* isocroni
* bulk
* interrupt

4 tipi di pacchetto (contenuto dentro al frame):
* token
* dati
* handshake
* speciali

> [!note] overhead
> tutto cio' (dati in uscita) che e' a supporto di un operazione che sto facendo ma non e' strettamente necessario.

> [!note] incapsulamento
> i dati vengono impacchettati con l'overhead

### interfacce di i/o
tipi:
* UART (universal async receiver trasmitter)
* USART (Universal sync async receiver transm)
* PIO (parallel i/o)

## intel 8255A (esercizio pagina 73 libro simo)
ha 3 porte I/O:
* A
* B
* C

l'ingresso per l'abilitazione del chip $\overline {CS}$ e' usato per collegare piu PIO (Parallel Input Output) in parallelo.

con $A_{0}, A_{1}$: abilito gli indirizzi.  

### decodifica indirizzi
con 16 linee di indirizzamento posso indirizzare: $2^{16} = 64KB$

