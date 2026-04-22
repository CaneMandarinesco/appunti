### INDEX COMPRESSION
- La **index compression** serve a **ridurre lo spazio occupato dall’indice**, mantenendo però la sua capacità di funzionare correttamente(operatività)
	- Non vogliamo perdere efficienza: l’obiettivo è avere una struttura **più compatta ma ancora utilizzabile velocemente**.
- Possiamo distinguere due tipi di compressione:
    - **Lossless**
        - Non si perde informazione
        - Dopo decompressione otteniamo esattamente i dati originali
        - È quella usata principalmente in Information Retrieval
    - **Lossy**
        - Si perde parte dell’informazione
        - Può influenzare la qualità dei risultati
        - Alcuni preprocessing (come stopword o stemming) possono essere visti come forme di compressione lossy
- Le tecniche di compressione devono avere un **basso costo di decompressione**
- Questo perché:
    - i dati vengono decompressi frequentemente durante le query
    - se la decompressione fosse lenta, annullerebbe i vantaggi della compressione
###### VANTAGGI DI COSA TOGLIERE NELLA TABELLA E COSA FARE
![[Pasted image 20260320170023.png]]
- questa tabella mostra **come cambia la dimensione dell’indice** a seconda del preprocessing applicato
	- il cambiamento viene misurato separatamente su:
		- **dizionario** = numero di termini distinti
		- **indice non posizionale** = numero di postings
		- **indice posizionale** = numero di posizioni da memorizzare
##### Effetti delle tecniche di preprocessing
- Le diverse tecniche di preprocessing non hanno lo stesso effetto su tutte le componenti dell’indice: la loro convenienza dipende da cosa stiamo analizzando (dizionario, postings non posizionali, postings posizionali).
- In particolare:
    - **Rimozione delle stopword**
	    - elimina tutte quelle che sono le stopword
        - è **molto efficace** nel ridurre la dimensione delle postings lists (sia non posizionali che posizionali), perché le stopword sono estremamente frequenti
        - è invece **poco utile sul dizionario**, perché il numero di termini distinti coinvolti è molto piccolo
    - **Case folding (rimozione delle maiuscole)**
	    - operazione che mira a togliere le maiuscole
        - è **molto efficace sul dizionario**, perché unifica parole come _Apple_ e _apple_
        - ha invece **effetto limitato sui postings**, e può non cambiare affatto il numero di posizioni
    - **Eliminazione dei numeri**
        - ha un **impatto ridotto sul dizionario**
        - ha un effetto **moderato sulle postings**, ma non è tra le tecniche più incisive
    - **Stemming**
	    - operazione che riduce le parole a una forma comune, eliminando suffissi e variazioni morfologiche
	        - è **molto efficace nel ridurre il numero di termini distinti** (dizionario)
	        - ha un effetto **più contenuto sulle postings**
	        - inoltre introduce una perdita di informazione, perché le parole vengono ridotte a forme non sempre linguisticamente corrette
        - per lingue come l'italiano è consigliata la lemmatizzazione, rispetto allo stemming è più complessa ma efficace
- cosa si intende per $\Delta \%$
	- **Δ%** indica la variazione percentuale rispetto alla riga precedente della tabella
	- **T%/cumul %** indica la riduzione cumulativa rispetto al caso iniziale “unfiltered”
- RICORDA: non ci sono regole generali ogni testo ha la sua particolarità questi dati possono variare in base a cosa hai davanti
	- un tweet avrà una riduzione praticamente solo sulle stopword, il resto di solito non si ripete
- **questi preprocessing** possono essere visti come forme di compressione **lossy**, perché eliminano o modificano informazione
### LEGGE DI HEAPS
- La legge di Heaps descrive come *cresce* il numero di *termini distinti* (dizionario) al crescere del numero di token in una collezione
- *formula* $M=kT^b$
	- $M$ è il numero di termini distinti
	- $T$ è il numero totale di token
	- $k$ è una costante (tipicamente tra 30 e 100)
	- $b$ è circa 0.5
Questa stima è *utile* perché consente di:
- **prevedere la dimensione dell’indice**
- **stimare** **l’altezza** delle strutture di ricerca (albero) (circa $log⁡M$) 
- funziona bene per **collezioni grandi (T sufficientemente grande)**
	- NON è esatta
	- ma è **molto accurata**
	- es con $1,000,020$ token
		- la legge predice $38,323$ termini
		- in realtà sono $38,365$ 
