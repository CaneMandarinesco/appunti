### BIOS con MBR
Il **BIOS** si trova nel settore 0 dell'MBR. Controlla che dispositivi sono installati, se ci sono errori e varie operazioni di manutenzione. Poi fa partire il sistema operativo, lo carica in RAM.

L'**MBR** sa dove si trovano i file per eseguire il boot del sistema operativo. Ha una **tabella delle partizioni**, una **mappatura del disco** e delle sue partizioni. Ogni entry della tabella indica da che settore fino a quale settore si trova ogni partizione e come e' formattata. *Le partizioni devono essere contigue*.

**In generale**, **tecnicamente**, ogni partizione del disco contiene:
* blocco di boot
* super-blocco
* gestione dello spazio libero
* i-node: specifico per file system
* Cartella radice
* File e directory

### UEFI
sostituisce il bios tradizionale, ha dei vantaggi;
* Avvio più veloce
* Compatibilità migliorata
* Puo' includere un'interfaccia grafica
* Sicurezza

UEFI funziona con GPT, contiene:
* blocco per sistemi legacy che cercano l'mbr
* Intestazione tabella partizioni: contiene piccoli programmi, come un'interfaccia grafica
* Blocco delle partizioni
* Partizioni
* Backup del blocco delle partizioni e dell'intestazione.

**Secure Boot** permette di avviare solo software certificato, utilizza del chiavi per controllare l'esecuzione del codice.

### GPT (GUID Partition Table)
Gestisce le partizioni dentro UEFI, esiste una partizione speciale: ESP (EFI System Partition) che permette di Archiviare i file di avvio, driver e utility di diagnostica e usa il file system FAT32.

### Secure Boot
hfhj

### Allocazione a liste concatenate con FAT
File Allocation Table, devo caricare la mappa in RAM. La tabella contiene la sequenza dei blocchi usata da ogni file. Vantaggi:
* l'intero blocco e' usato per i dati
* Ma usa troppa memoria: la tabella si trova in ram, non ottimale per dischi grandi. Se ho 1TB di spazio formattato in usando blocchi da 1KB, la tabella occupera 3GB.
* Infatti si usa per schede sd e chiavette usb.

### I-node: (Index Node)
Evito di caricare tutti i file. E' una struttura che contiene tutte le informazioni di un file **tranne nome e contenuto**, probabilmente se non lo leggo non mi serve saperne il nome e i suoi blocchi occupati. Il sistema I-Node permette di gestire meglio i meta-dati legati ad un file.

Un inode ha una lista di puntatori a blocchi predefinita, se me ne servono altri inserisco un puntatore ad un'altro inode. (comunque e' un unico inode)

### Directory
La **struttura delle cartelle** contiene i nomi dei file. E' un file del sistema operativo che **contiene una lista** di file dentro una determinata cartella. Ogni entry contiene: **nome** e **i-node** associato.
### `ls`
1. devo allocare pagine in memoria per eseguire  `ls`
2. devo vedere dove si trova `ls`
3. prendo l'i-node di ls
4. vado a prendere i blocchi del testo di ls
5. li butto nella pagina

### Hard/soft link
* hard link -> punto all'inode
* soft link -> punto al nome del file

Ho un contatore di link per sapere se l'inode e' usato da qualche nome, altrimenti posso cancellare il file.
