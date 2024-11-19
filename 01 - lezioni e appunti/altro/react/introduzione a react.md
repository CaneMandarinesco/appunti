### cosa e perche'
* react e' una libreria per [[introduzione a nodejs|node.js]]. su questa si appoggiano diversi framework per lo sviluppo
* unica code base per sviluppare su web, android e ios.
* uso dei componenti: riutilizzabili in qualsiasi momento, in qualsiasi punto del codice.
* con react native posso sviluppare applicazioni per android e ios

### struttura di un progetto react
* `node_modules`: cartella che contiene i moduli node
* `public`: cartella che contiene le risorse che saranno pubbliche sul sito (esempio: immagini)
* `src`: cartella dove risiede il codice
* `.eslintrc.cjs`: contiene le regole per la scrittura del codice, in modo da avere un codice consistente.
* `.gitignore`: file che git dovrebbe ignorare
* `index.html`: file fondamentale!
* `package.json`: file che posso modificare, contiene le impostazioni per l'esecuzione del progetto, le dipendenze, nome e versione del progetto.
	* `dependencies`: dipendenze essenziali, sono quelle che verranno caricate online, quando ho pubblicato il sito.
	* `devDependencies`: dipendenze che servono per sviluppare
* `package-lock.json`: contiene le dipendenze di node, sono le dipendenze base per le dipendenze di `package.json`. 
* `vite.config.js`: e' il motore dietro al progetto, costruisce il progetto.


### index.html
In `index.html`, ho il div root, che viene popolato grazie al modulo dentro il `src/main.jsx`. 

Dentro `main.jsx` ho:
```javascript
// importo il codice che mi serve da node_modules
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import './index.css'
import App from './App.jsx'

// crea la base per lavorare con react
createRoot(document.getElementById('root')).render(
  <StrictMode> // serve per ottenere dei riferimenti per errori durante lo sviluppo
    <App />    // e' la mia app, che contiene l'html dell'app
  </StrictMode>,
)

```

`App` viene importato da `App.jsx`. La funzione `App` ritorna il codice html che e' integrato dentro il file javascript.

> nota: ciò che scriviamo in html viene poi tradotto in codice javascript.

Riassumendo:
* `index.html`: chiama il main
* `main.jsx`: fa partire react
* `App.jsx`: popola la pagina con quello che voglio

Per importare lo stile, basta fare `import "App.css"`.

## componenti
e' un modo più efficiente per scrivere codice `html`. Con un unica code base posso gestire più pagine. Magari ho delle pagine simili ma con dettagli di versi, con react posso gestire questi casi con i componenti.

In react sono delle funzioni javascript che generano codice html.

* `<> </>` e' un tag vuoto di react, che non viene renderizzato. Serve per avere una radice quando faccio il return in un componente.
* `{expression}` con le graffe messe dentro codice html, posso indicare che voglio valutare un espressione javascript ed inserire li il valore dell'espressione
* `"/${img}.svg"` uso la variabile `img` per costruire la stringa

### stile
posso aggiungere lo stile direttamente dal codice html oppure con react:
* `<li style="background-color: #212121; font-family: ..."> </li>`
* `<li style={{height: "300px", borderRadius:"30px"}}> </li>`. La prima graffa aperta indica che voglio scrivere in javascript, la seconda indica che sto inserendo un'oggetto.

L'uso dello stile react, mi permette di avere un codice di stile dinamico, per esempio:
```jsx
<>
	<div className={`box rounded ${x < 10 ? "rotated" : ""}`} x e' {x} </div>
</>
```

in questo caso scelgo se applicare la classe `rotated` al dive in caso $x\leq 10$. 

### tailwind




