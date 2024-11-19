Java e' un programma fatto solo per la programmazione ad oggetti.
Gli oggetti che l'utente manipola in Java, sono delle **reference**.

```java
String s; // ho creato una refernce, ma non controlla nessun oggetto
// se chiamo un metodo di s, ottengo un errore
```

```java
String s = "asdf"; // reference ad un oggetto
// ora s punta ad un oggetto
```

Aree di memoria:
1. Registri
2. **Stack**: si trova nella RAM, SP. Questa aera e' usata per dati di cui si conosce il `lifetime`. Contiene i valori delle variabili dei metodi, gli indirizzi dei metodi, i valori degli stack pointer e program counter ad ogni chiamata, ecc...
3. **Heap**: si trova nella RAM. **General-purpose** pool of memory. Qui dentro si trovano gli oggetti. La gestione di questa area di memoria e' piu lenta dello stack.
4. Area costante.
5. Non-RAM

### tipi primitivi
Questi tipi di dati sono trattati in modo diverso. Questi sono: `boolean`, `char`, `byte`, ecc... . Questi tipi immagazzinano piccole quantità di dati, e non e' conveniente gestirle tramite l'heap ma piuttosto conviene trattare questi dati come facciamo in C/C++. Le variabili di tipi primitivi non sono riferimenti al valore effettivo ma immagazzinano direttamente i dati.

* La grandezza dei tipi di dati e' uguale in tutte le macchine su cui viene eseguito codice Java (ciò non accede per esempio in C).
* i tipi numerici non hanno una versione `unsigned`
* `boolean` non ha una grandezza: e' dato che accetta i valori true o false
* esistono delle classi `Wrapper` per i tipi primitivi.
* `BigInteger` e `BigDecimal` servono per operazioni matematiche ad alta precisione (`int/float`)

### array
gli array in Java garantiscono di essere sempre inizializzati (a valore `null`) e che l'accesso all'array si effettuato dentro i suoi limiti di range. 

### lifetime
primitivi  e oggetti hanno diversi lifetime.

```java
{
	String s = new String("hi");	
} // end of scope, s viene distrutto, ma l'oggetto continua ad esistere.
```

Come faccio a eliminare la stringa `"hi"` dalla memoria se non ho il suo riferimento? Ci pensa il **GarbageCollector**!


> [!note] valori di inizializzazione per le classi
> tutti i tipi primitivi, *appartenenti solo e soltanto ad una qualsiasi classe*, vengono inizializzati ad un valore di default (es: i booleani vengono sempre impostati a `false`)

### `class`
```java
class ATypeName { /* corpo della classe */ }
```

anche se la classe non contiene nulla, posso creare l'oggetto:
```java
ATypeName a = new ATypeName();
```

Una classe puo' contenere:
* `fields`: campi di dati
* `methods`: metodi, *member functions*. Descrivono come fare qualcosa.

Ogni metodo si definisce cosi:
```java
ReturnType methodName(/* arg list */) {
	/* method body */
	return /*qualcosa, se il metodo non e' tipo void*/;
}
```
## building a java program
### static
> [!note] static
> questa keyword indica che la definizione che segue non e' di proprieta' di nessun oggetto in particolare. Anche se non creo un oggetto posso utilizzare quel metodo, o accedere ad un campo

posso riferirmi ad un oggetto statico o a partire dalla reference di un oggetto o dal nome della classe:
```java
class StatiTest{
	static int i;
}

// ...
StatiTest st1 = new StaticTest();
st1.i++;
StaticTest.i++; // questo approccio e' meglio ottimizzato
```

### primo programma stupido
```java
import java.util.*;
// java.lang e' sempre importato in qualsiasi programma

public class Prova {
    public static void main(String[] args) {
        System.out.println("Hello, it's: ");
        System.out.println(new Date());
    }
}
```

> nota: `System.out` e' il nome per l'oggetto statico: `static PrintStream out` che si trova dentro la classe `System`.
> nota: `public` indica che il campo o la classe definita possono essere usate al di fuori del file.
> nota: l'oggetto `Date` creato con `new Date()` viene convertito automaticamente a oggetto `String`
> nota: l'oggetto `new Date()` appena terminata l'esecuzione del print, viene distrutto dal garbage collector.

### compilare
```bash
javac Prova.java # compila per jvm
java Prova       # esegui usando jvm
```

