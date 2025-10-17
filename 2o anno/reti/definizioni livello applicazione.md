**DNS**: protocollo per la risoluzione dei nomi. E' un gerarchia di server distribuiti gestiti da piu aziende.

**Datagramma DNS**

| 16-bit                    | 16-bit                   |
| ------------------------- | ------------------------ |
| ID                        | flag                     |
| Numero di domande         | Numero di RR             |
| Numero di RR autoritativi | Numero di RR addizionali |
| Sezione delle             | domande                  |
| Sezione dei               | RR                       |
| Sezione dei               | RR autoritativi          |
| Sezione dei               | RR addizionali           |
Con i flag posso sapere se: il server che risponde e' autoritativo, richiede la ricorsione

**CDN**: Database distribuiti geograficamente che contengono in genere file multimediali destinati allo streaming. La distribuzione geografica permette di ridurre il carico di traffico nel nucleo della rete preferendo una dislocazione presso gli `IPX` o in prossimità del nucleo della rete.

**VBR**: formato video a bit rate variabile. Consiste nello sfruttare la ridondanza tra frame o nella singola immagine mediante algoritmi di compressione. Riduce di molto il traffico in caso di porzioni di video con fotogrammi simili tra loro.

**Jitter**: descrive la variazione del ritardo, se alto e' problematico per lo streaming `UDP`

**Streaming UDP**: il client non ha bisogno di un grande buffer, se la connessione e' efficiente e supporta il VBR del video in questione allora ho una riproduzione fluida, ricordando pero' che con `UDP` potrei usare in modo egoistico il nucleo della rete. Inefficiente in caso di jitter alto.

**Streaming HTTP**:  utilizzo in questo caso il trasporto con `TCP`, piu' equo e quindi uso meno banda e ho molto overhead. Conviene avere un buffer utente piu grande in grado di accogliere piu secondi di video per avere uno streaming fluido.

**Streaming DASH**: il video e' diviso in versioni in base al bit rate richiesto ed e' diviso in chunk. Tutte queste informazioni sono raccolte nel file manifest del video da riprodurre in modo che il cliente possa scegliere da chi scaricare i chunk e con quale bit rate.

