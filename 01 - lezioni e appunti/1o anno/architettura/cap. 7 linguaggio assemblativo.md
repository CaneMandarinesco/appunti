due tipi di linguaggi:
* assemblativo, ogni istruzione corrisponde ad un istruzione ISA, la compilazione viene eseguita dall'assemblatore
* alto livello: la compilazione viene eseguita da un compilatore

### perche' assembly?
esistono dei sistemi dove l'efficienza e' cruciale. per esempio i sistemi operativi real-time (come macchinari per la dialisi) dove le procedure devono essere eseguite nel minor tempo possibile, in modo efficiente, e usando meno richieste di I/O memoria possibile.  linguaggi come C/C++ riescono a essere efficienti ma comunque scrivere direttamente codice assembly rimane sempre piu efficiente.  

e' particolarmente indicato anche per:
* chip RFID
* driver
* chip
* controllori

### formato istruzioni linguaggio assemblativo
le istruzioni seguono di solito il seguente formato:
* etichetta
* op code
* operandi
* commenti

le etichette sono utili per 

### codice operativo espandibile
posso avere piu tipi di istruzioni:
* 3 operandi
* 2 operandi
* 1
* nessuno

se ho 3 operandi il codice operativo e' di questo tipo
```
xxxx xxxx xxxx xxxx
```
se ho 2 operandi inserisco nel primo byte una sequenza `ESCAPE`. indica che il codice operativo effettivo si trova nel prossimo byte, quindi:
```
ESCAPE xxxx xxxx xxxx
```

se ho 1 operando
```
ESCAPE ESCAPE xxxx xxxx
```

quindi ho un byte per l'op code e un'altro per l'operando.
se non ho operandi:
```
ESCAPE ESCAPE ESCAPE xxxx
```
