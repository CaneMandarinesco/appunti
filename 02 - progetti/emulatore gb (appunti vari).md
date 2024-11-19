* https://gbdev.io/pandocs/Power_Up_Sequence
* [[https://gbdev.gg8.se/wiki/articles/OAM_DMA_tutorial]]
* tool immagini: http://chrisantonellis.github.io/gbtdg/
* https://github.com/tbsp/simple-gb-asm-examples/tree/master/src
* http://gameboy.mongenel.com/dmg/asmmemmap.html

## memory map
* 8000 - 9FFF: VRAM (Video RAM)
* C000 - CFFF: 4Kib WRAM (Work RAM)
* D000 - DFFF: 4Kib WRAM
* E000 - FDFF: **echo**(duplicata) ram (C000-CFFF)
* FE00 - FE9F: object attribute memory
* FF80-FFFE: high ram

la VRAM puo' essere usata come normale RAM.

## grafica

### VBLANK
e' importante non modificare la VRAM del gameboy durante il periodo di V-BLANKING, cio' causa la comparsa di una riga sullo schermo e di danni allo schermo su alcuni modelli

### tile e tilemap
manipolare la grafica pixel per pixel sarebbe costoso, invece la cpu gestisce gruppi di pixel chiamati tiles (8x8). e un tile e' in grado di assegnare dei colori ai sui pixel (4 colori).  
una palette consiste in un array di colori (4 nel gameboy)  
il gameboy ha 3 layer: background, window e objects con la possibilità di andare oltre questa astrazione.  
il background e' composto da una tilemap (griglia di riferimenti ai tiles), si puo' scrollare il background usando due registri.  
il window e' simile al background ma piu' limitato.

ogni tile ha 2 bit di color depth: quindi 4 possibili valori che identificano 4 diverse tonalita' di grigio. il colore `0` e' trasparente!

nella VRAM esistono 3 blocchi di ram di 128 oggetti:
* 8000 - 87FF blocco 1
* 8800 - 8FFF blocco 2
* 9000 - 97FF (inutilizabile)
ci sono 2 modi per indirizzare i blocchi in base all'indirizzo di riferimento (8000/9000). varia in base al valore di `LCDC bit 4`.
ogni tileset occupa 16 bytes e una linea e' formata da 2 bytes
## programmazione!
### boot rom e header
controlla che il logo nintendo nella rom sia uguale a quello immagazzinato dentro la console (misura antipirata che pero' e' stata deprecata). viene calcolata una checksum dell'header per controllare che non sia corrotta. 
l'header contiene la grandezza della rom. tutte queste informazioni devono essere calcolate correttamente altrimenti la rom non funziona, per farlo si usa `rgbfix` per trasformare la rom compilata in una rom usabile.

inoltre contiene:
* nome programma
* compatibilita' con game boy color
* grandezza rom
* checksum
* nintendo logo

anche se alcune di queste informazioni, anche se errate non invalidano la rom stranamente, dato che queste erano usate da Nintendo dato che l'header descrive l'hardware della cartuccia.  
ed e' proprio per questo motivo che per un emulatore e' importante analizzare l'header.

nella versione hardware del gameboy, dentro alla cpu troviamo la boot ROM che contiene le istruzioni eseguite all'accensione (compreso il logo nintendo) e controlla che le checksum e il logo nintendo nella cartuccia siano corretti.  

per scrivere correttamente queste informazioni ricordiamoci di dire all'assemblatore che la `ROM0` inizia all'indirizzo `$150` e dobbiamo riempire con degli 0 gli spazi nella memoria che `rgbfix` andra' a scrivere:
```asm
SECTION "hello", ROM0[$150]
	jp main
	ds $150-@, 0
```

## operazioni e flags
* inc
* dec

flags:

| nome | desc       |
| ---- | ---------- |
| Z    | zero flag  |
| N    | add/sub    |
| H    | half-carry |
| C    | carry      |
### compare
cp sottrae il suo operatore `x` da `a` e aggiorna le flag di conseguenza, quindi possiamo per esempio sapere quando `a >= x` o `a < x`!  

## jumps
* jump
* jump relative (relative 128 byte massimo di salto dall'indirizzo dell'istruzione corrente)
* call
* return

jump semplicemente imposta `PC` al suo argomento. guardiamo l'esempio:
```asm
    ld de, Tiles; Tiles corrisponde al primo byte dei dati sui tilesets
    ld hl, $9000; iniziamo a copiare in questo indirizzo
    ld bc, TilesEnd - Tiles; quanti dati dobbiamo copiare
CopyTiles:
    ld a, [de] ; copia de in a
    ld [hli], a 
    inc de ; vai al prossimo byte
    dec bc ; i byte da leggere decrementano di 1, dobbiamo sapere se arriviamo a 0
		    ; la flag non viene aggiornata nel caso dec bc quindi lo facciamo manualmente
    ld a, b 
    or a, c
    jp nz, CopyTiles ; se flag z e' non zero vai a CopyTiles
```

# video! e DMA
due tipi di sprite:
* 8x8
* 8x16

a ogni sprite e' associato un numero e questo tipo di informazioni si trovano nella OAM (Object Attribute Memory):
* 160 byte: ovvero 40 sprites, 4 byte per sprite.

nei 4 byte troviamo:
* coordinata X
* coordinata Y
* tile number (riferito alla tilemap)
* flags

le flag sono:

| bit pos | flag name            |
| ------- | -------------------- |
| 7       | render priority      |
| 6       | y flip               |
| 5       | x flip               |
| 4       | palette number       |
| 3       | vram bank            |
| 2       | palette number bit 3 |
| 1       | palette number bit 2 |
| 0       | palette number bit 1 |

per trasferire i dati sulla OAM bisogna usare il DMA (Direct Memory Access, a livello hardware il DMA e' un controller):
* durante un trasferimento DMA, la cpu puo' accedere solo alla HRAM (FF80-FFFE), anche solo per leggere le istruzioni successive.
* durano 160microsecondi

per iniziare un DMA abbiamo bisogno di scrivere nel registro a FF46, per esempio ipotizziamo si voler copiare da C100
```asm
; scrivo in a la parte superiore dell'indirizzo a cui voglio accedere
ld a,$C1
ld [rDMA], a

; bisogna aspettare che il trasferimento sia completato
ld a, 40
.loop:
	dec a
	jr nz, .loop
	ret
```

nota che non c'e' bisogno di immagazzinare anche 00.

l'unico problema e' che la cpu puo' leggere solo dalla HRAM durante una DMA, e noi stiamo leggendo le istruzioni dopo dalla ROM.  

```asm
SECTION "OAM DMA routine", ROM0
CopyDMARoutine:
	ld hl, DMARoutine
	ld b, DMARoutineEnd - DMARoutine
	ld c, LOW(hOAMDMA)
.copy
	ld a, [hli]
	ldh [c], a ; carica a in $FF00+C (locazione in high ram)
	inc c
	deb b
	jr nz, .copy
	ret

DMARoutine:
	ldh [rDMA], a
	ld a, 40
.wait
	dec a
	jr nz, .wait
DMARoutineEnd:
```

## registri
LCDC:

| bit pos | flag name   | desc                                    |
| ------- | ----------- | --------------------------------------- |
| 7       | LCD_ENABLED |                                         |
| 6       | WIN_MAP     |                                         |
| 5       | WIN_EN      |                                         |
| 4       | TILE_SEL    | seleziona il blocco vram a cui accedere |
| 3       | BG_MAP      |                                         |
| 2       | OBJ_SIZE    |                                         |
| 1       | OBJ_EN      |                                         |
| 0       | BG_EN       |                                         |

## memory map
la memoria video si trova in $8000-97FF divisa in 3 blocchi da 128 sprite:
* blocco 0: $8000-87FF
* blocco 1: $8800-8FFF
* blocco 2: $9000-97FF
per accedere a questi indirizzi di solito si somma un valore al primo indirizzo che corrisponde al blocco a cui voglio accedere:
* 8000
* 9000
> gli sprite usano l'indirizzamento con base 8000, invece background e windows possono usare entrambi i modi

> ogni tile occupa 2 bytes per linea, e posso avere sprite da 4 linee o 8.

### background map
* 9800-9BFF
* 9C00-9FFF

## ancora appunti piu tecnici
* character:
	* 128 obj
	* 128 bg
	* 128 character possono essere sia obj che bg
* bank 0 e 1: character data (8000-97FF)

character data: risiede tra 8000 e 97FF e posso seleziona quale usare per bg e window character data.

# interrupts
segnale che interrompe la CPU, e' costretta a eseguire delle istruzioni per gestire gli interrupt.  possono essere abilitati e disabilitati e possono essere usati per esempio per tenere traccia del tempo di esecuzione.  

* `ei`: enable interrupts
* `di`: disable interrupts
* `reti`: return and enable interrupts