![[Pasted image 20260320172623.png|400]]
- il log viene usato principalmente per rendere il grafico lineare e quindi più leggibile e confrontabile con i dati reali
	- qui abbiamo la retta teorica(quella tratteggiata)
	- e la retta data dai valori effettivi dove ogni punto è una coppia `(M,T)`
		- dove M sono i termini distinti e T i token totali
### LEGGE DI Zipf's
- La legge di Zipf's descrive **come sono distribuite le frequenze dei termini** all’interno di una collezione di documenti
- NON serve a stimare quando riappare un termine, ma a capire **quanto spesso compaiono i termini rispetto agli altri**
- Se ordiniamo le parole per frequenza decrescente:
    - la parola in posizione $i$ (cioè la i-esima più frequente) avrà una frequenza **inversamente proporzionale a $i$**
    - dove:
        - $i = 1$ → parola più frequente
        - $i = 2$ → seconda più frequente
        - ecc…
- In formula:
    $cf_i \approx \frac{K}{i}$
    **cf = collection frequency**
    K costante di normalizzazione
    di solito $K ≈$ frequenza del termine più frequente
Se la parola più frequente compare un certo numero di volte:
- la seconda compare circa la metà
- la terza circa un terzo
- e così via
- Le frequenze non vengono calcolate da Zipf:
    - sono già presenti nei dati
- Zipf descrive il fatto che:
    - poche parole sono **molto frequenti**
    - moltissime parole sono **molto rare**
![[Pasted image 20260320181700.png|400]]

### TECNICHE DI COMPRESSIONE
- Assunzioni:
    - ci concentriamo sulla compressione del **dizionario**
    - non consideriamo (per ora) l’indice posizionale
- L’obiettivo è ridurre lo spazio occupato, in modo da poter mantenere il dizionario in memoria (RAM)
###### VERSIONE SENZA NULLA, NAIVE fixed-width
- ogni termine è memorizzato separatamente
- si riserva uno spazio fisso (es. 20 byte per termine)
- problema:
    - grande spreco di spazio (molte parole sono più corte)
![[Pasted image 20260320182736.png]]
###### VERSIONE DICTIONARY AS A STRING
- si concatenano tutti i termini in un’unica stringa
- per accedere ai termini:
    - si usano **puntatori (offset)** che indicano dove inizia ogni termine
	    - bit per puntatore≈$log_2​(total \ string \ length)$
    - oppure si memorizza la lunghezza del termine prima che inizi quest'ultimo
- vantaggi:
    - si elimina lo spazio inutilizzato
    - si memorizzano solo i caratteri effettivi
- il costo totale è dato da:
    - dimensione della stringa (somma delle lunghezze delle parole)
    - spazio per i puntatori
i puntatori sono importanti:
- ogni puntatore occupa memoria (es. 4 byte)
- quindi bisogna bilanciare:
    - numero di puntatori
    - spazio risparmiato sulle stringhe
se si usa una struttura ad albero:
- ogni nodo contiene un offset alla stringa
- questo permette di accedere ai termini mantenendo la struttura di ricerca
![[Pasted image 20260320182805.png]]
bit per puntatore≈$log_2​(total \ string \ length)$
#### VERSIONE DICTIONARY AS A STRING CON BLOCCHI
- Per ridurre il numero di puntatori, si raggruppano i termini in **blocchi di k parole**
- Si memorizza:
    - una stringa unica con tutte le parole
    - un puntatore per ogni blocco (non per ogni parola)
- All’interno di un blocco:
    - le parole sono salvate consecutivamente
    - ogni parola è preceduta dalla sua lunghezza, per poterla individuare
- *VANTAGGI*
	- Il numero di puntatori passa da $M$ a $M/k$
		- dove $k$ è il numero di blocchi
- *SVANTAGGI*
	- Per cercare un termine:
	    - si individua il blocco
	    - si scansionano le parole nel blocco
	- Aumenta il numero di confronti → maggiore tempo di ricerca
		- Più blocchi piccoli → più spazio, meno tempo
		- Blocchi grandi → meno spazio, più tempo
	- Il costo medio della ricerca aumenta perché è necessario esplorare sequenzialmente il blocco dopo aver trovato il puntatore

##### CON BLOCCHI VS SENZA BLOCCHI
##### IN TERMINI DI MEMORIA
###### Senza blocking:
- hai **1 puntatore per parola**
- ogni puntatore ≈ **3 byte**(prendendo una stringa lunga tipo 3,2 mil)
- per 4 parole →
    $3 \times 4 = 12 \text{ byte}$

