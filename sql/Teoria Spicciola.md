* **modello dei dati**: come descrivo e strutturo i dati? Il piu' usato e' il modello **relazionale**, altri tipi sono: a oggetti, reticolare, gerarchico o semi-strutturato (`NoSQL`).
* Il modello dei dati e' detto **logico** se descrive anche in parte l'organizzazione dei dati nel calcolatore (tabella, puntatori, alberi, file, ecc...)
* Il modello dei dati **concettuale** non descrive come sono organizzati i dati nel calcolatore, e' indipendente dal modello logico.
* indipendenza **fisica**: posso interagire con il DBMS indipendentemente da come sono organizzati i dati al suo interno. Posso cambiare implementazione fisica senza dover aggiornare i programmi che usano il DBMS.
* indipendenza **logica**: posso interagire con il livello esterno indipendentemente dalla logica.
* `DDL`: Data Definition Language, `DML`: Data Manipulation Language.

Ogni base di dati ha:
* **schemi**: oggetti che **non variano nel tempo**, descrivono la struttura dei dati che descrivono un concetto.
* **istanza**: valori istanziati a partire da uno schema, **variano nel tempo.**

## modello relazionale
* si basa su **relazioni e schemi**.
* $D_{1}\times... \times D_{n}$ e' una tupla $(d_{1}, ..., d_{n})$ tale che $d_{1}\in D_1$ ecc...
* $t[A]$: valore di $t$ (riga), sull'attributo $A$ (colonna)
* **Schema di una relazione**: $R(X)$ con $X = (A_{1}, ..., A_{n})$ gli attributi della relazione e $R$ il nome.
* **Scheda di base di dati**: insieme di schemi: $R = \{ R_1(X_{1}), ..., R_n(X_{n})\}$
* Basato sui **valori**, una relazione e' fatta di valori su cui posso fare delle operazioni.

### vincoli
* **vincolo di integrità**: proprietà da rispettare per mantenere un informazione corretta in memoria.
* **vincolo di dominio**: riguarda un solo attributo, es: `VOTO >= 18 AND VOTO <= 30`.
* **vincolo di ennupla**: definita su più attributi, es: `LORDO = (RITENUTE + NETTO)`.

### chiavi
* **superchiave**: insieme di $K$ attributi t.c. ogni ennupla ha valori distinti per i $K$ attributi.
* permettono l'accesso a ciascuna ennupla  nella base di dati, accedo per valore!
* **chiave primaria**: *NON AMMETTE* `null`.
* **foreign key**