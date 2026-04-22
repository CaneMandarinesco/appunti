
## 1. Introduzione al Ranked Retrieval
**modelli distribuzionali della semantica lessicale**: *un modo per rappresentare il **significato** delle parole in modo automatico a partire dai **testi**.*

**Distributional Hypothesis**: *parole con significati simili tendono ad apparire in contesti simili*

**significato di una parola**: e' dato dall'insieme delle parole insieme a cui questa compare. guardiamo i contesti in cui compare.

**contesto di una parola**: e' dato dalle parole a sinistra e a destra.

**distribuzione dei sinonimi**: su larga scala i sinonimi appaiono negli stessi contesti, dunque hanno significato simile.

**Esempio**: se nel corpus compaiono spesso `cane` e `gatto`, allora posso concludere che `cane` e `gatto` nel contesto del corpus sono semanticamente simili.

**significato di una parola**: e' un concetto **statistico**.
## tipi di relazioni tra parole
**3 tipi di relazioni tra parole**:
1. **relazioni sintagmatiche**:  relazione in presenza, dipende dalla vicinanza e dall'ordine nella frase. 
	1. **Come si identifica?**: guardo a sinistra e a destra della parola, *cerco una concorrenza nella stessa frase.*
	2. **ES**: `il lupo e' affamato`, allora `lupo` e `affamato` sono in relazione sintagmatica
2. **relazioni paradigmatiche**: parole che possono essere scambiate.
	1. **non concorrenza**: non compaiono insieme, perche' condividono lo stesso contesto linguistico
	2. `il lipo e' affamato`, ma `affamato` potrebbe essere cambiato con `assetato`.
	3. **ci interessa per identificare**: parole semanticamente vicine.
3. **Relazioni topiche**: parole legate allo stesso **argomento**
	1. **Come si identifica**? Quando due parole compaiono spesso su un **topic**.

**word spaces**, spazio vettoriale:
* **vettore numerico**: rappresentare con un vettore ogni parola.
* **come si costruisce**? guardo parole che appaiono in contesti simili.
* **come si misura la similarità**? tramite le distanze nello spazio

**topic space**: 
* **cos'e' il contesto**? e' l'intero documento.
* **relazione semantica**: due parole sono nello stesso documento.
* usa tipicamente **TF-IDF**.

**Perche' Ranked retrieval?**
- **Problemi della Ricerca Booleana:**
    - **Feast or Famine (Abbondanza o Carestia):** a causa di OR e AND le query danno **troppi** risultati o nessun **risultato**.
    - **Complessità per l'utente:** gli utenti normali non la sanno usare.
![[Pasted image 20260327101429.png]]

- **Soluzione - Scoring:** Assegnare un punteggio (score) a ogni coppia query-documento, solitamente nell'intervallo [0,1], per misurare quanto bene corrispondano.
- **Vantaggi:** posso ritornare all'utente solo la **top 10** risultati.

**ordinati**: voglio i risultati ordinati per rilevanza.

## 2. Prime Misure di Similarità e Rappresentazione
 
- **Coefficiente di Jaccard:** $J(A,B)= \frac{|A \cap B|}{|A \cup B|}$
    - **Limiti:** Non considera la frequenza dei termini e non tiene conto della rarità dei termini nella collezione.
    - **Cosa vorrei**: i match in un documento su termini poco frequenti dovrebbero essere più informativi.
- **Da Binary Incidence Mat a Count Mat:**
    - **Binary Incidence Matrix:** Solo presenza/assenza (0/1).
    - **Count Matrix:** Contiene il numero di occorrenze (frequenza grezza).
![[Pasted image 20260327103347.png]]

- **Modello Bag of Words:** I documenti sono trattati come "sacchi di parole", ignorando l'ordine dei termini.
	- *John is quicker than Mary* e *Mary is quicker than John* sono rappresentati allo stesso modo.
	- **pro**: semplifica il problema
## 3. Ponderazione dei Termini: TF-IDF
Il sistema di pesatura più noto per bilanciare l'**importanza locale** e **globale** di un termine.
### A. Term Frequency (tf)
* $\text{tf}_{t,d}:=\text{numero di volte in cui } t \text{ occorre in } d$.
* **problema**: la rilevanza di un termine non dovrebbe aumentare al crescere di $\text{tf}_{t,d}$.
- **Log Frequency Weighting:** La rilevanza non aumenta proporzionalmente alla frequenza grezza. 
![[Pasted image 20260327124833.png]]
### B. Inverse Document Frequency (idf)
- **Principio:** I termini rari sono più informativi di quelli comuni.
- **Document Frequency (dft​):** Numero di documenti nella collezione che contengono il termine t.
![[Pasted image 20260327125338.png]]

### C. Peso tf-idf Finale
![[Pasted image 20260327125630.png]]
- Il peso aumenta con la frequenza nel documento e con la rarità nella collezione.
## 4. Il Modello a Spazio Vettoriale (VSM)

- **Concetto:** Ogni documento e ogni query sono rappresentati come vettori in uno spazio reale a ∣V∣ dimensioni (dove ∣V∣ è il numero di termini nel vocabolario).
    
    +1
    
- **Similarity vs. Distance:**
    
    - La **distanza Euclidea** è una cattiva misura perché influenzata dalla lunghezza dei documenti (documenti con la stessa distribuzione di parole ma lunghezze diverse risulterebbero distanti).
        
        +1
        
    - Si preferisce la **similarità del coseno**, che misura l'angolo tra i vettori.
        
        +1
        

### Similarità del Coseno

- **Normalizzazione:** Dividere i componenti per la norma L2​ del vettore mappa i documenti sulla sfera unitaria, eliminando l'influenza della lunghezza.
    
- **Formula:** 
- Per vettori già normalizzati, il coseno equivale semplicemente al prodotto scalare: ∑qi​⋅di​.