Per ogni termine si memorizzano 4 cose:
- **4 byte** per la frequenza
- **4 byte** per il puntatore alla postings list
- **3 byte** per il puntatore al termine nella stringa
- **8 byte** in media per il termine stesso nella stringa
Totale per termine:
$4 + 4 + 3 + 8 = 19 \text{ byte}$
Numero di termini:
$400,000$
Quindi:
$400{,}000 \times 19 = 7{,}600{,}000 \text{ byte} \approx 7.6 \text{ MB}$
##### Con blocking (k = 4):
- hai:
    - **1 puntatore per blocco** → 3 byte
    - **+ 4 byte per le lunghezze delle parole** (1 byte ciascuna)
👉 totale:
$3 + 4 = 7 \text{ byte}$
👉 risparmi: $12 - 7 = 5 \text{ byte ogni 4 termini}$
- prima avevi **4 puntatori** = 4⋅3=12 byte
- ora hai **1 puntatore + 4 lunghezze** = 3+4=73+4=73+4=7 byte

Risparmio:

12−7=5 byte ogni 4 termini12-7=5 \text{ byte ogni 4 termini}12−7=5 byte ogni 4 termini
##### IN TERMINI DI TEMPO
- per la ricerca viene fatto un calcolo di probabilità del valore atteso
	- assumendo che siano tutte ugualmente probabili
$$E[X]=∑(valore×probabilita)$$

###### senza blocking
Formula (semplificata):
$$(1 + 2×2 + 4×3 + 4)/8 ≈ 2.6$$
- mediamente **~2.6 confronti**
###### con blocking
👉 ora hai 2 fasi:
1. binary search sui blocchi
2. ricerca lineare nel blocco
Formula:
$$(1 + 2×2 + 2×3 + 2×4 + 5)/8 = 3$$
$$\frac{tp​+k​}{k}$$

byte per puntatore al termine + numero blocchi/numero blocchi

👉 media:
- circa **3 confronti**
#### ESERCIZIO DI ESAME
[[ESERCIZI CROCS]]
##### ULTERIORI OTTIMIZZAZIONI APPLICATI ALLA VERSIONE A BLOCCHI FRONT CODING
- Il **front coding** è un’ulteriore ottimizzazione applicata al dizionario compresso a blocchi.
- Invece di memorizzare ogni termine per intero, si salva una sola volta il **prefisso comune**, e per le altre parole del blocco si memorizza soltanto la parte finale che cambia.
- Esempio:
    - `automata` `automate` `automatic` `automation`
- Queste parole condividono il prefisso `automat`
- Quindi si può salvare:
    - una volta il prefisso comune
    - poi, per ciascuna parola, solo il suffisso diverso:
        - `a` `e` `ic`  `ion`
![[Pasted image 20260321101544.png|400]]

##### QUANTO RIDUCO CON QUALE TECNICA?
![[Pasted image 20260321101612.png|400]]
### LA POSTING COMPRESSION
- Dopo aver compresso il dizionario, si passa alla compressione delle **postings lists**, che occupano molto più spazio del dizionario e quindi sono il vero problema principale. Le postings possono infatti raggiungere dimensioni molto grandi, quindi comprimerle è essenziale.
- In questa parte della lezione si considera il caso più semplice:
    - **indice booleano**
    - **niente positional index**
    - una posting è semplicemente un **docID**
- Se memorizzassimo ogni docID in modo diretto, potremmo usare interi da **32 bit**. Tuttavia, con circa 800.000 documenti, per rappresentare un docID basterebbero già circa **20 bit**. L’obiettivo della compressione è però usare **molto meno di 20 bit per posting**, quando possibile.
##### GAP ENCODING
- Le postings lists sono ordinate per docID crescente.
- Invece di memorizzare tutti i docID completi, si salva:
    - il primo docID
    - poi, per ciascun successivo, la differenza rispetto al precedente, cioè il **gap**
- Esempio:
    - docID: `33, 47, 154, 159, 202`
    - gap: `33, 14, 107, 5, 43`
- L’idea è che molti gap siano piccoli, soprattutto per termini frequenti, e quindi si possano rappresentare con meno bit dei docID completi.
- nel gap encoding senza compressione, ogni gap viene rappresentato con un numero fisso di bit pari a $\log_2(N_{docid})$ risultando inefficiente perché molti gap sono piccoli ma occupano comunque lo stesso spazio
##### VARIABLE LENGTH ENCODING
- Una volta ottenuti i gap, non conviene usare per tutti lo stesso numero di bit.
- L’idea è usare una codifica a **lunghezza variabile**:
    - pochi bit per i numeri piccoli
    - più bit per i numeri grandi