## operatori
in java posso fare:
```java
int a = 5;
System.out.println("a = " + a + ".");
```

dove l'operatore `+` assume il significato di concatenazione di stringhe.

posso anche scrivere:
* `int x = a * -b`
* `++a` e `a++`: pre-incremento e post-incremento.
* `o1.equals(o2)` ritorna `True` se e solo se `o1` e `o2` sono lo stesso **riferimento**. Posso comunque fare un override del metodo `equals` nella mia classe per cambiare il comportamento della funzione.
* l'espressione `test1() && test2() && test3()` non implica necessariamente che vengano eseguite tutte le funzioni di test. se `test2` e' falsa allora non c'e' bisogno di eseguire `test3()`. (**short-circuiting**)
* `Integer.toBinaryString` per convertire un intero in una stringa di 0 e 1.
* L'operatore `+` converte tutto in stringa se e' presente almeno una stringa tra gli operandi.
* Java non ha bisogno di `sizeof` per gestire i dati. Hanno tutti la stessa grandezza indipendentemente dalla macchina su cui sto eseguendo.
* Java ha un ciclo **foreach**: `for(float x : f)`. Per esempio posso fare: `for(char c : "An African Swallow".toCharArray())`
* `printnb` non mette in automatico una nuova linea
* Java non ha l'istruzione `goto`

### inizializzazione e cleanup
Esempio di classe con un costruttore:
```java
class Rock {
	Rock() {
		System.out.print("Rock ");
	}
}
```

il metodo viene chiamato alla creazione dell'oggetto, quindi ogni volta che faccio `new Rock()`, viene stampata una stringa.

Posso avere costruttori e metodi con nome uguale ma parametri diversi:
```java
class Tree {
	int height;
	Tree(){
		height = 0;
		System.out.print("Ho piantato un nuovo albero!");
	}

	Tree(int h){
		height = h;
		System.out.print("Creato un nuovo albero con altezza: " + h);
	}
}
```
posso interpretare il codice come se potessi creare un albero piantandolo (con altezza 0 quindi) oppure aggiungendo un albero gia' cresciuto con altezza `h`.

> i metodi che hanno stesso nome devono avere una lista di parametri **distinta**.
> i metodi che hanno stesso nome ma diverso tipo di ritorno non sono ammessi

### costruttori di default
```java
class Bird { }

...
Bird b = new Bird(); // usa un costruttore di default che crea Java
```

ma se creo almeno un costruttore allora Java non definirà anche quello di default.

### `static`
indica che il metodo non ha la keyword `this`, ovvero un riferimento all'oggetto che sta eseguendo il metodo.

### finalizzazione
Il garbage collector e' in grado di gestire solo oggetti creati usando la keyword `new`. Se invece alloco spazio senza usare `new`? Come dico al garbage collector di occuparsi di questa nuova area di memoria?

Si usa `finalize()`. Prima di spiegare che fa bisogna ricordare che in Java:
1. Gli oggetti potrebbero non essere presi dal garbage collector.
2. Il Garbage Collector non e' un distruttore.
3. Il Garbage Collector si occupa solo di memoria


`finalize()` e' un metodo che viene chiamato dal Garbage Collector prima di distrugger l'oggetto, cosi da permettere all'utente di liberare tutte quelle aree di memoria che possono sfuggire al Garbage Collector. Questo potrebbe succedere chiamando dei **metodi nativi**, ovvero chiamando da Java metodi scritti in altri linguaggi. Sono ufficialmente supportati i linguaggi C e C++.

### cleanup

```java
class Book {
	boolean checkedOut = false;
	Book(boolean checkOut) {
		checkedOut = checkOut;
	}

	void checkIn() {
		checkedOut = false;
	}

	protected void finalize() {
		if(checkedOut)
			System.out.println("Error: checked out");
	}
}

public class TerminationCondition {
	public static void main(String[] args) {
		Book novel = new Book(true);
		novel.checkIn();
		new Book(true); // no prendere il riferimento all'oggetto
		System.gc(); // forza il garbage collector
	}
}
```

il codice mi darà la scritta di errore: "Error: checked out".

### come funziona il garbage collector
in Java  l'allocazione di storage sull'heap per gli oggetti e' molto efficiente, quasi quanto l'allocazione sullo stack in altri linguaggi di programmazione.

>[!warning] da finire!
> finisci di scrivere gli appunti coglione


