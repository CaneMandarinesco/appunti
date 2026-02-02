# IO
due fasi:
* localizzare la risorsa, usando `File` o `Path
	* `File` e' la versione legacy
* aprire la stream
## IO stream
**Stream**: Rappresentano una sorgente oppure una destinazione, usando sempre lo stesso modello, ossia una sequenza di dati letti volta per volta.

Gli oggetti rappresentati possono essere: *file su disco, dispositivi, programmi, array di memoria. Basta che siano in grado di generare dati o di processarli.

Ci sono due tipi di stream, gestiti dalle relative classi:
* **Stream di caratteri**: `Reader`, `Writer`
* **Stream di byte**: `InputStream`, `OutputStream`

## File System Basics
### Accedere alle risorse con `Path`
E' una **interfaccia**, e si trova in `java.nio.file`, rappresenta un `path` e questo e' manipolabile in vari modi.

Con `Files` posso testare l'esistenza del file corrispondente al `Path`, crearlo, aprirlo, ecc...

### Lavorare con i path

Ottenere l'oggetto a partire dalla percorso:
```java
Path p1 = Paths.get("/tmp/fooo");
Path d1 = Path.of("/tmp/debug.log"); // metodo factory
```

* The [`toAbsolutePath()`](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/nio/file/Path.html#toAbsolutePath\(\)) method converts a path to an absolute path. If the passed-in path is already absolute, it returns the same [`Path`](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/nio/file/Path.html) object. The [`toAbsolutePath()`](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/nio/file/Path.html#toAbsolutePath\(\)) method can be very helpful when processing user-entered file names.
* `toRealPath` risolve anche link simbolici.
* `resolve` combina path a stringhe

```java
Path p1 = Paths.get("C:\\home\\joe\\foo");

// Result is C:\home\joe\foo\bar
System.out.format("%s%n", p1.resolve("bar"));
```

### Accedere al File System
**Ottenere il default file system**:`FileSystems.getDefault()`. Questo viene usato in combinazione con i metodi di `FileSystem`.

**Nota**: ogni `Path` e' associata ad un file system. Infatti, ogni `FileSystem` ha il suo separatore.

**Store**: ogni dispositivo di archiviazione, o  dispositivo montato si ottiene con `FileSystems.getDefault().getFileStores()`che ritorna un `Iterable`.

```java
FileSystem fileSystem = FileSystems.getDefault();
for (FileStore store: fileSystem.getFileStores()) {
    IO.println(store.name() + " - " + store.type());
}
```

### Manipolare file e directory
* `Files.exists()`
* `Files.notExists()`
> **Nota**: se entrambi ritornano `false`, allora non posso determinare l'esistenza.

```java
Path file = ...;
boolean isRegularExecutableFile = Files.isRegularFile(file) & Files.isReadable(file) & Files.isExecutable(file);
```