- Idealmente, se un gap vale $G$, ci piacerebbe usare circa $\log_2 G$ bit, cioè un numero di bit proporzionale alla grandezza del valore da rappresentare.
- vedremo
	- *codifica unaria*
	- *gamma code*
	- *variable byte encoding*
	- *simple9*
##### CODIFICA UNARIA
- La codifica unaria rappresenta il numero $n$ come:
    - $n$ uni seguiti da uno zero
- Esempio:
    - $3$ → `1110`
- Da sola non è molto efficiente, ma è utile come componente di altre codifiche.
- In particolare, è ottimale solo per una distribuzione molto specifica, circa $P(n)=2^{-n}$
##### GAMMA CODE
- Il **gamma code** codifica un numero $G$ dividendolo in due parti:
    - **offset**
    - **length**
- Procedimento:
    1. scrivo $G$ in binario
    2. tolgo il primo 1 → ottengo l’**offset**
    3. considero la lunghezza dell’offset
    4. codifico questa lunghezza in **unario**
    5. concateno lunghezza e offset
- Esempio con $G=13$:
    - 13 in binario = `1101`
    - offset = `101`
    - lunghezza offset = 3
    - 3 in unario = `1110`
    - gamma code = `1110101`
- Proprietà importante:
    - un gamma code per $G$ usa $2\lfloor \log_2 G \rfloor + 1$bit
    - quindi è vicino all’ordine di grandezza ideale $\log_2 G$, pur restando decodificabile in modo univoco
- LIMITE PRATICO DEL GAMMA CODE
	- Il gamma code è teoricamente molto elegante e comprime bene.
	- Però lavora a livello di **bit**, quindi in pratica è più lento da manipolare, perché le macchine lavorano meglio con byte e parole di memoria.
	- Per questo, nei sistemi reali si preferiscono spesso codifiche come **Variable Byte**, che sono più semplici e più veloci, anche se comprimono un po’ meno.
##### VARIABLE BYTE ENCODING
- Il **Variable Byte Encoding (VB)** è una tecnica di compressione usata per rappresentare numeri interi (tipicamente i **gap delle postings lists**) usando un numero variabile di byte.
- È una tecnica **più pratica e veloce** rispetto al **Gamma code**, perché lavora a livello di **byte** (e non di bit), quindi è più efficiente sulle macchine reali.
-  IN COSA CONSISTE
- L’idea è:
    - rappresentare un numero usando **uno o più byte**
    - usare **l’ultimo bit di ogni byte** come **continuation bit**
        - indica se ci sono altri byte dopo
-  COME FUNZIONA
- Ogni byte è formato da:
    - **7 bit per il valore**
    - **1 bit di continuation (MSB)**:
        - `1` → questo è l’ultimo byte del numero
        - `0` → ci sono altri byte dopo che rappresentano quel numero
👉 Procedura:
1. si prende il numero da codificare
2. lo si scrive in binario
3. lo si divide in gruppi da **7 bit** (partendo da destra)
4. ogni gruppo diventa un byte:
    - si aggiunge il bit di continuation davanti
- tu vai a fare una lettura lineare di tutta la stringa
![[Pasted image 20260325100233.png]]

###### TABELLA SULLA RCV1 COMPRESSION
![[Pasted image 20260325100521.png]]
- la collezione viene inizialmente presa come file xml(davvero pesante)
- poi viene ripulita come text
- viene fatta la matrice di incidenza dei termini e documenti
- è irrealizzabile usare il gamma encoding quindi ci fermiamo all'uso del variable byte encoding
##### SIMPLE9 ENCODING
- Il **Simple9 encoding** è una tecnica di compressione delle postings più avanzata rispetto al Variable Byte.
- NON lavora su un numero alla volta (come VB), ma su **più numeri insieme**.
Si lavora su blocchi da **32 bit (4 byte)**:
- i primi **4 bit (selector)** dicono:
	- quanti numeri sto salvando
	- quanti bit uso per ciascuno
- **28 bit rimanenti** → contengono i numeri (i gap)
però è:
- più complesso da implementare
- richiede scegliere il formato giusto per ogni blocco
![[Pasted image 20260325101708.png|400]]
