Come HTML, non e' un linguaggio di programmazione. Indica al browser lo stile di ogni elemento che mostro sulla pagina.

```css
p {
	color: red;
}
```

per applicare il codice di stile, nel mio html, dentro il contenuto di `<head>` devo inserire:
```html
<link href="styles/style.css" rel="stylesheet" />
```

Nel codice `css`:
* `p` e' il **selector**
* `color` e' una **property**
* `red` e' il **valore** della property
L'intera struttura definita e' una **rule set**

Posso anche applicare l'intero rule set a piu elementi:
```css
p,
li,
h1 {
	color: red;
}
```

Il selector puo' selezionare:
* **elementi**: `p`, seleziona tutti gli elementi `p` nel codice html
* **id**: `#my-id` seleziona tutti gli elementi generici che hanno l'attributo `id="my-id"`
* **classi**: `.my-class` seleziona tutti gli elementi generici che hanno l'attributo `class="my-class"`
* **attributi**: seleziona tutti gli elementi generici che hanno un determinato attributo selezionato
* **pseudo-classi**: `a:hover` seleziona gli elementi `<a>` solo quando il cursore si trova sopra questi

Per esempio, cosi il font applicato per tutta la pagina.
```html
<link
  href="https://fonts.googleapis.com/css?family=Open+Sans"
  rel="stylesheet" />
```

### layout
Si usa il **box model**, ogni box ha le seguenti proprieta:
* **padding**: spazio intorno al contenuto
* **border**: linea solida che si trova appena oltre il padding
* **margin**: spazio attorno al bordo

Altre proprietà utili sono:
* `width`
* `background-color`
* `color`
* `text-shadow`
* `display`: imposta la modalita di display dell'elemento

### vari esempi
Cambiare il colore della pagina:
```css
html {
	background-color: #00539f;
}
```

Styling del body:
```css
body {
  width: 600px; /* forza la larghezza a 600 pixel */
  margin: 0 auto; /* top e bottom a 0, left e rigt a auto */
  background-color: #ff9500;
  padding: 0 20px 20px 20px;
  border: 5px solid black;
}
```

> **auto** divide in parti uguali lo spazio

Styling del titolo della pagina:
```css
h1 {
  margin: 0;
  padding: 20px 0;
  color: #00539f;
  text-shadow: 3px 3px 1px black;
}
```

> `text-shadow`, da sinistra a destra: horizontal offset, vertical offset, blur radius, colore dell'ombra.

Centrare un'immagine:
```css
img {
	display: block;
	margin: 0, auto;
}
```