
## 1. Introduzione al Ranked Retrieval
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