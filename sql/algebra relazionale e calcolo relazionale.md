Sta roba e' praticamente `SQL` ma con la matematica, perché qualcuno ne sentiva il bisogno.

Notazione: $X := \text { attributo}$, $r(X) := \text{ relazione}$

* $\rho_{B_{1}, ..., B_{n} \gets A_{1}, ..., A_{n}}(r(X))$: **==rename==**. 
* $\sigma_{\text{condizione}}(r(X))$: **==selezione==**. Estraggo la riga solo se la condizione e' verificata.
* $\pi_{\text{colonne da proiettare}}$(r(X)): **==proiezione==**. Scelgo quale colonna voglio dalla relazione, ossia un sottoinsieme di $Y$, inoltre potrebbero comparire dei duplicati a seguito di una proiezione! **OSS**: *la proiezione su una super chiave ritorna lo stesso numero di elementi di $X$.*
* $r_{1}\bowtie r_2$: **==join naturale==**. 
	* **incompleto**: se in $A_{i}\in r_{1}, r_2$ non si trova una corrispondenza per fare un join.
* $\pi_{X_1}(r_{1} \bowtie r_{2})$ ==semi-join==: come il join naturale, ma ritorna soltanto le righe di $r_1$ che partecipano in join con $r_2$
* $r_{1} \bowtie_{\text{condizione per il join}} r_{2}$: **==theta join==**. faccio il join se la condizione e' verificata.
* $r_{1} \bowtie_{\text{LEFT}} r_2$: gli attributi di $r_1$ compaiono sempre anche se non c'e' un join con $r_2$, in tal caso si avra `null` sulla colonna incriminata.
* $r_{1} \bowtie_{\text{RIGHT}} r_2$: similmente a prima...
* $r_{1} \bowtie_{\text{FULL}} r_2$: semplice dai...

# Esercizio Festa
Dato lo schema relazionale:
* `FESTA(Codice, Costo, NomeRistorante)`
* `REGALO(NomeInvitato, CodiceFesta, Regalo)`
* `INVITATO(Nome,Indirizzo,Telefono)`

> Determinare il nome dei ristoranti in cui non c'e' mai stato un invitato di nome marco

* $F: \pi_{\text{NomeRistorante}}(FESTA)$
* $M: \sigma _{\text{NomeInvitato='Marco'}}(REAGALO)$. Seleziona i regali di `Marco`
* $RM : \pi_{NomeRistorante}(M \bowtie_{Codice=CodiceFesta} FESTA)$. Seleziona `NomeRistorante` dalle feste con regali di `Marco`.
* $F-RM$ e' il risultato cercato.

> Determinare il nome degli invitati che hanno partecipato **a tutte le feste** avvenuta nel ristorante `Il Tulipano`.

* $FT = \pi_{Codice}(\sigma_{\text{NomeRistorante = 'Il Tulipano'}}(FESTA))$
* $TUTTI = \pi _{\text{NomeInvitato}}(REGALO) \times \sigma_{CodiceFesta \gets Codice}(FT)$
* $REALI = \pi_{NomeInvitato,CodiceFesta}(REAGALO)$
* $NRIS = \pi_{NomeInvitato}(TUTTI - REALI)$
* $\pi_{\text{nomeInvitato}}(REALI) - NRIS$.

# Esercizio Festa
Schema relazionale:
* `PERSONA(CF, Nome, Cognome, Stipendio)`
* `LAVORA(CFPersona, PIvaDitta, Anzianita)`
* `DIRIGE(CFPersona, PIvaDitta)`
* `DITTA(PIva, Denominiazione, NumeroDipendenti)`

> Nome e cognome dei dirigenti che hanno lo stipendio piu' alto

* $DIR =  \pi_{Nome,Cognome,Stipendio}(DIRIGE \bowtie_{CFPersona = CF} PERSONA)$
* $DIR1 = \rho_{n,c,s \gets \text{Nome,Cognome,Stipendio}}(DIR)$
* $NRIS = \pi _{Nome,Cognome,Stipendio}(DIR \bowtie_{Stipendio < s} DIR1)$ sono i dirigenti che non hanno stipendio massimo.
* Il risultato e' $\pi_{Nome,Cognome}(DIR- NRIS)$.

> Determinare la denominazione delle ditte in cue esiste almeno un lavoratore che percepisce uno stipendio maggiore di quello di almeno un diregente della stessa

Devo trovare lavoratore che percepisce stipendio maggiore della sua stessa ditta

* Raggruppo prima le informazioni che mi servono per i dirigenti: $DIR = \pi_{CF, Stipendio, Denominazione}((PERSONA \bowtie_{\text{CFPersona = CF}} DIRIGE) \bowtie_{\text{PIvaDitta = PIva}} DITTA)$.
* Stessa cosa per i lavoratori: $LAV = \pi_{CF, Stipendio, Denominazione}((PERSONA \bowtie_{CFPersona=CF} LAVORA) \bowtie _{PIva=PIvaDitta} DITTA$
* Faccio il join in base a denominazione e stipendio: $T = LAV \bowtie_{\text{L.Denominazione = D.Denominazione and L.Stipendio > D.Stipendio}} DIR$