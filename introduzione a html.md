### introduzione fast
HTML: Hyper Text Markup Language. Indica al browser come strutturare una pagina.

In html, un elemento e' formato da:
* **tag** di apertura
* tag di chiusura
* **contenuto**, spesso, quasi sempre e' un'altro elemento.

Posso avere degli **elementi vuoti**, per esempio le immagini, queste non hanno un contenuto ma inseriscono qualcosa all'interno della pagina.

```html
<img
  src="https://raw.githubusercontent.com/mdn/beginner-html-site/gh-pages/images/firefox-icon.png"
  alt="Firefox icon" />

```

Gli elementi possono avere degli **attributi**. `class` e' un attributo che identifica il nome usato per applicare determinate impostazioni di stile.

```html
<p class="editor-note"> My cat is very grumpy </p>
```
Il codice applica al paragrafo lo stile per `editor-note`. Anche `img` utilizza degli attributi per impostare l'immagine da visualizzare.

Posso inserire dei **valori booleani**, come parametri, all'intero degli elementi. Questi mi permettono di "**disabilitare**" un elemento:
```html
<input type="text" disabled="disabled" />
<input type="text" disabled />
```
Le due righe si comportano allo stesso modo, la casella di input e' visibile ma non modificabile.

Posso **omettere le virgolette** quando specifico il valore di un attributo.
> `<a>` e' un'**ancora**, il testo che contiene viene trasformato in un link cliccabile.

```html
<a href=https://www.mozilla.org/> favorite website</a>
```

In questo caso non posso omettere le virgolette:
```html
<a href=https://www.mozilla.org/ title=The Mozilla homepage> favorite website</a>
```
`title=The Mozilla homepage` non viene letto nel modo corretto dal browser. Bisogna mettere le virgolette perche la stringa contiene degli spazi.

> posso usare **doppie virgolette** o **v** per il valore di un parametro

> per inserire una virgoletta dentro un campo (quindi gia virgolettato) uso: `&quot;`

### anatomia di un file html
```html
<!doctype html>
<html lang="en-US">
  <head>
    <meta charset="utf-8" />
    <title>My test page</title>
  </head>
  <body>
    <p>This is my page</p>
  </body>
</html>
```
1. `<!doctype html>`: per ragioni **storiche** va messo.
2. `<html>`: elemento root della pagina, in generale contiene l'intera pagina.
3. `<head>`: per inserire dati che non saranno visibili sulla pagina
4. `<meta charset="utf-8">`: meta server per inserire meta dati all'intero della pagina, questi dati non possono essere rappresentati dagli altri elementi. `charset="utf-8"` specifica il tipo di encoding per il documento.
5. `<title>`: imposta il titolo della pagina!
6. `<body>`: contiene ciò che viene mostrato a schermo.

> in HTML non importa quanti spazi metti dentro al contenuto di un elemento. *Il browser interpreta una sequenza di spazi come un unico spazio*.

Per inserire i seguenti caratteri come contenuto devo inserire una squenza particolare:

| Carattere | Sequenza |
| --------- | -------- |
| <         | `&lt;`   |
| >         | `&gt;`   |
| ""        | `&quot;` |
| &apos;    | `&apos;` |
| &         | `&amp;`  |




