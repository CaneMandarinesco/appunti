**head** e' la parte che non viene mostrata dal web browser, contiene informazioni come: titolo della pagine, link ai file css, nome autore, e meta dati.

```html
<meta name="meta-name" content="mycontent" />
```

esistono tanti tipi di meta dati, per esempio Open Graph Data e' un protocollo di Facebook per i meta dati:
```html
<meta
  property="og:image"
  content="https://developer.mozilla.org/mdn-social-share.png" />
<meta
  property="og:description"
  content="The Mozilla Developer Network (MDN) provides
information about Open Web technologies including HTML, CSS, and APIs for both websites
and HTML Apps." />
<meta property="og:title" content="Mozilla Developer Network" />
```

Questo esempio permette a una pagina di Facebook, di mostrare una preview del link quando il cursore e' sopra questo.

I meta dati possono essere usati per aggiungere **icone**:
```html
<link rel="icon" href="favicon.ico" type="image/x-icon" />
```

Posso aggiungere piu icone:
```html
<link rel="icon" href="/favicon-48x48.[some hex hash].png" />
<link rel="apple-touch-icon" href="/apple-touch-icon.[some hex hash].png" />
```

Posso aggiungere uno script javascript:
```html
<script src="my-js-file.js" defer></script>
```
