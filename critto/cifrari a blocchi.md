
## def. cifrario a blocchi
* $\mathcal P = \mathcal C = \mathcal X$
* $\mathcal K = \{f: \mathcal X \to \mathcal X, \text{ con } f \text{ permutazione}\}$
* $f$ deve essere computazionalmente indistinguibile da una permutazione randomica
* **datablock**: $x \in \mathcal X$

### def. gioco di attacco
**Attaccante**: puo' fare un numero arbitrario di query $x$ al Challenger e ottenere la risposta $y$.

**Challenger**: ad ogni query risponde con:
* $f(x)$ se mi trovo nel mondo $b=0$
* $\text{RandPerm}(x)$ se mi trovo nel mondo $b=1$

**Obiettivo**: l'attaccante deve dunque capire se il Challenger usa una permutazione random o $f(x)$.

## SPN
E' un cifrario a blocchi **iterato**, faccio $N$ round dove eseguo sempre le stesse operazioni e un round $N+1$ leggermente diverso.
* $\mathcal X = \{0,1\}^{lm}$.
	* $l:=\text{numero di bit}$
	* $m := \text{numero di blocchi}$
* $N$ il numero di round
* $\mathcal K = (\mathcal K_{1}, \mathcal K_{2}, \dots, \mathcal K_{n}, \mathcal K_{n+1})$
* $\pi_{S}: \{0,1\}^{l} \to \{0,1\}^{l}$ e' la substitution box.
* $\pi_{P}$: permutazione di $lm$ bit.

**Pro e contro**:
* $\pi_{S}$ e $\pi_{P}$ sono facili da implementare via software e hardware.
* Tante iterazioni $=$ Performance basse
* Pero richiede tante chiavi! Risolvo con un algoritmo di `KeySchedule`.

### Implementazione
Per $N-1$ round:
* `XOR` con la `round-key`.
* $\pi_{S}$ su ogni byte del datablock.
* $\pi_{P}$ sul datablock.
L'ultimo round:
* `XOR` con la `round-key` $N$.
* $\pi_{S}$ su ogni byte del datablock.
* `XOR` con la `round-key` $N+1$.

## AES
E' una SPN definita in questo modo:
* se $\mathcal K$ su $\{ 0,1 \}^{128}$ allora $N=10$ round.
* $\pi_{S}: \{ 0,1 \}^8 \to \{ 0,1 \}^8$
* $\pi_{P}$: implementata da `SHIFTROWS` e `MIXCOLUMNS`.
> [!warning] `XOR` $K_{i}$ come ultima operazione
> Rispetto alla `SPN`, in `AES` faccio lo `XOR` come ultima operazione. All'ultimo round applico una sola chiave.

Si guarda il datablock $x$ come una matrice $4\times4$  di byte:
* 1 colonna e' di 32 bit.

Faccio $N$ iterazioni di $\pi_{AES}$ ed una di $\pi_{AES}'$.
$\pi_{AES}$:
* $\pi_{S}$
* `SHIFTROWS`: su ogni riga del datablock fai lo shift in un certo modo.
* `MIXCOLUMNS`: moltiplica il datablock per una matrice a coefficienti in $\mathbb F_{2^8}$
* `XOR` round key.
$\pi_{AES}' = \pi_{AES} \setminus \{ \text{MIXCOLUMNS} \}$.

Il `keyschedule` per la generazione della chiave prende in input $\mathcal K_{1}$ e applica shift e sbox per generare le chiavi successive.

### precalcolare AES
Posso unire tutti i passaggi di un round di `AES`:
* $T_0, T_1, T_2, T_3 : \set{0,1}^8 → \set{0,1}^{32}$
* $T_{i}[a] = M[i]S[a]$. 
* $M[i]$ riga $\text{i-esima}$ di $M$. 
* Posso usare $T_{i}$ per esprimere ogni riga in base a come vengono shiftate le righe.

## DES
* $\mathcal X = \{ 0,1 \}^{64}$, $\mathcal K = \{ 0,1 \}^{56}$
* $K \in \mathcal K$ viene usata per generare $(K_{1}, \dots, K_{16})$
* $K_{i} \in \{ 0,1 \}^{48}$ ottenute permutando $K$.
* $x_{i}= (L_{i}, R_{i}) = (R_{i-1}, L_{i-1} \oplus f(R_{i-1}, K_{i}))$

$f(A,J), A \in \{ 0,1 \}^{32}, J \in \{0,1\}^{48}$, in ordine fa queste cose
* $E(A): \{ 0,1 \}^{32} \to \{ 0,1 \}^{48}$: espando $A$ permutando e duplicando 
* `XOR` con $K_{i}$
* $\pi_{S}: \{ 0,1 \}^{6} \to \{ 0,1 \}^{4}$
* $\pi_{P}$. chiamata $P$.

Prima di iniziare i round e alla fine applico rispettivamente $IP, IP^{-1}$ permutazioni.
![[Pasted image 20260219172506.png]]

### Meet in the middle
L'attacco mostra che `2DES` non e' piu' sicuro di `DES` rispetto ad un attacco known plaintext/cyphertext

L'attacco funziona cosi:
* Siano $E,D$ usate in `DES`.
* `2DES` e' `DES` ripetuto due volte con chiavi differenti
* Posso ottenere $K_{1},K_{2}$ in $O(|\mathcal K|)$
* Con $x$ datablock e $y = E_{2DES}((K_{1}, K_{2}), x).$
* (1) Allora deve essere vero che: $E(x, K_{1}) = D(y,K_{2})$

Dunque:
* $\forall K_{1} \in \mathcal K$ calcolo $E(x,K_{1})$ e lo butto in memoria.
* $\forall K_{2} \in \mathcal K$ calcolo $D(y,K_{2})$ e lo butto in memoria.
* Cerco una collisione in memoria!

# Attacco ad una SPN

* devo calcolare la tabella degli $N_{L}(a,b)$.
* $N_{L}(a,b) = |\{x: ax  \oplus  bS(x) = 0|\}$
* Se $x,y$ stringhe binarie di lunghezza 4, allora $\epsilon = (N_{L} - 8) / 16$
* Cerco le combinazioni $(a,b)$ per cui lo xor vale 0 perche' mi dice quali input sono uguali all'output $ax = bS(x)$
* **Pilling Up Lemma**: $2^{k-1}\prod_{i} \epsilon_{i} = \epsilon_{1_{i}, 2_{i}, \dots}$ con $\epsilon_{1_{i}, 2_{i}, \dots}$ il bias della variabilie $X_{1_{i}} \oplus X_{2_{i}} \oplus \dots$  con le $X$ indipendenti
* **Bias**: $\epsilon_{X} = P(X = 0) - \frac{1}{2}$. dunque compreso tra 0 e 1.
* Gli $N_{L}(x,y)$ con **bias alto** sono gli input e output da abilitare in una sbox nella mia approssimazione lineare.
* Per attaccare identifico un numero arbitrario e comodo di sbox su cui applicare combinazioni di input e output con bias alto.
* L'output di una sbox dovrebbe essere collegato all'input attivo di un'altra
* Posso ricavare e studiare il bias di una variabile aleatoria $T$ che dipende da:
	* $X_{i}$ del testo in chiaro
	* $U_{i}$ input della SBOX all'ultimo round che dipendono da pochi bit della chiave.
* L'algoritmo che sfrutta $T$ calcola per ogni coppia $(x,y)$ a incrementa per ogni chiave $(L_{1}, L_{2})$ il suo contatore se $T(x,y) = 0$. 
