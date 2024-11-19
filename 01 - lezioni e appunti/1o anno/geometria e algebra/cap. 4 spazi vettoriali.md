## basi
una base e' un sistema di vettori linearmente indipendenti che generano tutto lo spazio vettoriale $V$. 

---
se $B$ e' base di $V$ vuol dire che una qualsiasi combinazione lineare di vettori in $B$ mi da un vettore $v \in V$: $a_{1}x_{1} + \dots + a_{x}x_{x} = v$. ma algebricamente spostando il termine $v$ a sinistra: $a_{1}x_{1}+\dots + a_{n}x_{n}-v = 0$ ottengo una relazione di **dipendenza lineare**, 

---
### prop. 4.6
se $B$ e' base di $V$ allora i vettori di $B$ sono linearmente indipendenti e massimali in $V$.  
#### dim.
* per definizione di *Base*, i vettori di $B$ sono **linearmente indipendenti**. data una combinazione qualunque di vettori in $B$ posso ottenere un qualsiasi $v \in V$: $a_{1}x_{1} + \dots + a_{n}x_{n} = v$, ciò algebricamente significa che: $v - (a_{1} x_{1} + \dots a_{n}x_{n}) = 0$, ovvero ho ottenuto una relazione di **dipendenza lineare** aggiungendo alla combinazione lineare il vettore $v$ generato da questa, e quindi $B$ e' massimale
* se ho dei vettori **linearmente indipendenti** e massimali in $V$ per dimostrare che sono una *Base* uso questo lemma
### lemma 4.7
se $B = \{ v_{1},\dots,v_{n} \} < V$ e $span(B)$ contiene dei generatori di $V$. allora $B$ genera $V$.  
#### dim.
se $B$ contiene i generatori di $V$ allora $B$ genera direttamente $V$.  

### teo. 4.8
se $A=\{v_{1},\dots, v_{n}\}$ generatori di $V$, se $B$ sottoinsieme massimale di $A$ di vettori linearmente indipendenti allora $B$ e' base di $V$.  
#### dim.
applico lemma 4.7 e quindi so che $B$ genera $V$, ma i vettori di $A$ si trovano in $B$
si perche' essendo $B$ massimale in $V$ questo mi genera tutti i vettori di $A$