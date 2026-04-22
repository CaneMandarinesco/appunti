### PROBLEMA DI MEMORIZZAZIONE IN FASE DI COSTRUZIONE
il numero totale di token $T$ nella collezione può essere enorme, quindi salvare tutte le coppie `(termID, docID)`
- quindi costruire un indice solo in memoria ram potrebbe essere difficile
→ Token string = la singola parola estratta dal testo del documento.
#### RCV1
**RCV1 (Reuters Corpus Volume 1)** è una collezione di documenti usata spesso negli esempi di Information Retrieval per **stimare dimensioni e costi della costruzione di un indice**.
##### Come è fatto un documento in questo esempio
Un documento della collezione è **una notizia Reuters**, composta principalmente da:
- **title**
- **body**
Durante la costruzione dell’indice:
- il testo del titolo e del corpo viene **analizzato e segmentato**
- il testo viene diviso in **token (parole)** tramite **tokenization**
- questi token vengono poi usati per costruire l’inverted index

##### RCV1 in numeri
![[Pasted image 20260314181945.png]]
- È composta da **circa 800.000 documenti** di notizie Reuters.
- Ogni documento contiene mediamente **circa 200 token** (parole).
- Il numero totale di token nella collezione è quindi circa **100 milioni**.
- DOMANDA CHE FA SEMPRE ALL'ESAME
	- perché la media del numero di bytes per token è inferiore a quella per termine?
		- i **token** rappresentano **tutte le occorrenze delle parole nel testo**
			- molte parole molto frequenti sono **molto corte** (es. _the, a, of, to_)
			- queste parole corte compaiono **migliaia di volte** e abbassano la media della lunghezza dei token
		- i **term**, invece, rappresentano **le parole distinte del vocabolario** e quindi **ogni parola viene contata una sola volta**
			- di conseguenza le parole lunghe (che spesso sono meno frequenti) **pesano di più nella media dei term**, rendendo la media dei byte per term **più alta** rispetto a quella dei token.
###### TOKEN vs TERM
- **Token (token stream)**
    - sono **le parole effettivamente estratte dal testo dei documenti**
    - rappresentano **tutte le occorrenze delle parole**, nell’ordine in cui appaiono nei documenti
- Il **token stream** è l’insieme di tutte le coppie:
	- `(termID, docID)`
	- generate durante il parsing dei documenti.
- **Term**
    - sono **le parole distinte del vocabolario**
    - cioè **le parole nel dizionario dell’indice**, ogni parola compare **una sola volta**
