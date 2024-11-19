Server per creare servizi back-end usando javascript. 

### architettura di node
il codice javascript deve essere interpretato da un engine:
* firefox usa `spidermonkey`
* chrome usa `v8`

Fino al 2008 si poteva eseguire javascript solo sul browser. Node prende `v8`, lo inserisce dentro un'applicazione desktop, in modo da permettere l'esecuzione di codice javascript fuori dal browser. 

>[!note] node non e' un linguaggio
> node e' un `runtime environment` per l'esecuzione di codice javascript

### come funziona node
* non blocking / asynchronous: un singolo thread gestisce più richieste contemporaneamente, mentre aspetta i dati dal database per esempio il thread accetta altre richieste e le inoltra.
* blocking / synchronous: un singolo thread gestisce una sola richiesta alla volta

I thread mettono inseriscono le richieste nella `Event Queue`, node in background legge di continuo questa struttura dati. E' ottima per app **I/O intensive** (lettura dischi, operazioni di rete).

Node non dovrebbe essere usato per app CPU-intensive (image manipulation, ai, ecc...). 

> [!note] OSS.
> node e' ottimo per applicazioni real time e quando bisogna processare grandi quantita di dati

### esempio
```javascript
function sayHello(name) {
    console.log('Hello ' + name);
}

sayHello('Davide'); // OK
console.log(window); // ReferenceError!
```

per eseguire il codice: 
```bash
node app.js
```

In node non esistono tutti gli oggetti a cui un programmatore web potrebbe essere abituato: per esempio se provo a fare `console.log(window)` ho un `ReferenceError`, dato che nell'ambiente di `Node` non esiste quest'oggetto. Invece se vado ad eseguire il codice in un vero e proprio browser non ho problemi.

### node module system
tutte le funzioni globali appartengono all'oggetto `global`
```javascript
console.log(); // global.console.log()

setTimeout(); // uguale a global.setTimeout()
clearTimeout(); // uguale a global.clearTimeout()

var message = '';
	console.log(global.message); // Questo pero' mi da undefined
```

Le variabili non sono aggiunte all'oggetto global. Per questo bisogna introdurre i moduli

In node esistono molti moduli che possiamo importare, per evitare che i nomi delle loro variabili siano in conflitto bisogna definire un modulo:

```javascript
console.log(module); // module non e' un oggetto dentro global
```

Ora creiamo un modulo che per esempio gestisce il log della mia app e lo inoltra ad un server remoto, in un file chiamato per esempio `logger.js` andrò a scrivere:
```javascript
var url = 'https://mylogger.io/log';

function log(message) {
    // Send an HTTP request
    console.log(message)
}

// voglio esportare il metodo
module.exports.log = log;
```

invece in `app.js`:
```javascript
const logger = require('./logger') // si assume che tutti i file terminiano in .js
logger.log('messaggio');
```

> `jshint` e' un comando che permette di trovare errori nel codice (es: assegnazione ad una variabile costante)

in ogni modulo, il codice viene `wrappato` dentro una funzione, infatti il codice non viene mai richiamato direttamente. Quando eseguiamo `logger.js` diventa piu o meno cosi:
```javascript
(function (exports, require, module, __filename, __dirname){
	var url = 'https://mylogger.io/log';
	
	function log(message) {
	    // Send an HTTP request
	    console.log(message)
	}
	
	// voglio esportare il metodo
	module.exports.log = log;
})
```



#### osservazioni
* posso fare `module.exports = log` (ovvero quando faccio require ottengo direttamente la funzione `log`)
* non posso fare `exports = log`, sto modificando il riferimento a `modules.exports`
* posso fare `exports.log = log`


