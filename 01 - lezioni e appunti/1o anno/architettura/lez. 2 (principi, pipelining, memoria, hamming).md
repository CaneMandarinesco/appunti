 ## processore
e' il cervello del computer e contiene:
* unita' di controllo (**CU**), preleva dati dalla memoria e ne analizza il tipo
* unita aritmetica e logica (**ALU**), esegue operazioni come l'addizione e l' `AND`
* registri (memorie piccolissime ad altissima velocita')
i registri più importanti sono:
* **program counter** (PC): dove si trova la prossima istruzione *da prelevare*
* **instruction register** (IR): mantiene l'istruzione corrente in fase di esecuzione
le componenti di un computer sono collegati grazie al **bus**: una collezione di cavi paralleli usati per trasferire indirizzi, dati e segnali di controllo

### organizzazione della CPU
una CPU di *von Neumann* contiene:
* il **datapath** (percorso dati) costituito da 1 a 32 registri
* ALU; ha due registri di input: `A` e `B`, ed uno in output che deve essere poi spostato in un registro della cpu
* bus di collegamento (con immagine)

le operazioni di una cpu possono essere del tipo: registro-registro o registro-memoria\overline {A \cdot B}

esempio di flusso di lavoro di una ALU (workflow)
![[Pasted image 20240312180210.png]]

### esecuzione delle istruzioni 
il ciclo **fetch-decode-execute** e' fondamentale per l'esecuzione delle istruzioni, e' costituito dai seguenti passaggi:
1. **preleva** l'istruzione dalla memoria e mettila nell'IR.
2. **aggiorna** il program counter
3. decodifica l'istruzione in IR
4. se l'istruzione referenzia una parola in memoria, la ricerca (???)
5. (se necessario) preleva la parola dalla memoria e mettila nel registro
6. esegui
7. ritorna a 1.
### strategie di progettazione
#### extra: interpretazione e architettura
IBM introduce il termine architettura per descrivere la **compatibilità** tra vari processori appartenenti alla stessa famiglia (per venire in contro ai clienti). quindi una famiglia di calcolatori dovrebbero condividere la stessa architettura in modo che diversi processori (con prezzi diversi) possano essere in grado di eseguire tutti lo **stesso codice**.

> ciò e' possibile grazie all'**interpretazione**.

grazie all'interpretazione e' possibile quindi aggiornare/aggiustare le funzionalità di un processore senza doverlo sostituire, o addirittura ritirare dal commercio.  l'uso dell'interprete abbassava i prezzi.

---

#### architettura CISC
in **CISC** (Complex Instruction Set Computer) la CPU e' in grado di comprendere molte istruzioni complesse nativamente, e lente! (livello di astrazione molto alto)
#### architettura RISC
nasce dal tentativo di  progettare cpu VLSI **senza interpretazione**, e sono progettati senza doverli rendere retro-compatibili.  come risultato abbiamo poche istruzioni disponibili (circa 50, contro le 200 dei competitor CISC).
quindi abbiamo l'architettura **RISC** (Reduced Instruction Set Computer), poche istruzioni, semplici e veloci. (e' necessario solo un ciclo nel datapath ???)

