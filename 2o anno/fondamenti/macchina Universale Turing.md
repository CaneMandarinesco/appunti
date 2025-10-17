La macchina di Turing e' un procedimento che risolve un problema tramite il linguaggio delle quintuple, un'algoritmo in un particolare linguaggio.

Da $P$ possiamo ricavare $\Sigma$ e $Q$, ma non sapere gli stati iniziali e finali.

proviamo a costruire una macchina con le seguenti regole:
* la prima parola rappresenta lo stato $q_0$, seguito da un carattere non in $\Sigma$, per esempio: $-$
* poi troviamo $q_A$, il carattere $-$ e in fine $q_R$.
* abbiamo definito gli stati di $T$ usando una parola.
* e poi di seguito, compariranno tutte le quintuple, scritte come parole, secondo un certo linguaggio

> Abbiamo definito tutta $T$ come una parola, costruita sull'alfabeto $Q \cup \Sigma \cup \{-,<,>,\square\}$, quindi $T$ puo' essere input di un'altra  macchina insieme all'input di $T$.

Riusciamo dunque a definire la macchina $U$ che data qualsiasi macchina $T$ con il suo input la possa eseguire.

*Caratteristiche dei nastri:*
1. $U$ simula macchine ad un solo nastro.
2. **1o nastro**: $p_T$, ossia la parola che descrive $T$.
3. **2o nastro**: input $x$.
4. **3o nastro**: contiene lo stato della macchina.
5. **4o nastro**: contiene lo stato di accettazione, mai modificato.

> Il primo nastro e' tipo cosi: $q_{0}-q_A-q_{R} \times q_{0} - b_{1} - b_{2} - q_{1} - m_{1} + .... + ... + ... +$

*Funzionamento*:
1. $U$ **cerca** le quintuple di $T$ da eseguire sul primo nastro.
2. Se la trovo, **esegui** e torna a 1.
3. Se non la trovo, confronta il carattere sul 3o con quello sul 4o
	1. Se uguali: **accetto**
	2. Se diversi: **regetto**

#### cercare le quintuple
Con la testina posizionata sul primo $<$:
1. muovi a destra di una posizione su $N1$
2. Se $N3$ e $N1$ leggono lo **stesso carattere**:
	1. mi sposto a destra su $N1$ e controllo se $N1$ e $N2$ leggono lo **stesso carattere**
		1. **eseguo**
	2. altrimenti vado alla prossima quintupla, cerco: $<$.
3. Altrimenti vado alla prossima quintupla, cerco: $<$.
#### eseguire
Se ho trovato la quintupla da eseguire allora eseguo e torno al punto uno:
1. mi muovo di due caratteri, per leggere il carattere da scrivere
2. copio
3. mi muovo di due caratteri, leggo lo stato in cui entro
4. **ATTENZIONE**: copio su $N3$ lo stato, non lo ricordo direttamente!
5. mi muovo di due caratteri, leggo il movimento da fare su $N2$.

## piu formalmente..
Con $p_{T} = \omega_{0} - \omega_{1} \times {\omega_{1}}_{1} - {b_{1}}_{1} - {b_{1}}_{2} - {\omega_{1}}_{2} - m_{1} + ...$ 
### stato $q_0$
Copio $\omega_0$ su $N_3$ e $\omega_1$ su $N_4$, poi sposto la testina di $N1$ sul primo carattere a destra di $\times$.

### stato $q_1$
Devo cercare una quintupla compatibile:
$<q_{1,}(x,a,x,y), (x,a,x,y), q_{1}, (d,f,f,f)> \forall x,y \in Q_{T} \land \forall a \in \{0,1,\square\}$$<q_{1}(-,a,x,y), (-a,x,y), q_{\text{statoCorretto}}, (d,f,f,f)> \forall a \in \{0,1,\square\} \land \forall  x,y \in Q_T$
Con la prima verifico lo stato, e eventualmente 
mi sposto sul carattere da leggere con stato $q_{\text{statoCorretto}}$.

**Se poi leggo simboli differenti**, non posso eseguire la quintupla, e mi sposto su $N_1$ fino ad arrivare al prossimo $+$ (stato $q_3$). se il carattere successivo e' $\square$ non ci sono quintuple da eseguire, e procedo per terminare l'esecuzione.

**Se i simboli sono uguali**: mi sposto di due caratteri e vado in $q_{\text{scrivi}}$

### stato scrivi
Esegui la quintupla, leggi il simbolo e scrivilo su $N_2$, poi vai in $q_{\text{cambiaStato}}$.

### stato cambia stato
leggi e scrivi il nuovo stato su $N_3$
### stato muovi
muovi la testina su $N_2$ in accordo su quanto scritto nella quintupla
### stato riavvolgi
ritorna al primo carattere a destra di $\times$, poi ritorna in $q_1$

--- 
La computazione di $U$ rigetta ogni volta che $U$ non trova quintupla da eseguire e su $N_3$ non c'e' scritto lo stato di accettazione di $T$, quinti si  rigetta senza verificare che $T$ abbia effettivamente rigettato.

Pero' abbiamo fatto un'assunzione, ossia che $U$ conosce a priori l'alfabeto $\Sigma_T$ e gli stati $Q_T$, ma non e' cosi. Possiamo pero' codificare gli stati e i simboli in input in binario, cosi $U$ deve saper conoscere solo $\{0,1,+,\times, -, f,s,d\}$ e indichiamo con $b^Q(\omega_1)$ la codifica di uno stato in binario.

Ora il controllo dei simboli e degli stati e' più complesso...
1. devo copiare gli $m$ caratteri che rappresentano $q_0$ su $N_3$ e gli $m$ caratteri di $q_1$ su $N_4$
2. Devo posizionare le testine di $N_3$ e $N_4$ sul primo carattere più a sinistra.
3. Come prima $N_1$ punta al  carattere a destra diu $\times$

Ora per controllare lo stato:
* Su $N_1$ e $N_3$ comincio a leggere e controllo  che i caratteri siano uguale fino a incontrare $-$ su $N_1$ e $\square$ su $N_3$.
* Mi sposto di due caratteri e entro in $q_{\text{statoCorretto}}$ e vedi se leggo la stessa sequenza di caratteri.
.....