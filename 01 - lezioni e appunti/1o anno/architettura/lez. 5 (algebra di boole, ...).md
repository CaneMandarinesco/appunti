fmul## funzioni booleane

| valore | costante | identita' (buffer) | negazione (inverter) | costante |
| ------ | -------- | ------------------ | -------------------- | -------- |
| 0      | 0        | 0                  | 1                    | 1        |
| 1      | 0        | 1                  | 0                    | 1        |
quindi:
* 0 -> 0011
* 1 -> 0101

> il buffer e' un amplificatore di ampiezza, se un valore e' basso mantiene un voltaggio ma se un valore e' alto ne viene amplificato il segnale.

### transistor
oggetto circolare a 3 pin:
* **base**, agisce da interruttore
* emettitore
* collettore
si puo' immaginare come un interruttore pilotato dalla base, il segnale passa da collettore ad emettitore, e' presente una freccetta che indica il flusso di elettroni. quindi varia da circuito aperto (non c'e' segnale nella base), chiuso se c'e' segnale.  

### negazione
nel circuito $Vin$ viene negato, si implementa grazie al transistor invertendo le tensioni (???).

> [!note] interpretazione del voltaggio
>  esistono dei range per cui un certo range di valori di voltaggio sono riconosciuti come ALTO o BASSO. esiste un range di valori intermedi in cui non si può determinare il valore.

### possibili funzioni booleane binarie
quelle che ci interessano sono: **OR** e **AND**.  facciamo un esempio con 3 variabili in ingresso ($2^3 = 8$ combinazioni).  per creare una tabella con i valori possibili di A,B,C faccio cosi:
* per dividere i casi scelgo di dividere i casi in 2: quelli che hanno $A=1$ e quelli che hanno $A=0$
* scrivo le combinazioni di B e C

un $OR$ si scrive come $+$ dato che ha delle proprieta' simili all'addizione e l'and come $\cdot$ (similmente).  
* $OR$: e' sufficiente che uno dei bit sia $1$ per avere $1$ in uscita
* $AND$: tutti i bit in entrata devono essere $1$ per avere $1$ in uscita

> nota: $AND$ e $OR$ si completano

invece $NAND$ e $NOR$ sono negazioni dei rispettivi operatori (graficamente un pallino all'uscita). le formule sono:
* $AND = A \cdot B$
* $OR = A + B$
* $NAND = \overline{A \cdot B}$
* $OR = \overline{A + B}$
...
### proprieta' dell'algebra di boole
ha delle proprieta' simili all'algebra tradizionale.
* identita: $1 \cdot A = A$ e $0 + A = A$ (rispettivamente AND e OR)
* null: $0 \cdot A = 0$ e $1+A = A$ 
* $A \cdot A = A$ e $A + A = A$
* $A \overline A = 0$ e $A + \overline A = 1$
* leggi di de morgan
* proprieta' commutativa
* proprieta' distribuitiva
* ...

### come verificare la validita di un equivalenza?
esempio: 
$$
A + AB =A
$$

* uso le proprieta' algebriche: $A+AB = A \cdot (1+B) = A$
* uso la tavola della verita: costruiamo due tavole della verita' e  valuto se le due formule suono uguali confrontando i valori in uscita delle tabelle.
* utilizzo la logica e le definizioni: $A$ e' un termine che compare in entrambi i membri dell'$OR$. la presenza di $A$ nell'AND riduce $B$ ad $A$, quindi il valore di $B$ e' superfluo. (corso di logica e reti logiche).

> [!note] ordine di esecuzione delle operazioni
> 1. operatori unari
> 2. operatori binari
> 3. and
> 4. or

esercizio in aula:
$$
A + \overline A B = A + B
$$
$$
A+ \overline A B = (A+ \overline A)(A+B) = A+B
$$
usando la proprieta' distriutiva rispetto all'or.


### trasformare tabella di verita' a espressione logica

| A   | B   | X   |
| --- | --- | --- |
| 0   | 0   | 0   |
| 0   | 1   | 1   |
| 1   | 0   | 1   |
| 1   | 1   | 0   |
$X = (A+B)(\overline A + \overline B)$.  

> con 3 elementi (NOT, AND, OR) dell'algebra posso costruire qualsiasi circuito. ma possiamo fare di meglio.

usando NAND e NOR pero' posso creare le altre porte logiche? (???).  

* con due NAND ($\overline {A \cdot A}$) di fila ottengo un inverter.
* con un NAND($\overline {A \cdot B}$) e un'altro NAND($\overline {A \cdot B} \cdot \overline {A \cdot B}$)


> #chiedere cos'e' un multiplexer?