# Domanda 1 (20% della valutazione complessiva)
Mostrare uno schema concettuale ER che rappresenti un database dei Libri presenti nelle diverse
biblioteche di area dell’Ateneo di Tor Vergata
- Ogni Libro è identificato dal titolo, dall’ISBN (identificatore univoco), casa editrice, anno
pubblicazione, lingua, numero copie.
- Generi delle pubblicazioni, nome del genere, fino a tre chiavi descrizione del genere.
- Ogni Autore è identificato dal nome, cognome, e nazionalità.
- Ogni Biblioteca d’area ha un nome (Biblioteca di Macroarea di Scienze, ingegneria, etc.), un
indirizzo, comprensivo di città e CAP e il nome del responsabile;
- Ogni Libro è di un certo genere e di uno o più autori ed è collocato in una ed una sola
biblioteca d’area;
- La relazione fra libro e Biblioteca d’area deve indicare la collocazione specifica
Mostrare lo schema relazionale derivante dallo schema concettuale.