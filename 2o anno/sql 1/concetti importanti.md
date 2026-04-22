# Query
* Quando voglio in `WHEN` se un valore e' `NULL` devo usare la keyword `IS`
* `GROUP BY` opera su tutti e soli gli attributi selezionati da `SELECT` (tranne che per le keyword insiemistiche: `AVG` ecc...)
* `GROUP BY` opera su gruppi definiti su certi attributi! fare `SELECT S.Nome, S.Cognome, V.dataOra` e poi `GROUP BY S.Nome, S.Cognome, V.dataOra` e' inutile perche' ragruppo ma non faccio operazioni.
* `GROUP BY attr1` vuol dire che tutti gli `attr1` saranno condensati in un unica riga, `count, max, avg` possono essere usati per istanziare i valori su quella riga. 
*  `LIMIT 1` per selezionare solo il primo elemeto dal risultato: utile per ottenere il piu grande valore o piu piccolo dopo averli ordinati.
* Usare **vincoli di integrita**: `CHECK( condizione )` se richiesto dalla realta di interesse. `condizione` puo' essere una query.
* Vincoli: `not null, unique, primary key, check`
* `default <value | null>`
* `foreign key (...) references (...) on delete|update cascade| set null | set def | no action`. 
* `between ... and ...`
* `union | except | intersect`
* `avg, count, max, min, sum`
* **Nidificazione**: `all | any`, `in | not in` e `= any | <> any`. `>= all`, `exists`
* Nelle query devo fare attenzione alla richiesta. Es: nell'ultimo anno, nell'ultimo mese e aggiungere dunque i giusti controlli in `WHERE`.
* `with check option`: si usa con le viste per permettere modifiche alla vista
* `NULLIF(p1, p2)` confronta p1 e p2, se vere allora 
* `Coalesce`: ritorna primo valore non nullo, es: `coalesce(Nome, 'Nessuno')`
* `extract(year from ...), current_date`


# Normalizzazione e dipendenze
* **Anomalie di inserimento**: non e' possibile inserire elemento perche' dipendente da un'altro. (Es. per aggiungere un progetto devo aggiungere anche un nuovo dipendente)
* **Anomalia di cancellazione**: eliminare un oggetto comporta eliminare altri oggetti.
* **Anomalia di modifica**: modifcare un'oggetto comporta modifica di piu oggetti
* *Le tabelle non dovrebbero ammettere valori* NULLI!
* Quando si guardano le forme normali `1NF`, `2NF`, ecc...  bisogna considerare quali attributi potrebbero costituire la chiave primaria.

* `1NF`: No ridondanze, tutti attributi sono atomici. 
* `2NF`: Non ci sono dipendenze da una **chiave parziale**
* `3NF`: $X \to Y$, allora $X$ e' superchiave oppure $Y$ e' attributo primo.
* `Boyce-codd`: ogni attributo dipende dalla superchiave.
* $X \twoheadrightarrow Y$. *Si ha quando per ogni valore di $X$ allora l'attributo $Y$ ha un valore che appartiene ad un insieme di valori.*
* `4NF`: Dipendenze Multivalore: se per ogni $X \twoheadrightarrow Y$, e' BCNF ed $X$ superchiave per tutte le dipendenze, e inoltre per $X \twoheadrightarrow Y$ e $X \twoheadrightarrow Z$, $Y$ e $Z$ devono quanto meno essere dipendenti l'uno dall'altra, altrimenti avrei informazione ripetuta.
* `5NF`: Dipendenze Join. Vale se `4NF` e se posso ottenere piu sotto tabelle che se joinate mi danno quella originale senza perdita o aggiunta di dati.
* se $Y \subseteq X$, allora $X \to Y$. 
* $XZ \to YZ$ se $X \to Y$.
* se $X \to Y$ e $YZ \to V$ allora $XZ \to V$.

* 1. Dato $R(A,B,C,D,E)$ con le seguenti dipendenze $F$: $A\to B, B \to C, A \to D, D\to E$.
	a. Quali DF si possono dedurre da $F$? Si possono dedurre $A\to C$ ed $A \to E$. 
	b. $A\to E$ e' valida? $A\to E$ e' valida perche' deducibile da $F$
	c. Calcola la chiusura di $A$. La chiusura di $A^+ = \{A,B,C,D,E\}$

2. Dato $R(X,Y,Z,W)$ e le dipendenze $F: X \to Y, Y \to Z, X \to W$ allora:
	a. Calcola $X^+$. $X^+ = \{X, Y, Z, W\}$.
	b. $X$ e' chiave? $X$ e' chiave perche' determina tutti gli altri attributi.
	c. Quali altri insiemi possono essere chiavi? Solo $X$ e' chiave.

3. Con $AB \to C, C\to D, D \to E, A\to C$.
	a. Porta in forma canonica le dipendenze. Ogni dipendenza ha gia un attributo a destra.
	b. Elimina le ridondanze.  $A \to C$ e' ridondante, e' contenuta in $AB \to C$,
	c. L'insieme minimale e': $AB \to C, C \to D, D \to E$.
4. $R(A,B,C,D)$ e $DF: A \to B, AB \to C, C \to D$.
	a. Trovare la chiusura di $A$. $A^+= \{B, C, D\}$.
	b. $\{A\}$ e' la chiave candidata, determina tutti i campi.
	c. e' gia minimale

