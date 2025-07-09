1. Dato $R(A,B,C,D,E)$ con le seguenti dipendenze $F$: $A\to B, B \to C, A \to D, D\to E$.
	a. Quali DF si possono dedurre da $F$? Si possono dedurre $A\to C$ ed $A \to E$. 
	b. $A\to E$ e' valida? $A\to E$ e' valida perche' deducibile da $F$
	c. Calcola la chiusura di $A$. La chiusura di $A^+ = \{A,B,C,D,E\}$

2. Dato $R(X,Y,Z,W)$ e le dipendenze $F: X \to Y, Y \to Z, X \to W$ allora:
	a. Calcola $X^+$. $X^+ = \{X, Y, Z, W\}$.
	b. $X$ e' chiave? $X$ e' chiave perche' determina tutti gli altri attributi.
	c. Quali altri insiemi possono essere chiavi? Solo $X$ e' chiave.

3. Con $AB \to C, C\to D, D \to E, A\to C$.
	a. Porta in forma canonica le dipendenze. Ogni dipendenza ha gia un attributo a destra.
	b. Elimina le ridondanze.  $A \to C$ e' ridondante, e' contenuta in $AB \to C$,
	c. L'insieme minimale e': $AB \to C, C \to D, D \to E$.
4. $R(A,B,C,D)$ e $DF: A \to B, AB \to C, C \to D$.
	a. Trovare la chiusura di $A$. $A^+= \{B, C, D\}$.
	b. $\{A\}$ e' la chiave candidata, determina tutti i campi.
	c. e' gia minimale
