### 🧠 **Query SQL - Domande aperte avanzate**

#### 1.
Hai una tabella `Ordini(CodOrdine, ClienteID, DataOrdine, Importo)`.  
Scrivi una query che restituisca i clienti che **hanno effettuato almeno un ordine ogni mese dell'anno corrente**.

> ⤷ Usa funzioni temporali, `GROUP BY`, `HAVING` e nidificazione.

```sql
SELECT ClientID
FROM Ordini
WHERE 
GROUP BY ClienteID
HAVING COUNT(DISTINCT extract(MONTH from ))
```

---

#### 2.

Scrivi una **vista** che mostra i prodotti il cui prezzo è maggiore della media dei prezzi dei prodotti dello stesso fornitore.  
Impedisci che si possano aggiornare i prezzi da questa vista se la modifica violerebbe la condizione iniziale.

> ⤷ Usa `WITH CHECK OPTION` + `AVG` in una sottoquery correlata.

---

#### 3.

Hai la tabella `Libri(Titolo, ISBN, AnnoPubblicazione, CasaEditrice)`.  
Scrivi una query che restituisca le case editrici che **non hanno pubblicato alcun libro negli ultimi 3 anni**, **utilizzando `EXISTS` o `NOT EXISTS`**.

---

#### 4.

Hai una tabella `Prestiti(CodLibro, CodUtente, DataPrestito, DataRestituzione)`.  
Trova gli utenti che **non hanno mai restituito in ritardo** un libro, sapendo che il prestito dura al massimo 30 giorni.  
Usa `NOT EXISTS` o `ALL`.

---

#### 5.

Supponiamo che ogni `Dipendente` abbia un attributo `Salario`.  
Trova le **sedi** in cui **tutti i dipendenti guadagnano almeno quanto la media nazionale**.

> ⤷ Usa `ALL`, `HAVING` e sottoquery nidificate.

---

#### 6.

Hai la tabella `Vendite(ClienteID, ProdottoID, Quantita, DataVendita)`.  
Scrivi una vista che mostra i clienti che hanno acquistato **più di 5 prodotti diversi** in ogni mese.

> ⤷ La vista deve poter essere interrogata, ma le modifiche devono rispettare la condizione. Usa `WITH CHECK OPTION`.

---

### 🧪 **Normalizzazione e vincoli - Domande complesse**

#### 7.

Hai una relazione `Prenotazioni(PazienteID, MedicoID, Giorno, Reparto, TipoEsame)`.  
Sai che ogni medico è assegnato a un solo reparto e ogni tipo di esame può avvenire solo in certi reparti.  
Quali **anomalia** potresti avere? In quale **forma normale** la relazione non si trova?

---

#### 8.

Considera la relazione `Insegna(Professore, Corso, Lingua)`.  
Un professore può insegnare più corsi e parlare più lingue, ma non tutte le lingue sono usate per ogni corso.  
Qual è la forma normale più bassa violata? Spiega che **dipendenza multivalore** stai violando.

---

#### 9.

Hai una tabella `Consegne(OrdineID, CorriereID, GiornoConsegna)`.  
Ogni ordine può essere spedito da più corrieri in giorni diversi.  
Spiega perché potresti **non essere in 4NF** e come decomporre.

---

#### 10.

Hai le seguenti dipendenze:

- A → B
    
- B → C
    
- A → D
    
- D → E
    

Trova le **chiavi candidate**, verifica se l'insieme è **minimale**, e indica se la relazione è in **3NF** e **BCNF**.

---

### 💡 Se vuoi, posso correggere le tue risposte a voce d’esame, oppure prepararti la soluzione passo passo per ognuna.

Pronto per provare?
---
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