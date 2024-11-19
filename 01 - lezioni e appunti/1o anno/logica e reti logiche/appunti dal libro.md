> a beginners guide to mathematical logic
# Insiemi Infiniti
Un insieme infinito e' numerabili (?) se i suoi elementi possono essere messi in corrispondenza biunivoca con l'insieme dei numeri interi positivi.

La domanda che si pose Cantor e': tutti gli insiemi infiniti sono numerabili?

#### caso di studio\
immagina che tu ed io siamo immortali. Scrivo un numero intero positivo su un pezzo di carta, e ogni giorno hai un solo tentativo per indovinarlo.
La strategia vincente per questo gioco sarebbe scegliere $1,2,3,\dots,n$ (giorno per giorno) dove $n$ e' il mio numero vincente, prima o poi arriverò ad $n$.

Ora se invece io scelgo un numero intero (potrebbe essere sia positivo che negativo)? Potrei scegliere i numeri in questo modo: $a_{k} = 1, -1, 2, -2, 3, -3, \dots, n$.

E se invece scelgo una frazione: $\frac{x}{y}$? Che strategia posso adottare per indovinare prima o poi il numero?\

In quasi tutti i casi mostrati sul libro, esiste sempre una soluzione e risolvendo questi semplici problemi non abbiamo fatto altro che mettere in corrispondenza biunivoca insiemi infiniti con dei numeri naturali.


### arrivamo al dunque
> l'insieme di tutti gli insiemi dei numeri positivi interi non e' numerabile.

Consideriamo un libro di $n$ pagine. Su ogni pagina troviamo un sottoinsieme di numeri naturali. Se sul libro ci sono tutti i sotto insiemi dei numeri naturali allora il libro vince un premio.

Questo tipo di problema e' stato trattato nel corso quando abbiamo parlato del problema del barbiere!

### barbiere
In una citta' c'e solo un barbiere che rade tutti e solo quelli che non si radono da soli. Chi rade il barbiere?
* il barbiere non si puo' radere da solo
* nessun'altro puo' radere il barbiere (perche' ce n'e' solo uno)
# logica del primo ordine (cap. 8)
Ogni persona in questi problemi puo' essere o di tipo **True** (dice la **verita**) o **False** (dice il **falso**). (oppure qualcuno puo' essere ***stronzo***).
### Problema 1
Visito un'isola e chiedo ad ogni persona su questa di dirmi qualcosa sul tipo di tutte le persone che ci abitano. Tutti dicono la stessa cosa: "siamo dello stesso tipo". Cosa posso dire del tipo di tutti gli abitanti?

Se tutti gli abitanti dicono la stessa cosa, allora siamo sicuri che o sono tutti True o tutti False. Dato che tutti dicono che sono tutti dello stesso tipo, allora sono tutti tipo True.

### Problema 2
E se invece tutti dicono: "Alcuni sono di tipo T e altri di tipo F". Che tipo di abitanti ci sono?

Qui invece tutti gli abitanti sono di tipo False, perche' se dicono tutti la stessa cosa, allora devono essere tutti dello stesso tipo, non puo' essere che alcuni sono T e altri F.

### Problema 3
Adesso per qualche motivo ci interessa capire chi fuma tra gli abitanti. Tutti dicono la stessa cosa: "Tutti i tipi True fumano". Quanti tipi True e False ci sono? e quanti fumano?

Per assurdo, se tutti fossero di tipo False vuol dire che esiste un tipo True che non fuma, ma questo sarebbe impossibile. Allora sono per forza tutti True, e tutti fumano.

### Problema 5
Se gli abitanti sono tutti dello stesso tipo e dicono: "Se io fumo allora tutti fumano". Cosa posso dedurre?

Se fossero tutti False, allora vuol dire che $x$ fuma e tutti gli altri non fumano, il che e' una contraddizione perche' per persona $y$ allora $x$ non dovrebbe fumare . Se fossero tutti True non ci sono contraddizioni: $x$ fuma e tutti gli altri anche. se $y$ fuma allora e' lecito che anche $x$ fumi.
### Problema 6
Tutti sempre dello stesso tipo e dicono: "Se qualcuno fuma, allora io fumo".

Se fossero tutti di tipo False vuol dire che esiste $x$ che fuma, ma $y$ (la persona che pronuncia la frase) non fuma. Ma vale lo stesso per $x$! Infatti se esiste $z$ che fuma allora $x$ non fumerebbe (questa volta stiamo chiedendo alla persona $x$).

Se fossero tutti ti tipo True vuol dire che tutti fumano, e non ci sono contraddizioni.
