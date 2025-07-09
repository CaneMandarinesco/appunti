Il Layer di Rete si occupa di calcolare e di effettuare l'inoltro dei pacchetti ricevuti dal livello di trasporto, fino a destinazione.
Il Layer opera sul piano dei dati: inoltro del pacchetto nel router attraverso l'uscita corretta e piano di controllo: calcolo delle tabelle di inoltro di ogni router.
La qualita del servizio di IP e' limitata, ma sul lungo termine IP e' il protocollo piu usato, anche se altri promettevano una QoS migliore.

Questi due piani vengono implementati oggi grazie alla SDN: Software Defined Network, un'approccio dove il piano di controllo viene attuato da un'unico controllore, che installa le tabelle di inoltro calcolate nei router della rete.
Si tratta di un'approccio centralizzato, semplice da implementare.

Uno degli attori principali del layer di rete e' il router, questo e' costituito da:
* porte in ingresso
* porte in uscita
* struttura di commutazione che collega in modo arbitrario input con output, lavora nell'ordine dei nanosecondi, attua il piano dei dati
* processore, lavora nell'ordine dei millisecondi, attua il piano di controllo

Le porte in ingresso invece sono costituite da: 
* terminale in ingresso: livello fisico
* struttura di eleborazione del segnale: livello di collegamento
* **buffer**: su cui posso cercare i pacchetti in arrivo

Ora come faccio ad inoltrare i pacchetti?
* inoltro generalizzato: grazie a strumenti come OpenFlow posso scegliere dove reindirizzare un pacchetto in base a una serie di controlli sui campi del pacchetto
* inoltro su destinazione: decido dove inoltrare guardando solo ip destinazione del pacchetto IP

Comunque sia prima di inoltrare un pacchetto lo devo analizzare! E lo devo fare velocemente per evitare code sui buffer di entrata! Oppure se sono troppo veloce, o ho troppi pacchetti in input potrei riempire i buffer in uscita Head Of Line blocking,.

L'inoltro su destinazione e' implementato grazie alle TCAM: memorie che possono immagazzinare blocchi di indirizzi (rappresentati con notazione CIDR) associati ad una porta di uscita. La TCAM e' interpellabile in parallelo per ottenere in breve tempo la corrispondenza migliore grazie ad un priority encoder e ad un decoder del segnale (accendo la porta decodificata).

La commutazione dei pacchetti avviene in 3 modi:
* memoria: input copiato in memoria e poi riprelevato
* bus: un bus comune su cui e' possibile scrivere e leggere
* rete di commutatori: le porte di input e output sono collegate mediante una rete di commutatori, in modo da favori comunicazione parallela. Talvolta si usano in paralello piu reti di commutatori.

Nel router avvengono frequentemente i seguenti problemi:
* Head of Line Blocking: il buffer in uscita e' pieno
Ossia bisogna scegliere cosa succede: quando arriva un pacchetto chi scarto per fare posto? E bisogna scegliere una scheduling policy per capire chi trasmettere per primo sulla rete.

Di solito si applica come Drop Policy: Tail Drop oppure RED (ossia marco i pacchetti per segnalare che il router e' congestionato e li scarto)

Invece per lo scheduling si puo' usare Round Robin a classi, ossia ciclo su ogni classe di priorita dei pacchetti e su ognuna di queste eseguo un'algoritmo di scheduling (FIFO), oppure eseguo Weighted Fair Queuing. Ad ogni pacchetto viene assegnato un peso che dipende dalla classe a cui appartiene, e lo scheduling viene fatto in round robin sulle classi dando priorita alla classe piu pesante rispetto alle altre. 


