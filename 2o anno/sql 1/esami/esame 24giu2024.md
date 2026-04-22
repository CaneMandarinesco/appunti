# Domanda 1
Mostrare uno schema concettuale ER per la descrizione di una raccolta di ricette di piatti aventi le
seguenti specifiche:
	a. Ogni ricetta ha un identificatore numerico, un nome identificativo (es. spaghetti alla carbonara), una regione di provenienza, descrizione della ricetta in termini dei passi per eseguirla. Ogni ricetta ha una sua tipologia (primo, secondo, contorno, ...),
	b. Un insieme di ingredienti, identificati da un nome e la caratteristica (vegana, vegetale, animale) e una propria unità di misura (etto, litro, cucchiaio, ecc.) in modo tale che ogni piatto richiederà una certa quantità della misura di quell’ ingrediente.
	c. Ogni ricetta è composta dagli ingredienti e le loro quantità. Mostrare lo schema logico/relazionale derivante dallo schema concettuale.
# Domanda 2
Modificare uno schema in modo tale vengano soddisfatte le seguenti specifiche:
	a. Ogni ricetta è abbinabile ad una o più ricette (esempio ad un piatto di carne, con uno o più
	contorni);
	b. Organizzare le ricette in menù. Ogni menù contiene qualche antipasto, qualche primo
	qualche secondo, contorno ed infine dessert.
	Mostrare lo schema logico/relazionale derivante dallo schema concettuale.

![[Drawing 2025-07-08 17.52.27.excalidraw]]

Schema relazionale:
**Ricetta**(id, nomeRicetta, regioneProv, descrizione, tipologia)
* Primary Key: id
**IngredientePartecipa**(idRicetta, nomeIngrediente, quantita)
* Primary Key: (idRicetta, nomeIngrediente)
* Foreign Key: idRicetta su Ricetta e nomeIngrediente su ingrediente
**Ingrediente**(nomeIngrediente, caratteristica, misura)
* Primary Key: nomeIngrediente
**Menu**(nomeMenu, idRicetta, tipologiaInMenu)
* Primary Key: (nomeMenu, ricetta)

# Domanda 3
Scrivere le query SQL che:
> per ogni piatto elenca ingredienti e quantita associate ad unita di misura


```sql
SELECT nomeRicetta, nomeIngrediente, caratteristica, misura, quantita
FROM Ricetta AS R 
	JOIN IngredientePartecipa as IP ON IP.idRicetta = R.id
	JOIN Ingrediente AS I ON IP.nomeIngrediente =  I.nomeIngrediente
GROUP BY nomeRicetta;
```

> Stampare un menu vegano

```sql
SELECT nomeMenu
FROM Menu AS M JOIN Ricetta AS R ON idRicetta = id
	JOIN INGREDIENTE ON ...
GROUP BY nomeMenu
HAVING COUNT(*) = SUM(caratteristica = "vegano");
```

oppure
```sql
SELECT nomeMenu
FROM Menu AS M

EXCEPT

SELECT nomeMenu
FROM Menu AS M JOIN RICETTA AS R ON idRicetta = id 
	JOIN INGREDIENTE ON ...
WHERE caratteristica  <> "vegano"
```

> Stampare un menu Laziale
```sql
SELECT *
FROM Menu

EXCEPT 

SELECT nomeMenu 
FROM Menu AS M JOIN RICETTA AS R ON idRicetta = id
WHERE regioneProv <> "Lazio"
```

# Domanda 4
Riportare tutti i secondi piatti che non contengono uova
* $SP =\sigma_{\text{tipologia="Secondo Piatto"}}(Ricetta)$ \ 
* $SPUOVA = \sigma_{\text{nomeIngrediente="Uova"}}(SP \bowtie_{\text{id=idRicetta}} IngrPart)$
* $QUERY = \pi_{nomeRicetta}(SP) - \pi_{nomeRicetta}(SPUOVA)$

