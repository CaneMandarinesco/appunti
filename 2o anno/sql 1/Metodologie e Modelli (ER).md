* **progettazione concettuale**: produce schema concettuale descrizione  **astratta** della realtà dei dati e delle relazioni tra questi, rappresenta solo il contenuto informativo dei dati, occorre specificare il numero di occorrenze. E' necessario 
* **progettazione logica**:  produce schema logico, come sono organizzati i dati.
* **progettazione fisica**: completo lo schema con indici e organizzazione dei file, come sono registrati in memoria.
Nella **progettazione logica** le tabelle sono rappresentate nel seguente modo: `nomeRelazione(Attr1, Attr2, ...)`
## ER
E' un modello **concettuale** dei dati per descrivere una realtà di interesse.
* **relazione**: indica un legame tra 2 o più entità.
* **attributi**.
* **partecipazione**: opzionale o non-opzionale o a molti. può essere specificata sia per attributi che per relazioni.
* **identificatori**
* **entità deboli**: entità senza chiavi primarie, devono essere messe in relazione con un'altro oggetto per essere completamente significative.
* `IsA`: tipo di associazione dove viene identificata una identità padre ed una figlia, senza ereditarietà multipla.
* **Generalizzazione**: si parla di generalizzazione quando un padre si estende a piu' figli.