nasce un dibattito tra CISC (intel, IBM) e RISC, dato che **una** istruzione $X$ in CISC poteva essere *emulata* da $n$ istruzioni $Y$ in RISC riuscendo cosi ad eguagliare il tempo della istruzione $X$ o addirittura a diminuirlo.   
perche' RISC non sostituisce CISC? nasce l'ibrdio tra i due:
#### ibrido (CISC e RISC)
dal *x486* le CPU (quindi compreso Intel) contengono un sottoinsieme delle istruzioni RISC piu' comuni che possono essere eseguite con un solo ciclo nel datapath, mentre quelle piu' complesse vengono interpretate dall'architettura CISC.
### principi di progettazione
* le istruzioni devono essere semplici da decodificare (regolari, lunghezza predefinita, numero ridotto di variabili e campi).  
* le istruzioni `LOAD` e `STORE` sono le uniche che devono fare riferimento alla **memoria** (tempi di accesso elevati), le altre devono operare sui **registri**
* le CPU dovrebbero contenere un *numero elevato di registri* (sempre a causa della lentezza della memoria), cosi dopo il fetch di una word, questa puo' essere tenuta in un registro
* le istruzioni devono essere eseguite direttamente sull'hardware
* massimizzare la frequenza di emissione di istruzioni
### parallelismo a livello del processore
dato che non si può aumentare il clock oltre un **certo limite** dovuto alle leggi della fisica, i progettisti guardano al **parallelismo** per incrementare le performance, con due tipi di parallelismo:
* al livello di istruzione (un processore gestisce piu flussi di lavoro contemporaneamente)
* a livello di processore (piu' processori insieme)
### pipelining (prefetched buffer)
l'operazione di `FETCH` (recupero dati dalla memoria) e' lenta, per ridurre questo problema sono stati introdotti i **prefetched buffer** precaricati con l'istruzione già pronta per essere letta, non bisogna aspettare il completamento della lettura dalla memoria.
grazie al pipeline l'esecuzione delle istruzioni e' divisa in **stage** o fasi in modo da eseguire queste fasi in parallelo su hardware dedicato.
esempio di pipelining a 5 stage:
1. fetch dalla memoria e memorizzazione nel buffer di prefetch
2. decodifica
3. rintraccia gli operandi
4. esegui
5. metti i risultati nei registri (compreso lo stato del processore)

senza aspettare che il ciclo di stage finisca, se il primo stage non sta facendo niente gli posso dare gia' un'altra istruzione!
### banda del processore
il pipelining permette di bilanciare la **latenza** (durata esecuzione di una istruzione) con la **banda del processore** (quanti MIPS/*Millions of Instructions Per Second* la CPU emette).  
![[Pasted image 20240316221600.png]]

> una CPU senza pipeline che opera ad un clock $T \text{ ns}$ ed emette una istruzione per ciclo ha una banda di $10^3/ \text{T MIPS}$

ora supponendo di avere a disposizione di una CPU con:
* clock pari a $T \text{ ns}$
* una pipeline a **p** stadi in cui ogni stadio viene eseguito in un ciclo di clock
la ***latenza totale*** e':
$$
p \cdot T
$$
mentre la ***banda*** e':
$$
p \cdot 10^3 \text{/ MIPS}
$$
### architetture superscalari
se una pipeline funziona allora possiamo metterne di piu', come nell' **intel x486** ma,tutta via:
* non tutte le istruzioni possono essere svolte in parallelo 
* e' necessario troppo hardware per le varie unita
![[Pasted image 20240312184501.png]]

si adotta un approccio diverso: il processore dispone di una pipeline con piu' unita' funzionali in alcuni stadi di essa.  
![[Pasted image 20240312184807.png]]

per far si che questo tipo di implementazione abbia senso, la velocità in emissione di `S3` deve essere piu' alta dello stadio `S4`. in `S4` possiamo trovare piu' ALU duplicate.

--- 
### parallelismo a livello di cpu
possiamo ottenere un fattore di miglioramento che va da 5 a 10. se vogliamo incrementare drasticamente la performance del sistema possiamo arrivare ad un incremento di 50, 100 o piu. abbiamo 3 differenti approcci:
* computer con parallelismo sui dati / array computer
* multiprocessori
* multicomputer

#### classificazione flynn
| flusso di istruzioni | flusso di dati | nome                  | esempio                         |
| -------------------- | -------------- | --------------------- | ------------------------------- |
| Singolo              | Singolo        | SISD / array computer | modello Von Neumann             |
| Singolo              | Multiplo       | SIMD                  | Supercomputer vettoriali        |
| Multiplo             | Singolo        | MISD                  | non sono note                   |
| Multiplo             | Multiplo       | MIMD                  | multiprocessori e multicomputer |

### computer con parallelismo sui dati
il parallelismo sui dati e' il metodo piu efficiente per ottenere piu potenza dal processore.  quando si parla di parallelismo sui dati abbiamo tanti processore che eseguono la stessa istruzione ma su dati diversi. c'e' bisogno solo di un processore che si occupi di fare il fetch, decode e la logica di controllo
#### processori SIMD (Single Instruction-stream Multiple Data-stream) / array computer.  
i processori SIMD sono costituiti da un vasto numero di processori identici, che eseguono la stessa sequenza di istruzioni su dati differenti, esempio: GPU.  
> vengono usati tanti sommatori quanti sono gli elementi del vettore
#### processori vettoriali
esegue la stessa sequenza di operazioni su coppie di dati ma le addizioni sono svolte da un unico sommatore nella pipeline.  
> viene usato un solo sommatore e un unico **registro vettoriale** (dove i dati possono essere caricati da un unica istruzione)

il risultato di queste operazioni e' un vettore che viene immagazzinato in un **registro vettoriale**.  

> entrambe le architetture lavorano su array di dati, per il programmatore non c'e' differenza.

### multiprocessori (memoria sincronizzata)
architettura costituita da piu' CPU che condividono una **memoria comune**: ***thighly coupled***
dato che ciascuna CPU puo' *leggere e scrivere in qualsiasi zona della memoria comune*, devo essere **sincronizzate** via software, quindi devono interagire in modo profondo tra di loro.  

### multicomputer (memoria privata)
dato che multiprocessori con più di 256 CPU sono difficili da costruire si e' preferito abbandonare la memoria comune e usando CPU interconnesse con ognuna una **memoria privata**.  
![[Pasted image 20240312190936.png]]
quindi in un multi-computer le CPU:
* sono accoppiate in modo lasco (**loosely**)
* comunicano attraverso messaggi
anche se in architetture piu grandi la completa interconnessione non e' fattibile, quindi si usano metodi che sfruttano la **topologia** (griglia, anelli, albero)

# memoria principale
memorizza i programmi e i dati. l'unita piu' piccola di memoria e' il bit (BInary digiT).  metodo affidabile per codificare l'informazione digitale, inoltre il formato binario rispetto a quello decimale e' piu vanntaggioso, con 16 bit possiamo ottenere 65.536 combinazioni in binario rispetto alle 10.000 del decimale (dato che in decimale ogni coppia di 4 bit rappresenta una cifra significativa: unita, decine, centinaia, ecc...)

## sistemi di numerazione
e' un sistema linguistico fatto di numeri ed e' composto di alfabeto e grammatica.  
* romano
* posizionale: il numero di simboli usati indica la base, e il valore di ogni simbolo dipende dalla base
* basi importanti: `2,8,10,16`
in base `16` si usano le  lettere `A...F` perché la macchina da scrivere all'epoca disponeva di questi simboli (la scrittura in base 16 nasce prima del computer!).  
metodi per trasformare le basi:
* per trasformare la base si usa il metodo delle **divisioni successive**.  
* metodo semplificato: **base 10 -> base 2** (nel metodo basta chiederci: il numero e' pari o dispari?).  
* metodo semplificato: **basi potenza l'una dell'altra** (es. 2 e 8)

> 1 byte = 8 bit
## indirizzi di memoria
una memoria e' organizzata in **celle** (parole/word, `8-bit` nello standard), dove ogni cella contiene lo stesso quantitativo di informazioni in byte, e rappresenta quindi l'unita' di memoria piu piccola indirizzabile, i byte possono essere raggruppati in **parole**.
ordinamento dei byte nelle parola:
* **Big Endian**: da sinistra a destra
* **Little Endian**: da destra a sinistra
nella conversione di stringhe misti a interi ci possono essere vari problemi nell'interpretazione delle cifre: 
![[Pasted image 20240312192726.png]]

modi per organizzare una memoria a 96 bit.
![[Pasted image 20240312192211.png]]

$$
2^n
$$
e' il numero di celle massime indirizzabili in un sistema con indirizzi composti da $n$ bit.

## codici di correzione errore (hamming)
in una memoria potrebbero verificarsi dei picchi anomali di corrente che potrebbero alterare il valore di una memoria, invalidandola. per risolvere il problema e' stato introdotto il **codice di hamming**.  
### codice di hamming
quando voglio vedere il numero di bit per cui due numeri binari differiscono applico l'operatore `XOR`. (quanti bit valgono 1?) = **distanza di hamming**.
per rendere il codice di hamming funzionante la distanza tra due word deve essere di $k+1$ bit.

### applicazione del codice hamming
* prendo una parola e inserisco al suo interno dei bit di controllo: $\text{bit\_dati} + \text{bit\_controllo}+ 1 <= 2^{\text{bit\_controllo}}$

per esempio posso avere una parola di 7 bit con 4  bit di dati e 3 di controllo:
$$
4 + 3 + 1 \leq 2^3
$$

* posiziono i bit di controllo usando come indici le potenze di 2
* ora ogni bit di controllo posizionato, controlla i bit in posizione $n$, tale che $n$ ha un 1 nella posizione del bit pari al numero di ordine in cui il bit di controllo compare nella parola. per esempio il bit di controllo in 1a posizione (che esiste sempre), controlla tutti i valori che sono in un indice dispari! (xxx1, un numero di questo tipo e' sempre dispari)
* vado a eseguire la parità dei bit rispetto ad ogni bit di controllo -> devo modificare il bit di controllo in modo che il `bit` concatenato al `bit_dati` sia pari

ora abbiamo calcolato il bit compreso di codice hamming e possiamo spedirlo! e immaginiamo che avvenga un errore!  
se avviene un errore vuol dire che almeno un bit di controllo e' sballato se li calcoliamo.  

> per scoprire qual'e' il bit sbagliato basta sommare le posizioni dei bit di parita' sbagliati.
## memorie cache
#### extra
memoria all'interno della cpu, velocita' prossima a quella dei registri. contiene le parole piu' usate di frequenti, per esempio se accedo ad un indirizzo $A$ e' probabile che il prossimo indirizzo a cui devo accedere si trovi nelle vicinanze di $A$! (per esempio se sto eseguendo un programma la memoria in cui esso risiede e' sicuramente contigua). quindi abbiamo che se accediamo $k$ volte ad una parole, il 1o accesso lo facciamo sulla memoria lenta, e le restanti $k-1$ volte sulla memoria veloce.  

le memorie centrali e le cache sono divise in blocchi: **linea di cache**, quando si verifica un fallimento della cache, l'intera linea viene ricaricata dalla memoria centrale.  
> conviene prelevare $k$ parole tutte insieme che piuttosto $k$ parole in momenti diversi.

ci sono vari modi per progettare una cache:
* come deve essere divisa? (16-bit, 8-bit, n-bit?)
* piu' grande e' e piu' costa
* come deve essere organizzata?
* unificata o divisa in due (istruzioni/dati separati)(architettura **Harvard**)? 

---
## banco di memoria
* chip di memoria
* chip di controllo

