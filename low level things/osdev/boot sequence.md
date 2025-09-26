### POST
Quando un computer viene accesso si entra in un processo di diagnostica chiamato `POST` (Power On Self Test): si cerca un dispositivo avviabile e si esegue il bootloader.

### MBR
> [!note] `MBR`
> Viene caricato dal `BIOS` ed si trova sul primo settore di un hard disk. Contiene l'`MBR Bootstrap Program` e la Partition TAble.

I BIOS legacy cercano una **firma** nel settore 0 di ogni dispositivo, ossia se `0x55 0xAA` si trovano rispettivamente nei byte `510` e `511`.

Una volta trovato, il programma viene caricato a `0x0000:0x7c00` (segmento 0, indirizzo `0x7c00`). Cio' avviene per esempio in un floppy disk.

In un'hard disk di solito troviamo l'`MBR`. Successivamente si spera che la `cpu` sia in `Real Mode`, cosi posso cambiare privilegi e passare il controllo al kernel.

### Loading
Dobbiamo ora:
* determinare la partizione da avviare
* determinare dove si trova l'immagine del kernel.
* caricare il kernel in memoria
* abilitare modalità protetta
* imposta uno stack base e prepara la cpu per l'esecuzione di codice in ambiente protetto.
* chiamata a `kmain()`! Punto di entrata nel kernel.

> **Nota**: `GCC` non permette di produrre codice in `real mode`.

