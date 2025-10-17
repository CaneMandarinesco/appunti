* `SHOW DATABASES;`
* Pipeline: `from -> where -> select -> order`

# `SELECT`
* Nel costrutto `SELECT ... FROM ...` viene valutato prima il `FROM`.
* `FROM` e' opzionale, es: `select now();` ritorna il timestamp corrente del server.
* `FROM`: vienae usato se si vuole coinvolgere nell'operazione righe appartenenti a piu' tabelle.

### `ORDER BY`
Per ordinare le query:
```sql
...
ORDER BY
	sort_expr1 [asc | desc],
	sort_expr2 [asc | desc],
	...;
```
* `asc` e' usato di default
* per ordinare per una determinata espressione, questa deve comparire anche in `SELECT`

### `WHERE`
Per selezionare le righe solo se soddisfano una condizione che può valere `true|false|unkown`.
* `expr between low and high`. si può usare anche per `timestamps`
* `expr in (v1, v2, ..., vn)`.
* `LIKE pattern` dove pattern e' una stringa in cui `%` e' un matching con uno o più carattere e `_` e' un matching con un carattere.

```sql
select name 
from countries 
where name like 'J%'
order by name;
```

### `DISTINCT`
```sql
select distinct c1, c2, ...
```
Rimuovi i duplicati dal risultato sulla base di `c1, c2, ...` . **Nota**: la keyword va posta subito dopo la parola `select`.

### `COUNT, SUM, MIN, MAX, AVG`
```sql
select count(<*, distinct, all>, attr...)
```
* `*`: conta tutti i valori per quell'attr.
* `all`: conta tutti i valori non nulli trovati su quell'attr.
Lo stesso vale per gli altri operatori...

### `HAVING`
Si usa insieme alla clausola: `GROUP BY`, per specificare la condizione per far apparire ogni gruppo nel risultato, ipotizzando una `groub by Dipartimento`
* `... having avg(Stipendio) >= 25`
* `... having count(all) >= 50`

# `EXCEPT`
Fa parte delle operazioni insiemistiche tra query, si mette appunto tra 2 query per togliere dalla prima i risultati dell'altra. Utile quando si vuole ottenere per esempio un'insieme di oggetti che ha una caratteristica, quando e' piu' semplice ottenere l'insieme di oggetti che non ha quella caratteristica!

# Domini Elementari
* `character [varying] [(lunghezza)] [character set NomeFamiglia]`
* `numeric` -> valore preciso
* `decimal` -> non per forza preciso.
* `date`, `time`, `timestamp`

# Esempi di query
```sql
create table Dipartimento 
{
	Nome varchar(20) primary key,
	Indirizzo varchar(50),
	Citta varchar(20) 
}
```

```sql
create domain Massa as numeric default 0 CHECK (value <= 50)
```

### vincoli intrarelazionali
* `not null`, va messo alla fine della dichiarazione del campo
* `unique`, va messo alla fine oppure scrivo separatamente `unique(Nome, Cognome)`
* `unique(Nome, Cognome)`: in piu' righe non possono comparire stessi nomi o cognomi
* `primary key`: posto alla fine, il campo non puo' essere nullo
* `primary key (Cognome, Nome)`: entrambi i valori sono chiave primaria!

### vincoli integrità referenziale
* `references`: da mettere alla fine della definizione di un attributo
* `foreign key`: per specificare più' attributi insieme

Cosa deve succedere quando un valore referenziato cambia o viene eliminato? (`on delete/update`)
* `cascade`
* `set null`
* `set default`
* `no action`

# Es Pittori
Sia dato lo schema relazionale:
* `PITTORE(Nome, DataNascita, DataMorte, LuogoNascita)`
* `QUADRO(Titolo, NomePittore, NomeMuseo, Data)`
* `MUSEO(NomeMuseo, Indirizzo, Citta)`

> Interrogazione 1: Determinare il nome dei pittori nati a parigi dopo il 1968 che hanno dipinto almeno tre quadri esposti al MOMA

