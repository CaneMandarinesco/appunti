# bohm jacopini
ogni algoritmo puo' essere implementato con:
* sequenza
* if statemente
* e iterazioni
# principi fondamentali
**incapsulamento**:  incapsulare comportamento e stato in oggetti riutilizzabili. Posso istanziare quante volte voglio gli oggetti a partire dalla classe che li definisce.
**astrazione**: nascondere i dettagli implementativi degli oggetti, devo solo conoscere la specifica per utilizzarne i metodi
**ereditarieta**: le classi possono ereditare stato e comportamento da altre classi, in modo da riutilizzare il codice.
**polimorfismo**: posso creare oggetti di una stessa classe che si comportano in modo diverso
* **overloading**: posso creare piu metodi con nome identico ma parametri differenti
* **overriding**: posso ridefinire il comportamento di un metodo

**Interfaccia**: e' un contratto, indica quali metodi deve implementare una classe. Se voglio gestire una collezione.

**Stringhe**: sono immutabili, infatti e' una classe porcodio.

**Static**: indica comportamento e stati globali relativi ad una classe. Tutti gli oggetti condividono campi e procedure static.


**Final**: 
* campo: non puo' essere modificato dopo che e' stato assegnato nel costruttore.
* classe: non puo' essere estesa

**Shadowing**: le variabili dentro uno scope possono coprire il nome delle variabili nello scope piu esterno.

**Classi**: 
* **Inner Class**: appartien
* **Static nested class**: non
* **Local Class**: dentro al metodo
* **Anonima**: la crei al volo.

**Annotazioni**: 
1. Ottenere informazioni su una classe e comunicare col compilatore 
2. `@Override`
3. `@Deprecated`
4. `@SuppresWarnings`

**Generics**: parametrizzare i tipi all'interno di classi interfacce e metodi.
* `?` e' la wildcar

---
* `findall`: non fallisce mai. invece di fallire ritorna la lista vuota
* `bagof`: raggruppa per variabili libere
* `setof`: come bagof ordina e rimuove duplicati