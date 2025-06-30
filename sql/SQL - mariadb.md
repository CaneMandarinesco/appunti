* `SHOW DATABASES;`
* Pipeline: `from -> where -> select -> order`

# `SELECT`
* Nel costrutto `SELECT ... FROM ...` viene valutato prima il `FROM`.
* `FROM` e' opzionale, es: `select now();` ritorna il timestamp corrente del server.

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
Rimuovi i duplicati dal risultato!