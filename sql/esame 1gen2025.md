# Domanda 4 (20% della valutazione)
Considerare lil seguente schema relazionale NON in 1 NF, 2 NF, e 3NF.
1. Libri (CodLibro, Titolo, Autore, Genere, Editore, IndirizzoEditore, CittàEditore, AnnoPubblicazione, ISBN, NomeSezione, PosizioneScaffale)
2. Prestiti (CodPrestito, CodLibro, Titolo, CodUtente, NomeUtente, CognomeUtente, Telefono, DataPrestito, DataScadenza, DataRestituzione)
3. Utenti (CodUtente, NomeUtente, CognomeUtente, Indirizzo, Città, CAP, Telefono, E-mail, CodLibroAttualmenteInPrestito, TitoloLibroInPrestito, DataPrestito, DataScadenza),
Determinare:

> Le violazioni delle forme normali presenti nello schema relazionale iniziale.

Non in **1NF** perche' ci sono informazioni ridondanti tra le relazioni: comapre piu volte il campo Telefono (riferito all'utente), Titolo (riferito a libro).

Violano la **2NF** (ogni dipendenza da una chiave deve essere completa) perche' non e' 1NF anche se non ci sono dipendenze parziali.

3NF violata perche' ci sono dipendenze $X \to Y$ dove $X$ non e' superchiave e $Y$ non e' nemmeno attributo primo.

*  Uno schema relazionale normalizzato fino alla terza forma normale (3NF).

* `Utente(CodUtente, NomeUtente, CognomeUtente, Indirizzo, Citta', CAP, Telefono, E-mail)` dove `CodUtente` e' Primary Key.
* `Prestito(CodPrestito, CodLibro, CodUtente, DataPrestito, DataScadenza, DataRestituzione)` dove `CodPrestitio` e' Primary Key.
* `Libro(CodLibro, Titolo, Autore, Genere, Editore, IndirizzoEditore, CittàEditore, AnnoPubblicazione, ISBN, NomeSezione, PosizioneScaffale)` dove `CodLibro` e' Primary Key
e' stata rimosso `CodLibroAttualementeInPrestito` e le informazioni dipendenti da questa poiché  l'informazione e' ridondate e puo' essere ricavata dalla relazione `Prestito`

*  Verificare la BCNF
La BCNF e' verificata: tutte le dipendenze sono di tipo Chiave -> Non Chiave.

*  Un modello Entità-Relazione (ER) che rappresenti in modo chiaro e strutturato la base di dati.