#### 1. Aggregazioni nidificate  
Scrivi una query che restituisce i dipendenti il cui salario è **superiore alla media dei salari del proprio dipartimento**.  
Tabelle:

- `Dipendenti(Matricola, Nome, Cognome, Dipartimento, Salario)`
```sql
SELECT Matricola, Salario, Nome, Cognome, Dipartimento
FROM Dipendenti AS D
WHERE Salario >= (
	SELECT AVG(Salario)
	FROM Dipendenti as D1
	WHERE D1.Dipartimento = D.Dipartimento
)
```


#### 2. **Sottoquery con `EXISTS`**  
Trova i pazienti che **hanno fatto almeno una visita in ogni mese dell’anno corrente**.  
Tabelle:
- `Visita(CodVisita, CF, DataOra)`
- `Paziente(CF, Nome, Cognome)`

```sql
SELECT P.CF, P.Nome, P.Cognome
FROM Paziente AS P JOIN Visita AS V on V.CF = P.CF 
WHERE YEAR(DataOra) = YEAR(now())
GROUP BY P.CF, P.Nome, P.Cognome
HAVING COUNT(DISTINCT MONTH(DataOra)) = 12;
```

---

3. **Aggregazione e HAVING complesso**  
    Scrivi una query per trovare tutti gli autori che **non hanno scritto nessun libro pubblicato prima del 2020**.  
    Tabelle:
    - `Libro(ISBN, Titolo, AnnoPubblicazione)`
    - `LibroAutore(ISBN, Nome, Cognome)`

```sql
SELECT DISTINCT Nome, Cognome
FROM Libro AS L JOIN LibroAutore AS LA ON L.ISBN = LA.ISBN
GROUP BY Nome, Cognome
HAVING MIN(AnnoPubblicazione) >= 2020;
```

---

4. **Interrogazione con JOIN e vincolo**  
    Restituisci il **numero medio di libri attualmente in prestito per ogni biblioteca**, considerando solo quelli **non ancora restituiti**.  
    Tabelle:
    
v    - `Prestito(ISBN, CodBib, DataPrestito, DataRestituzione)`
    - `Libro(ISBN, Titolo)`
    - `Biblioteca(CodBib, NomeBib)`
```mysql
SELECT Biblioteca.CodBib, Biblioteca.NomeBib, COUNT(DISTINCT L.ISBN)
FROM Prestito AS P JOIN Libro AS L ON L.ISBN = P.ISBN 
	JOIN Bilioteca AS B ON B.CodBib = P.CodBib
GROUP BY Biblioteca.CodBib, Biblioteca.NomeBib
```

---

5. **Controllo su vincolo CHECK logico complesso**  
    Definisci un vincolo CHECK su una tabella `Prenotazione` che assicuri che **la data della visita non può essere nel passato** rispetto alla data attuale.
```mysql
CHECK( DataVisita >= now()) 
```
---

6. **Vincoli di integrità referenziale**  
    Spiega come imposteresti un vincolo `ON DELETE SET NULL` in una relazione `Progetto(CodProgetto, CodResponsabile)` per garantire che, se un responsabile viene rimosso dalla tabella `Impiegato(CodImpiegato, ...)`, il campo `CodResponsabile` nel progetto venga azzerato.
    

---

7. **Uso avanzato di `ALL`, `ANY`**  
    Trova i prodotti il cui prezzo è **maggiore di tutti** i prezzi dei prodotti della categoria 'elettronica'.
    

---

8. **Verifica delle forme normali tramite SQL**  
    Hai una tabella `Vendite(ClienteID, NomeCliente, ProdottoID, NomeProdotto, Quantita)`. Quali anomalie o violazioni delle forme normali intravedi? Come normalizzeresti la tabella?
    

---

Fammi sapere se vuoi che ti corregga o se vuoi un’altra batteria. Posso anche mescolare SQL + algebra relazionale in un’unica domanda complessa, in stile esame. Vuoi?