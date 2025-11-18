**Definizione di agente**: Un'agente e' un intelligenza artificiale che agisce razionalmente sull'ambiente, facendo la cosa giusta. Questo ha percettori per ricevere segnali in input dall'ambiente e degli attuatori per applicare un'azione sull'ambiente.
La sequenza delle percezioni descrive la storia dell'agente e da questa in generale, deriva l'azione che deve essere eseguita a seguito di un percezione ricevuta, questa tabella descrive la funzione a gente e se e' compilata correttamente allora ho un'agente razionale.

**Definizione agente razionale**: Fa la cosa giusta, e se non c'e' una cosa giusta da fare ha delle preferenze sul comportamento da assumere. Agisce in modo efficace per massimizzare la funzione obiettivo.

> parla della funzione obiettivo, **deve massimizzare il valore atteso della funzione obiettivo**

**Definizione Misura di Prestazione**: serve a valutare la prestazione di un'agente, analizzando la sequenza di stati che attraversa.

> **Nota**: guarda gli stati che attraversa l'ambiente!!! Es di vacuum world, ad ogni quadrato pulito assegno +1. In una partita di scacchi la misura di prestazione e' vincere la partita per esempio
> Deve essere **oggettiva**

**Definizione di Razionalita dell'agente**: da cosa dipende la razionalita? La razionalita dell'agente oltre che a dipendere dalla misura di prestazione, dipende anche da quanta conoscenza pregressa ha questo dell'ambiente, dalle azioni che questo e' in grado di fare e dalle percezioni che ho effettivamente ricevuto.

**Definizione di Onniscenza**: un'agente onniscente, sa, per ogni azione cosa succede nell'ambiente. L'onniscenza non c'e' mai 

**Definizione di Agente Autonomo**: un'agente autonomo non ha conoscenza pregressa, dunque si comporta esclusivamente in misura dell'esperienza che ha dell'ambiente. E' altamente flessibile in quanto si puo' adattare a diversi ambienti.

* **Osservabilita**: se l'ambiente e' completamente osservabile, vuol dire che posso percepirlo tutto, altrimenti e' semi-osservabile o non osservabile.
* **Determinismo**: se l'ambiente e' deterministico, allora ad ogni azione, l'ambiente si comporta in maniera prevedibile. Altrimenti l'ambiente cambia seguendo una distribuzione di probabilita (se stocastico), o in modo completamente imprevedibile se non deterministico.
* **Episodico**: Se episodico, l'azione applicata dall'agente in un determinato stato non influenza l'azione successiva. Altrimenti vuol dire che c'e' bisogno di pianificare una sequenza di azioni per risolvere il problema.
* **Statico/Dinamico**: Se statico, il mondo non cambia mentre l'agente decide. Se dinamico tutto puo' succedere mentre elaboro. 
* **Noto/Ignoto**: Se noto, vuol dire che l'agente sa esattamente cosa succede all'ambiente quando esegue un'azione (sebbene l'ambiente possa essere non deterministico). Se fosse ignoto, allora l'agente dovrebbe imparare cosa fanno le sue azioni nell'ambiente.
> **Nota**: noto e ignoto e' riferito all'agente!!!

* **Environment Generator**: Ho bisogno di generare ambienti che interagiscono con gli agenti inviando percezioni e raccogliendo le azioni di questi in modo da poterne valutare il comportamento. E' un generatore e simulatore di ambienti.

**Table driven agent**: questo programma agente si limita ad implementare la funzione agente come se fosse una tabella.
```c

function TableDrivenAgent(percept)
	percepts.append(percept)
	action = table.lookup(percepts)
	return action
```
Questo agente occupa moltissima memoria per la `table` ed e' difficilmente estensibile, tutta via basta compilare correttamente la `table` per avere un'agente razionale.in pratica va bene solo per agenti semplici.

**Skeleton-Agent**: il modello che vogliamo seguire per scrivere gli agenti e' il seguente. Abbiamo una memoria interna che viene aggiornata, ed in base allo stato di questa scelgo l'azione da eseguire.
```c
function SkeletonAgent(percept)
	mem = update(percept)
	act = selectBestAction()
	mem = update(act)
```

**Agente reattivo semplice**: l'agente reattivo semplice e' simile a quello table driven, ma piuttosto guarda solo l'azione percepita per indicizzare la tabella. Si applica bene all'esempio del vacuum robot su due stanze.
```c
function SimpleReflexAgent(percept)
	state = interpretPercept(percept)
	rule = table.lookup(percept)
	act = rule.action
	return act
```

> Nota: usano condition action **rule** su **stati**

**Agente reattivo su modello**: similmente a quanto detto prima, ma quello su modello, usa un modello per determinare il prossimo stato a partire dalla percezione e lo stato corrente!
```c
function SimplexModelAgent(percept, model)
	state = updateState(state, percept, model)
	rule = table.lookup(percept)
	act = rule.action
	return act
```


**Agente con obiettivo**: un'agente con obiettivo agisce al fine di massimizzare una funzione obiettivo, o quanto meno di raggiungerlo.

Quando l'obiettivo non basta, si introducono gli agenti con funzione di utilita: raggiungere l'obiettivo non basta, bisogna scegliere azioni in base alla loro utilita per lo scopo.


**Componenti di un agente**:
* atomico
* fattorizzato
* strutturato