```sql
SELECT PITTORE.Nome
FROM PITTORE JOIN QUADRO on PITTORE.Nome = NomePittore
			 JOIN MUSEO on PITTORE.Nome = NomeMuseo 
WHERE DataNascita > 1968 AND LuogoNascita = 'Parigi'
	AND Museo.Nome = `MOMA` AND Citta = `NewYork`
GROUP BY PITTORE.Nome
HAVING COUNT(*) >= 3
```
Nota: puo' essere riscritta senza join.

> Determinare i nomi dei musei che contengono almeno un quadro dipinto fra il 1300 e 1600 **ma non contengono** quadri di Leonardo Da Vinci.
> 
```mysql
SELECT DISTINCT NomeMuseo
FROM Quadro
WHERE Data BETWEEN 01-01-1300 AND 31-12-1600
EXCEPT
SELECT DISTINCT NomeMuseo
FROM Quadro
WHERE NomePittore = "Leonardo Da Vinci";
```

oppure:
```sql
SELECT DISTINCT NomeMuseo
FROM Quadro
WHERE Data BETWEEN 01-01-1300 AND 31-12-1600 
	AND NomeMuseo NOT IN(
		SELECT DISTINCT NomeMuseo
		FROM Quadro
		WHERE NomePittore = "Leonardo Da Vinci";
	);
```


> Determinare i nomi dei musei che espongono almeno un quadro di un pittore ancora in vita
```mysql
SELECT NomeMuseo
FROM PITTORE  JOIN Quadro on  Nome = NomePittore
WHERE DataMorte IS NULL
```

# Es Festa
* `FESTA(Codice, Costo, NomeRistorante)`, codice pk.
* `REGALO(NomeInvitato, CodiceFesta, Regalo)`, Nome Invitato pk.
* `INVITATO(Nome, Indirizzo, Telefono)` Nome pk.

> Determinare per ogni festa il nome dell'invitato piu generoro, ovvero quello che ha portato il maggio numero di regali
```sql
SELECT R.CodiceFesta, R.NomeInviato
FROM Regalo AS R
GROUP BY R.CodiceFesta, R.NomeInvitato
HAVING COUNT(*) >= ALL (
	SELECT COUNT(*)
	from Regalo AS R0
	WHERE R0.CodiceFesta = R.CodiceFesta
	GROUP BY R0.NomeInvitato );
```

> Determinare nome invitati che hanno partecipato alla festa piu costosa

```sql
SELECT DISTINCT R.NomeInvitato
FROM Regalo as R
WHERE R.CodiceFesta IN (
	SELECT F.Codice
	FROM Festa as F
	WHERE F.Costo >= ALL(
		SELECT F1.Costo
		FROM Festa as F1lll
	)
)
```

# Es Autobus
* `AUTISTA(CF, Nome, Cognome, Eta, NumAutobus)`
* `PERCORRE(NumAutobus, NomeStrada)`
* `STRADA(NomeStrada, Lunghezza, Pedonale)`, dove pedonale e' valore booleano.

> Determinare coppie di autisti che guidano lo stesso autobus.

```sql
SELECT A1.CF as CF1, A2.CF as CF2
FROM AUTISTA AS A1 JOIN AUTISTA AS A2 ON A1.NumAutonus = A2.NumAutobus 
```

> Determinare gli autobus che percorrono tutte le strade pedonali
> ed eventualmente anche altre strade

```sql
SELECT NumAutobus
FROM PERCORRE JOIN STRADA ON NomeStrada = PERCORRE.NomeStrada
WHERE Pedonale = true
GROUP BY NumAutobus
HAVING COUNT(*) = (
	SELECT COUNT(*)
	FROM Strada
	WHERE Pedonale = True
)
```

> Conteggiare autobus che percorrono solo strade pedonali

```sql
SELECT Count(DISTINCT NumAutobus)
FROM PERCORRE
-- conta il numero di autobus distinti che passano per strade pedonali
WHERE NumAutobus NOT IN (
	-- ottieni strade non pedonali
	SELECT NumAutobus
	FROM PERCORRE AS P JOIN STRADA AS S ON P.NomeStrada = S.NomeStrada
	WHERE Pedonale = False
);
```

* `AUTISTA(CF, Nome, Cognome, Eta, NumAutobus)`
* `PERCORRE(NumAutobus, NomeStrada)`
* `STRADA(NomeStrada, Lunghezza, Pedonale)`, dove pedonale e' valore booleano.