- tutto questo vedendo i dati (slide con quelle tabelle) occupa tanta memoria
##### Come risolvere?
#### ALGORITMI DI COSTRUZIONE 
##### BSBI(Block Sort Base Indexing)
- È un algoritmo per **costruire l’inverted index quando la collezione è troppo grande per stare in memoria (RAM)**
- Idea principale:
1. **si divide il token stream in blocchi**
2. ogni blocco viene **ordinato in memoria RAM**
3. il blocco ordinato viene **scritto su disco**
4. alla fine **si fa il merge dei blocchi** per ottenere l’indice finale
- potrei usare il 90% della ram senza sfruttarla tutta
##### PSEUDOCODICE
![[Pasted image 20260314183331.png]]
Dato che il dataset è troppo grande per stare in memoria:
- si usa **solo una parte della RAM** (ad esempio il 90%)
- si salva il resto su **disco**
##### Procedura:
- il tutto organizzato con un dizionario con AVL
- sia **N** il numero totale di coppie `(termID, docID)`
- si divide il token stream in **blocchi di dimensione B**
- B è scelto in modo che **ogni blocco possa stare in RAM**
- costi
	- si hanno `N/B` blocchi
	- ogni blocco si sposta in ram e si ordina
		- costo `O(B log B)`
		- facciamolo per ogni blocco
		- ``O((N/B) * B log B)`
			- `O(N log B)`
			- dove $B$ sarebbe il token stream del singolo blocco
Durante il merge:
- si leggono i blocchi dal disco
- si uniscono le posting list dei termini uguali ma sempre controllando il loro ordine
- prendiamo per assodato che nella fase di tokenization e di divisione in blocchi i docID siano incrementali
Se un termine compare in più blocchi:
`postings_final = concatenazione(postings_blocco1, postings_blocco2, ...)`
Dato che ogni blocco è già ordinato:
- le posting list sono **già in ordine**
- quindi basta **concatenarle sequenzialmente**.
![[Pasted image 20260314183959.png|400]]
##### SPIMI(Single Pass In Memory Indexing)
- **SPIMI** significa **Single-Pass In-Memory Indexing**.  
- È un algoritmo di costruzione dell’indice invertito pensato per collezioni grandi, quando non conviene usare l’approccio con ordinamento globale dei record come in BSBI.
- Idea principale:
	- l’obiettivo è **sfruttare al massimo la RAM disponibile**
	- lavora **un blocco alla volta in memoria**
	- per ogni blocco costruisce direttamente un **indice invertito parziale**
	- quando la memoria finisce, scrive il blocco su disco e ricomincia con un nuovo blocco.
- **Single-pass**: i token vengono processati **una sola volta**
	- non usa una struttura globale `term -> termID` come BSBI
	- usa direttamente i **termini** invece dei `termID`
- **non ordina tutte le coppie `(termID, docID)`** prima di costruire le posting list
	- costruisce le **posting list direttamente mentre legge i token**.
		- appena si legge un token, si va subito nella posting list giusta
###### PSEUDOCODICE
![[Pasted image 20260314184903.png]]
- costi
	- `Θ(T)`
###### PASSAGGI
- 1. Processing dei token
	- si leggono i documenti sequenzialmente
	- si generano i token `(term, docID)`
	- si continua finché la **RAM si riempie**
- 2. Struttura dati
	- si usa una **tabella hash** come dizionario
	    - chiave → **termine**
	    - valore → **posting list (lista di docID)**
	- se la posting list la raddoppio
- 3. Inserimento dei token
	- Per ogni token `(term, docID)`:
	- se il termine è **già presente** nella hash table:
	    - si recupera la sua **posting list**
	    - si aggiunge il `docID` alla lista
	- se il termine **non è presente**:
	    - si crea una **nuova entry nel dizionario**
	    - si crea una nuova **posting list**
- 4. Quando la memoria è piena
	-  si **ordinano i termini del dizionario**
	    - cioè le **chiavi della hash table**
	- si scrivono su disco:
	    - il **dizionario ordinato**
	    - le **posting list**
-  5. Ripartenza
	- si **svuota la RAM**
	- si ricomincia a processare i **token successivi**
- 6. Alla fine ottieni:
	- **molti blocchi su disco**
	- si fa il **merge dei blocchi**
		- per ogni termine:
		    - si uniscono le posting list provenienti dai vari blocchi
	- si ottiene **l’inverted index finale**
##### INDEXING DISTRIBUITO
- ricordiamo che distribuito significa che il lavoro di costruzione dell’indice **non viene fatto su una singola macchina**, ma viene **suddiviso tra molte macchine (nodi)** che lavorano in parallelo.
- *SLA*
	- *Service level agreement*
		- Lo **SLA** indica il livello di disponibilità del sistema, cioè **quanto tempo il sistema deve rimanere operativo**.
		- lo SLA al 99.99% non è sempre fattibile per costi
Quando si hanno **molte macchine**, la probabilità che **qualcuna si rompa aumenta**.
Esempio concettuale:
- se ho **1000 nodi**
- anche se ogni nodo ha una probabilità molto alta di funzionare
- è molto probabile che **in un certo momento almeno uno fallisca**.
![[Pasted image 20260314190106.png]]

Quando l’indice è distribuito su più macchine, possiamo dividerlo in due modi principali.
##### Term-partitioned index
In questo caso:
- ogni macchina gestisce **un sottoinsieme dei termini**
- quindi memorizza le **posting list di quei termini**
Esempio:
```scss
Macchina 1 → termini A–F  
Macchina 2 → termini G–P  
Macchina 3 → termini Q–Z
```
Se l’utente IN FASE DI QUERY E NON DI COSTRUZIONE cerca:
`brutus AND caesar`
la query deve essere inviata **alle macchine che gestiscono quei termini**.

##### Document-partitioned index
In questo caso:
- ogni macchina gestisce **un sottoinsieme dei documenti**
- quindi memorizza **tutti i termini**, ma solo per quei documenti.
Esempio:
```scss
Macchina 1 → documenti 1–1M  
Macchina 2 → documenti 1M–2M  
Macchina 3 → documenti 2M–3M
```
Se l’utente IN FASE DI QUERY E NON DI COSTRUZIONE cerca:
`brutus AND caesar`
la query viene **eseguita su tutte le macchine**, ma ciascuna restituisce risultati solo per **i propri documenti**.

I grandi motori di ricerca (Google, Bing, ecc.) **preferiscono il document partitioning**.
#### MODELLO MAP REDUCE
Per costruire l’indice in un sistema distribuito si usa spesso il modello **MapReduce**.
Le due fasi principali sono:
##### MAP
- i documenti vengono **divisi in blocchi (splits)**
- ogni macchina **fa il parsing dei documenti**
- produce coppie:
	`(termID, docID)`
Queste coppie vengono scritte in **segment files**.
##### REDUCE
- le coppie con lo stesso termine vengono **raccolte insieme**
- si costruiscono le **posting list finali**.
Ogni nodo reduce si occupa **di un sottoinsieme dei termini**.

- **MapReduce** viene usato soprattutto nella **fase di costruzione dell’indice** (index construction).
- **Term-partitioned** e **Document-partitioned** descrivono **come l’indice è distribuito tra le macchine**, cosa che è particolarmente importante **quando si eseguono le query (ricerca)**.
##### Data Flow dell'index construction
- il data flow rappresenta le procedure di costruzione dell'indice
- il **master** divide la collezione in **split** (blocchi di documenti) e li assegna dinamicamente ai nodi disponibili
    - se un nodo fallisce, il task viene riassegnato → **fault tolerance**
    - **alta availability** grazie a backup dei dati
    - **Fault tolerance** = capacità del sistema di continuare a funzionare anche se alcune parti si rompono
- utilizzeremo due task paralleli che rappresentano map e reduce
	- parser(Map)
	- inverter(Reduce)
###### Fase di parsing (Map)
- ogni nodo (parser) prende uno **split**
- legge i documenti e fa:
    - `parsing + tokenization`
    - produce direttamente coppie:
        `(term, docID)`
- le coppie vengono scritte in **segment files**
    - organizzati per **partizioni di termini** `(es. a–f, g–p, q–z)`
- costruisce una **struttura locale temporanea**, NON ancora l’indice finale
###### Shuffle / Partitioning
- le coppie vengono **raggruppate per termine**
- tutti i dati relativi allo stesso termine vengono inviati allo stesso nodo (inverter)
- se vedi infatti dalla `a-f` vanno tutte in un unico inverter
###### Fase di inversione (Reduce)
- gli **inverter** ricevono tutte le coppie di un termine
- costruiscono le **posting list**:
    `term → [docID1, docID2, ...]`
- producono una parte dell’**inverted index finale**
![[Pasted image 20260317184416.png|400]]

- vogliamo:
    - **partition tolerance** (il sistema funziona anche con problemi di rete)
    - **availability** (sempre operativo)
- quindi accettiamo di perdere:
    - **consistency** (non tutti i nodi sono sempre aggiornati allo stesso istante)
#### Dynamic indexing
- LA NOSTRA ASSUNZIONE FATTA IN PRECEDENZA OVVERO CHE LA NOSTRA STRUTTURA DATI È STATICA ORA NON È PIÙ VALIDA, LA STRUTTURA SARÀ DINAMICA
- Il **dynamic indexing** è il processo di aggiornamento continuo dell’indice quando la collezione di documenti cambia nel tempo.
###### GESTIONE DELLA DYNAMIC INDEXING
- si sfrutta nella fase di risposta alle query
- divido l’indice in:
    - **main index** (grande, su disco)
    - **auxiliary index** (piccolo, in RAM, contiene i nuovi documenti)
- i nuovi documenti vengono inseriti nell’**auxiliary index**
    - quindi non modifico subito il main (operazione costosa)
- durante le query:
    - cerco sia nel **main** che nell’**auxiliary**
    - poi **unisco i risultati**
- periodicamente:
    - faccio il **merge** tra auxiliary e main
    - ottengo un nuovo main aggiornato
- per la eliminazione:
    - uso un **bit vector di invalidazione**
    - i documenti eliminati non vengono cancellati subito
    - ma filtrati a query time
#### Quando va fatto questo merge(sapendo che sia costoso)
- voglio fare **pochi merge**
    - per non toccare spesso il main index
- quindi:
    - lascio crescere l’auxiliary fino a una certa dimensione
    - poi faccio il merge
- trade-off:
    - merge rari → indexing più efficiente
    - ma auxiliary più grande → query più lente
- assunzione:
    - il main index è memorizzato come **un unico grande file**
    - quindi il merge è costoso (richiede riscrittura)
##### Logarithmic merge(miglioria al semplice merge tra i due)
- è un algoritmo per gestire il **dynamic indexing**
- nasce per migliorare la strategia semplice **main index + auxiliary index**, in cui il merge con il main è troppo costoso se fatto troppo spesso
##### Idea principale
- invece di avere solo:
    - un **main index** 
    - un **auxiliary index**
- mantengo **più indici di dimensione crescente**(vedi foto sotto)
	- in memoria tengo:
	    - **$Z_0$**, il più piccolo indice temporaneo
		    - è l'auxiliary index
	- su disco tengo:
	    - **$I_0, I_1, I_2, ...$**
		    - è il main
- abbiamo il nostro auxiliary index $Z_0$, ogni volta che supera una certa soglia in termini di dimensioni va a diventare un main index $I_0$, se già presente un main index $I_0$ quest'ultimo verrà combinato con l'ultimo main index disponibile per crearne uno nuovo $I_k$
	- ad ogni merge i livelli impiegati vengono eliminati permettendo la loro ricostruzione nuova

![[Pasted image 20260317191928.png|400]]
###### PSEUDOCODICE DI LOG MERGE
![[Pasted image 20260317192130.png]]
- COSTI DI LOG MERGE
- ricordando che 
	- `T = numero totale di token nella collezione`
- con il metodo semplice **main + auxiliary**:
    - un posting può essere riscritto moltissime volte
    - costo totale:
        - $O(T^2/n)$
			- T = numero totale di posting (nella collezione)  
			- n = dimensione massima del buffer $Z_0$ (in memoria ram)
        - un merge costa T
	        - ogni volta faccio un merge quindi
	        - $(T / n) × T$
- con **logarithmic merge**:
    - ogni posting viene rielaborato solo una volta per livello
    - numero di livelli circa:
        - $\log(T/n)$
	    - costo totale:
	        - $O(T \log(T/n))$
###### MULTIPLE INDEXES
I **multiple indexes** sono una struttura in cui l’informazione non è contenuta in un unico inverted index, ma è distribuita su **più indici separati**, che devono essere consultati insieme durante la ricerca.
- presenti in ogni sistema con DYNAMIC INDEXING compreso quello che usa il log merge
- durante una query avrò più indici quindi la loro ricerca sarà complessa
	- oltretutto sarà anche complesso gestire la loro frequenza e il loro ranking
##### nei sistemi reali (es. Twitter / Earlybird):
- l’indice è diviso in **segmenti piccoli**
- ogni segmento è:
    - scrivibile (uno alla volta)
    - oppure read-only
- le posting list:
	- sono spesso ordinate **per tempo (reverse chronological)**
		- quindi:
		    - i documenti più recenti sono in fondo (o in cima)
per query real-time:
- si parte dagli elementi più recenti
- si scorrono velocemente le posting list
- molto efficiente per contenuti freschi (tweet)