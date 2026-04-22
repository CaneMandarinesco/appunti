# Domanda 1 (20% della valutazione complessiva)
Mostrare uno schema concettuale ER che rappresenti un database dei Libri presenti nelle diverse
biblioteche di area dell’Ateneo di Tor Vergata
- Ogni Libro è identificato dal titolo, dall’ISBN (identificatore univoco), casa editrice, anno
pubblicazione, lingua, numero 3copie.
- Generi delle pubblicazioni, nome del genere, fino a tre chiavi descrizione del genere.
- Ogni Autore è identificato dal nome, cognome, e nazionalità.
- Ogni Biblioteca d’area ha un nome (Biblioteca di Macroarea di Scienze, ingegneria, etc.), un
indirizzo, comprensivo di città e CAP e il nome del responsabile;
- Ogni Libro è di un certo genere e di uno o più autori ed è collocato in una ed una sola
biblioteca d’area;
- La relazione fra libro e Biblioteca d’area deve indicare la collocazione specifica
Mostrare lo schema relazionale derivante dallo schema concettuale.

### Svolgimento

![[Drawing 2025-07-09 16.03.38.excalidraw]]

Schema relazionale: 
* `LIBRO(ISBN, Titolo, CasaEditrice, AnnoPub, Lungua, NumeroCopie, NomeGenere, CollocazioneLibro, NomeBiblioteca)`.
	* `ISBN` e' Primary Key
	* `NomeGenere` e' Foreign Key su `GENERE`
	* `NomeBiblioteca` e' Foreign Key su `BIBLIOTECA`
* `GENERE(NomeGenere, Key1, Key2, Key3)`
	* `NomeGenere` e' Primary Key
	* `Key1, Key2, Key3` possono essere valori nulli
* `AUTORE(NomeAutore, CognomeAutore, NazionalitaAutore)`
* `LIBROAUTORE(ISBN, NomeAutore, CognomeAutore, NazionalitaAutore)`, dove tutti 4 i campi sono Primary Key. 
	* `ISBN` Foreign Key su `LIBRO`
	* `NomeAutore, CognomeAutore, NazionalitaAutore` Foreign Key su `AUTORE`

# Domanda 2 (10% della valutazione complessiva)
Modificare lo schema in modo che rappresenti il prestito dei libri e vengano soddisfatte le seguenti
specifiche:
- Ogni libro è dato in prestito ad un personale autorizzato, compatibilmente con il numero di
copie disponibili.
- Ogni personale autorizzato è identificato dalla matricola, nome cognome e dipartimento di
afferenza.
- La relazione fra libro e prestito deve indicare la data di prestito e eventualmente la data di
riconsegna.
- Ugni personale autorizzato non può prendere in prestito più di 5 libri.
Mostrare lo schema relazionale derivante dallo schema concettuale.

Schema relazionale risultante:
* `PRESTITO(ISBN, Matricola, DataPrestito, DataRestituzione)`
	* `ISBN, Matricola` sono Primary Key e Foreign Key, rispettivamente su `LIBRO` e `PERSONALE`
# Domanda 3 (30% della valutazione)
In base allo schema relazionale della domanda 2, scrivere le query in SQL che rispondono alle seguenti
domande.
> Mostrare tutti i libri attualmente in prestito

```sql
SELECT Titolo
FROM PRESTITO AS P JOIN LIBRO AS L ON P.ISBN = L.ISBN
WHERE P.DataPrestito <= NOW() AND (P.DataRestituzione IS NULL OR P.DataRestituzione > NOW());
```

> Elencare i libri per ogni genere e per ogni macroarea.

```mysql
SELECT Titolo, NomeGenere, NomeBiblioteca
FROM LIBRO
GROUP BY NomeBiblioteca, NomeGenere, Titolo;
```

> Trovare il genere maggiormente dato in prestito

```sql
SELECT NomeGenere
FROM LIBRO AS L JOIN PRESTITO AS P ON P.ISBN = L.ISBN
GROUP BY NomeGenere
ORDER BY COUNT(*) DESC
LIMIT 1;
```

c. Trovare il genere maggiormente dato in prestito
Scrivere in algebra relazionale una query che mostri i libri “fisica” nella macroarea di Scienze.
# Domanda 4 (30% della valutazione)
Considerare il seguente schema relazionale NON in 1NF, 2NF e 3NF.
1. `Dipendenti (Matricola, Nome, Cognome, DataNascita, Indirizzo, Città, CAP, Telefono, Email, CodUfficio, NomeUfficio, TelefonoUfficio, CodProgetto, NomeProgetto, DataInizioProgetto, DataFineProgetto, RuoloNelProgetto)`
2. `Uffici (CodUfficio, NomeUfficio, Piano, Edificio, IndirizzoEdificio, CittàEdificio, Telefono, CodDipendenteResponsabile, NomeResponsabile, CognomeResponsabile)`
3. `Progetti (CodProgetto, NomeProgetto, DataInizio, DataFine, Budget, CodDipendenteAssegnato, NomeDipendente, CognomeDipendente, RuoloNelProgetto)`

Domande:
1. Identificare e spiegare quali problemi di normalizzazione presenta questo schema (violazioni di 1NF, 2NF e 3NF).
Lo schema viola 1NF perche' ci sono informazioni ridondanti, es: `NomeUfficio` compare come attributo distinto in `Dipendenti` e in `Uffici` e `DataFineProgetto`, `DataInizioProgetto` si riferiscono agli stessi dati di `Progetti(DataInizio, DataFine)`.
Inoltre non e' gestito correttamente il caso in cui un progetto abbia piu dipendenti o viceversa.

La `2NF` e' violata perche' lo schema non e' in `1NF`, ma non ci sono attributi che dipendono da chiavi primarie parziali. Pero' se fosse che dipendente e' identificato da `Matricola` e `CodUfficio` ci sarebbero dipendenze paraziali di chiave.

La `3NF` e' violata se $X \to Y$ allora $X$ non e' attributo primo e $Y$ non e' parte della chiave primaria. Possiamo trovare violazioni in:
* `Progetti`, gli attributi `NomeDipendente, CognomeDipendente` dipendono da `CodDipendenteAssegnato`.
* `Uffici`, gli attributi del `Responsabile` dipendono dal `CodDipendenteResponsabile`
* similmente per `Dipendenti`

2. Proporre una decomposizione dello schema per portarlo in 3NF, mantenendo la perdita
minima di informazioni e preservando la dipendenza funzionale.

* semplice

3. Spiegare come la nuova struttura migliora l’integrità e la coerenza dei dati.
