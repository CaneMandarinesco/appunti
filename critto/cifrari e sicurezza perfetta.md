
# introduzione
**Secret key Crittography** o **Simmetrico**: una *chiave viene concordata per criptare e decriptare*.
**Public Key Crittography** o **Asimmetrico**: chiave pubblica per criptare e chiave privata per decriptare.

**Crittosistema**: $(\mathcal P, \mathcal C, \mathcal K, \mathcal E, \mathcal D)$ 
* deve valere che $\forall K \in \mathcal K$, ed $\forall x \in \mathcal P$, voglio che $d_{K}(e_{K}(x)) = x$

**Shift Cipher**:
* $\mathcal P = \mathcal C = \mathcal K = \mathbb Z_{26}$ 
* $e_{K}(x) = x + K \mod 26$
* $d_{K}(y) = y - K \mod 26$
* **Cifrario Giulio Cesare**: se $k=3$

**Exhaustive Key Search** (eks): Shift Cipher in $\mod m$ con $m$ "piccolo" e' vulnerabile ad eks.

**Sub Cipher**: 
* $K$ e' insieme di permutazioni di 26 simboli.
* $|K| = 26!$. Per tanti simboli, $K$ e' grande.
* $e_{\pi}$, $d_{\pi}$ criptano permutando lettera per lettera.

**Analisi del testo**: Sub Cipher vulnerabile ad analisi delle frequenze.

# Sicurezza Perfetta (Stinson Paterson)
**Probabilita Condizionata**: $P[X|Y] = \frac{P[Y|X] \cdot P[X]}{P[Y]}$.

**Probabilita sui cifrari** (Stinson Paterson): 
* *Fissato $K$, scelto dunque a priori del plaintext.*
* $\mathcal C(K) = \{e_{K}(x):  x \in \mathcal P \}$, con $X$ variabile aleatoria sul Plaintext e $Y$ variabile aleatoria su Cyperhtext
![[Pasted image 20260303011258.png]]

**Perfect Secrecy**: se $P[X|Y]=P[X] \;\; \forall x,y \in \mathcal P \times \mathcal C$ .

**Cosa vuol dire Perfect Secrecy**: conoscere $Y$ non avvantaggia un'attaccante.

**C. Affine non ha Perfect Secrecy**: 
* se $y_{i} = y_{k}$ allora so che $x_{i}=x_{k}$ (e viceversa). 
* Inoltre per messaggi più lunghi di un carattere vale che $|\mathcal K| < |\mathcal P|$
* Per **Teorema 2.5**, $|\mathcal K| < |\mathcal P|$ invalida Perfect Secrecy.

**Permutazione**: da $y$ attraverso analisi delle frequenze risalgo a $x$.

**Shift Cipher su un carattere ha Perfect Secrecy**:
* $K$ uniforme e random su $\mathcal K$
* messaggi di un solo carattere.
* Allora ogni chiave e' equiprobabile dato $y$.


# Sicurezza Perfetta (Boneh Shoup)
**One Time Pad**: crittosistema basato su $\oplus$ (**XOR**).

**Two-Time Pad Vulnerabile**: $c_{1} \oplus c_{2} = (m_{1} \oplus K) \oplus (m_{1} \oplus K) = m_{1} \oplus m_{2}$


**Perfect Secrecy**: sia $\mathcal E$ Shannon Cypher (ossia un cifrario). $\mathcal E$ ha perfect sercecy se
* $\forall m, m_{0} \in M, \forall c \in C$
* **fissato $k$ u.a.r**
* **vale**: $P[E(k,m)=c]=P[E(k,m_{0})=c]$.
![[Pasted image 20260303013537.png]]![[Pasted image 20260303013559.png]]

**Cosa vuol dire Perfect Security**: 
* il cifrato non rivela informazioni sul messaggio originale
* **ossia**: $P[M=m | C=c]=P[M=m]$ (come su Stinson Paterson)

**Teo 2.5**: Sia $\mathcal E = (E,D)$ perfettamente sicuro, allora $|\mathcal K| \geq |\mathcal M|$.
* **dim**: per assurdo $|\mathcal K| < |\mathcal M|$, fissato $c$, allora esiste un messaggio con $P[E(k,m_{1})=c]=0$ ed $m_{0}, k_{0}$ con $P[\dots] > 0$.
![[Pasted image 20260303014248.png]]

**Conseguenza del Teo 2.5**: devo avere più chiavi che messaggi, ma e' un requisito troppo stretto nella realta.
