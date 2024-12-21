Il disco e' una lista di blocchi, posso leggere/scrivere il blocco $k$. 
Il file system e' un modo per organizzare e memorizzare informazioni, fornisce un'astrazione, esempi: FAT, NTFS, Ext4, Btrfs, ecc... Ogni formattazione e' un driver che permette di leggere il formato di archiviazione, grazie alle chiamate `open`, `read`, `write` posso accedere a questi driver.

Caratteristiche dei file:
* hanno un nome, il sistema operativo crea un collegamento tra descrittore e nome (???). In base al file system ci sono dei caratteri vietati. Alcuni file system sono **case sensitive**.
* estensione: indicano le caratteristiche, proprietà del file, che significato dare alla sequenza di codice binario.

Struttura di un file, due metodi:
* **sequenza non strutturata di byte**, i file sono una sequenza di byte. I programmatori del livello utente hanno libertà su come interpretare i file.
* **sequenza di record di lunghezza fissa**.

Tipi di file:
* File e Directory normali: le directory permettono di strutturare i file, i file contengono informazioni degli utenti
* File Speciali: 
	* **a caratteri**: sono usati per accedere a dispositivi di **I/O**
	* **a blocchi**: per modellare i dischi.
* File Normali:
	* ASCII: contiene testo
	* Binari: non leggibili come testo.

`/dev/sda` contiene i dischi come sequenza di blocchi. Un programma deve capire come vanno letti i blocchi, si usa `mount`.

La struttura di un file eseguibile viene creata dal compilatore, questo contiene:
* Intestazione
* Testo
* Dati

il comando `file` permette di capire che file stai aprendo.

Ogni file ha degli attributi:
* Protezione e accesso
* Flag
* ...

### operazioni sui file
* `create`
* `delete`
* `open`
* `close`
* `read`

#### write
* `append`
* `seek`
* `Get/Set Attributes`


### aprire e leggere un file
```c
int fd = open("foo.txt", O_RDONLY);
char buf[512];
ssize_t bytes_read = read(fd, buf, 512);
close(fd);
printf("read %zd: %s\n", bytes_read, buf);
```

