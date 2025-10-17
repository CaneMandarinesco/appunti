## Problemi della Ridondanza

Le **ridondanze** nei database relazionali causano:
- Spreco di spazio.
- Maggiore complessità negli aggiornamenti.
- Rischio di **incoerenze** nei dati.
### Soluzione: Analisi delle Dipendenze Funzionali
Una **dipendenza funzionale** è una relazione tra insiemi di attributi:  
$X \to Y$ significa che se *due tuple concordano sui valori di $X$, allora devono concordare anche sui valori di $Y$.*

---
## Forme Normali

Le forme normali sono livelli successivi di qualità nella progettazione di schemi relazionali. Servono a **eliminare la ridondanza** e **preservare la consistenza**.

### **Prima Forma Normale (1NF)**

Una relazione è in 1NF se:
- Ogni **attributo è atomico** (non contiene insiemi o liste).
- Ogni riga ha un **solo valore per ogni attributo**.
- Esiste una **chiave primaria**.

> Esempio di violazione: un attributo `Figli a carico` contenente una lista (`[Anna, Marco]`) viola la 1NF.

---

### **Seconda Forma Normale (2NF)**
Una relazione è in 2NF se:
- È in **1NF**.
- Non contiene **dipendenze funzionali parziali**, ovvero:  
    per ogni dipendenza $X \to A$, o $X$ è **superchiave**, oppure $A$ non dipende **solo da una parte di $X$** (nei casi di chiave composta).
---

### **Terza Forma Normale (3NF)**

Una relazione è in 3NF se:
- È in **2NF**.
- Per ogni dipendenza funzionale $X \to A$ vale almeno una delle seguenti:
    - $X$ è **superchiave**.
    - $A$ è **attributo primo** (cioè fa parte di qualche chiave candidata).
---

### **Forma Normale di Boyce-Codd (BCNF)**
Una relazione è in **BCNF** se:
- È in **3NF**.
- Per ogni dipendenza $X \to A$, $X$ è una **superchiave**.

> BCNF è più restrittiva della 3NF, e non sempre è possibile raggiungerla **senza perdita di dipendenze**.

---
## Decomposizione e Preservazione
Una **buona decomposizione**:
- È **senza perdita di informazioni**: il join delle tabelle decomposte restituisce la relazione originale.
- **Preserva le dipendenze funzionali** (quando possibile).
> Ogni relazione decomposta dovrebbe contenere una **chiave primaria**.

---
## Assiomi di Armstrong

Insieme minimo di regole per **derivare nuove dipendenze funzionali**:

1. **Riflessività**: se $Y \subseteq X$, allora $X \to Y$
2. **Aggiunta (Augmentazione)**: se $X \to Y$, allora $XZ \to YZ$
3. **Transitività**: se $X \to Y$ e $Y \to Z$, allora $X \to Z$
### Altri derivabili:

- **Unione**: se $X \to Y$ e $X \to Z$, allora $X \to YZ$
- **Decomposizione**: se $X \to YZ$, allora $X \to Y$ e $X \to Z$
- **Pseudotrasitività**: se $X \to Y$ e $YW \to Z$, allora $XW \to Z$

---
## Chiusura di un Insieme

La **chiusura di $X$ rispetto a $F$**, indicata $X^+_F$, è l'insieme di tutti gli attributi **determinabili** da $X$ usando $F$.

> $X$ è **superchiave** se $X^+ = U$ (tutti gli attributi dello schema).

---

## Minimizzazione delle Dipendenze Funzionali

### Equivalenza tra insiemi di dipendenze

Due insiemi di dipendenze $F_1$ e $F_2$ sono **equivalenti** se:

- Ogni dipendenza in $F_1$ è implicata da $F_2$, e viceversa.
    

### Dipendenza **ridondante**
Una dipendenza $f \in F$ è **ridondante** se $F - {f}$ implica comunque $f$.

### Insieme **minimale** o **ridotto**

Un insieme $F$ è **minimale** se:

- È **non ridondante**.
- Ogni dipendenza ha **un solo attributo** a destra. (non va bene $X \to ABC$, ma invece $X \to A$, $X \to B$ ...).
- Nessun attributo nel lato sinistro di una dipendenza è ridondante.

---

## Riepilogo Forme Normali

|Forma Normale|Requisiti principali|
|---|---|
|**1NF**|Attributi atomici, chiave primaria|
|**2NF**|Nessuna dipendenza parziale|
|**3NF**|Nessuna dipendenza transitiva su attributi non primi|
|**BCNF**|Tutte le dipendenze sono da superchiavi|