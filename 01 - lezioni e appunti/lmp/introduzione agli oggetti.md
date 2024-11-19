Alan Key, 5 concetti chiave su Smalltalk (uno dei primi linguaggi ad oggetti):
 1. *Everything is an object*: alle variabili posso chiedere di eseguire delle operazioni su se stesse.
 2. *Un programma e' un insieme di oggetti che comunicano tra loro grazie a messaggi*: dove per messaggio si intende una chiamata ad un metodo
 3. *Ogni oggetto ha la sua memoria*
 4. *Ogni oggetto ha un tipo*, ovvero e' istanza di una **classe** (o tipo)
 5. *Tutti gli oggetti di un tipo particolare possono ricevere gli stessi messaggi*, per esempio un oggetto `circle`, riceve gli stessi messaggi dell'oggetto `shape`, dato che il cerchio e' una forma.

Una descrizione più precisa e breve:
>[!note] *oggetto*
> Un oggetto ha: **stato**, **comportamento** e una **identita**.

Una classe e' un `abstract data type`, e si comporta come una qualsiasi variabile, perche' per esempio, anche il tipo `float` ha delle **funzionalita**', un **comportamento** e un **identita**.

Bisogna creare una mappatura tra: oggetti del `problem-space` e oggetti della `solution-space`, in modo da manipolare questi per arrivare alla soluzione. 

Con le **interfacce** posso definire le richieste che posso fare agli oggetti:
```java
Light lt = new Light();
lt.on(); // il metodo on, fa parte dell'interfaccia
/* ... */
lt.off();
```

ora pero' bisogna associare ad ogni richiesta un metodo: **implementazione**.

> posso pensare agli oggetti come dei `service provider`. questo modo di pensare aumenta la `coesione` del software. E' meglio avere tanti oggetti che fanno piccole cose, piuttosto che pochi oggetti che fanno tutto.

Possiamo identificare nella OOP: i `class creators` (chi crea data types) and `client programmers` (chi usa i data types). Il class creator crea una classe che fa qualcosa e nasconde la sua implementazione all'utente.  Inoltre bisogna definire dei confine tra `creators` e `programmers` grazie alle chiavi: **public, private e protected**. 

Questi **access specifiers**: determinano chi puo' usare la definizione che segue. 

## riusare le classi

>[!note] *composition*
> usare classi per costruire altre classi

Per esempio: una classe macchina nella sua definizione potrebbe includere un oggetto motore.

Gli oggetti membri di una classe, tipicamente sono privati, cosi da non disturbare il codice per il cliente. 
Il numero di oggetti membri di un oggetto puo' crescere anche **dinamicamente** a run-time (questo comportamento non vale per i metodi, che vengono identificati a compile-time).

### Inheritance
>[!note] inheritance
> prendo una classe, la "clono", e faccio delle modifiche. La classe di partenza e' la **base**, quella clonata e' la classe **derivata**, o **child**. I cambiamenti apportati alla base si rifletto sulla classe derivata

Per esempio data una classe `Shape`, delle classi derivate potrebbero essere: `Square`, `Triangle` e `Circle`.

>[!warning]
> Ogni classe derivata ha lo stesso tipo della classe base: un oggetto `Square` e' di tipo `Shape`

in una classe derivata posso aggiungere metodi, o modificare quelli della classe base grazie all'`override`:
* se mi limito a fare l'override, allora posso dire che `Square` e' di tipo `Shape` ed eventualmente salvare `Square` in una variabile di tipo `Shape`
* se aggiungo metodi ed effettuo la sostituzione, allora non posso piu accedere ai nuovi metodi di `Square`

In un compilatore OOP generalmente, gli indirizzi dei metodi degli oggetti, vengono determinati a **run-time** (**late-binding**), ovvero il Polimorfismo. Questo in altri linguaggi come c++ viene fatto  con la keyword `virtual`.

Quando mi riferisco un oggetto, per esempio di tipo `Circle`, quando la sintassi si aspetta il tipo `Shape` allora avviene un **upcasting**. La chiamata dei metodi giusti, viene gestita a run-time a seconda se l'oggetto sia effettivamente un `Circle` o una `Shape`. 
### Gerarchia a radice singola
In Java, tutte le classi sono figlie della classe `Object`. In C++ questo non succede, anche perche' cosi C++ e' retro compatibile con C, ma a quanto pare spesso conviene creare la propria classe radice anche in C++.

In Java, so che posso fare certe operazioni, con tutti i tipi di oggetti!

### Container
in java, esiste un container (un oggetto) che gestisce l'allocazione di memoria per gli altri oggetti, e ne detiene i puntatori.
Esempi di container:
* Liste
* Mappe
* Queues
* Trees
* Stacks
* ecc...

### Parametrizzazione
Prima di JavaSE5, tutti i Container gestivano tipi Object. Quindi se passo un tipo `Circle` al mio Container, perdo informazioni sul suo tipo, e quando vado a richiedere il suo indirizzo non sono in grado di effettuare un **downcasting**. Percio' esiste una tecnica chiamata parametrizzazione:
```java
ArrayList<Shape> shapes = new ArrayList<Shape>();
```

ossia nel mio container posso specificare il tipo di dato con cui andro' a lavorare.

### Creazione e distruzione degli oggetti
Java usa l'allocazione dinamica, per la creazione degli oggetti. Infatti in Java si usa l'operatore `new`, che sta a indicare la creazione di un'istanza dinamica dell'oggetto. 

Per quanto riguarda l'eliminazione dei dati in memoria di un oggetto che non viene piu utilizzato, esiste il `Garbage Collector`, che individua questi oggetti, per poi distruggerli. Il Garbage Collector in Java funziona su tutti gli oggetti proprio perche' tutti questi hanno come base la classe `Object`.

### Errori
In Java sei costretto a scrivere codice per gestire gli errori. Un Oggetto si occupa di gestire gli errori.
