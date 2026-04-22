# stored procedure
* efficienza
* sicura: limito le operazioni possibili
* riutilizzabili
```sql
CREATE PROCEDURE DIOCANE @nome tipo, ... AS 

...

GO;
```

# trigger
* paradigma evento condizione azione 
```sql
CREATE TRIGGER noadsf ON TABLE asfae
AFTER UPDATE, INSERT, DELETE
AS BEGIN

END
```

si usa per implementare controlli piu avanzati e messaggi errore.

# transazione 
sequenza di operazioni **stato corretto** a **stato corretto**
* acid
	* atomic
	* consinstency: rispettare i vincoli (**stato corretto**)
	* isolated: no interferenze con altre transazioni, gestione **concorrenza sui dati**, si risolve con sistemi di `lock` e isolazione.
	* durable (persistente): una volta `commit` le modifiche devono essere applicate in mem non volatile.
* es: pagamento da $A$ a $B$. Devo essere sicuro che sottrago i soldi da $A$ e li aggiungo a $B$ atomicamente. Se qualcosa va storto devo annullare la transazione
* `commit`, `rollback` e `autocommit`

si definisce con `START TRANSACTION` e `COMMIT`

## recovery manager
si basa su:
* **log WAL**: registra le attivita nel log prima di eseguirle, cosi posso usare il log per annullare le operazione
* **checkpoint**: punti di ripristino (tramite `diff` per esempio), piu' veloce rispetto a seguire il log per annullare. Si trasferiscono in memoria di massa le **pagine sporche**
* **dump**: proprio un dump della memoria diocaro

analizzando il `log` posso identificare:
* **operazioni da annullare**: quelle che non hanno raggiunto il `commit`
* **operazioni da rifare**: quelle che hanno raggiunto il `commit`

tipi di guasti:
* **soft**: contenuto RAM persa, ma disco intatto. Recupero guardando il log
* **hard**: perdo il contenuto della memoria ram. Perdita di log. Conviene ripristinare da un checkpoint. Se disponibili, posso usare i log per ritornare allo stato del db prima del guasto.

### recupero
4 fasi per il **recupero a caldo**:
* **analisi**
* costruzione di undo e redo
* redo: applica **policy di durabilita**
* undo: annulla le operazioni non completate, applica **policy di atomicita**

recupero **a freddo**:
* ripristino da backup
* se dipsonibile, faccio redo delle operazioni da log


### scheduler 
**scheduler**: per schedulare le transazioni insieme. accetta o rifiuta transazioni
* seriale: una transazione per volta
* serializabile: eseguo piu' transazioni insieme ma dal punto di vista dell'IO (utente), l'esecuzione e' come se fosse seriale.

#### view serializabili
* due scheduler (in particolare uno concorrente e serializzabile) sono equivalenti?

concetti chiave:
* T1 legge da T2, se c'e' stata una $w1$ su un'attributo e T2 poi legge con $r1$
* scrittura finale: l'ultima $wk$ eseguita determina il valore letto dagli altri.

*due scheduler sono VS se valgono i concetti chiave.* e le transazioni leggono sempre stessi valori 

#### conflict serializabilita
conflitti:
* WoW
* RoW
* WoR
due scheduler sono equivalenti se gestiscono i conflitti nello stesso ordine, verificabile con grafo

### locking
si effettua il two phase locking: acquisco e poi rilascio in 2 fasi. Non posso fare l'una o l'altra fuori fase.

### deadlock
succede quando due transazioni hanno lock l'una sull'altra.

# Tipi dipendenze funzionali
* banale: $\{A,B\} \to A$.

# Calcolo relazionale (dichiarativo)
* sui **domini**: ottieni domini presi da relazioni che soffisfano condizionew
* o sulle **tuple**: ottieni tuple da una relazione che soddisfano una determinata condizione espressa come logica del primo ordine.
* basato sulla logica del primo ordine (e quindi sulle **fbf**)
* una query non sicura puo' diventare sicura aggiungendo operatore esistenzale: **esiste** o **non per ogni**.
* algebra e' procedurale

Nota, **potenza espressiva**: calcolo relazionale e algebra relazionale sono equivalenti, una cosa si puo' esprimere nell'altro.

# chiave minimale
* insieme minimale di attributi che determinano tutta la relazione
* minmale := se tolgo un'attributo dalla chiave minimale non determina piu tutti gli attributi.

# indice
struttura dati per velocizzare le operazioni di ricerca (join, select).
Non conviene avere indici su attributi che cambiano spesso (bisogna aggiornare la struttura dati associata porcaccioddio)

* `btree`: implementazione in $O(log n)$

# progetto

### schema er
* orario settimanale mette in relazione un'orario con la sede ama, permette di riutilizzare gli orari (no ridondanze)
* lista cap e' una tabella di (cap, sede_ama). ossia ad ogni cap corrisponde una sede_ama.
* turno settimale e' simile a orario settimanale

Le operazioni piu' importanti da ottimizzare sono:
* le query con molti join
* le operazioni di prenotazione per l'utente

