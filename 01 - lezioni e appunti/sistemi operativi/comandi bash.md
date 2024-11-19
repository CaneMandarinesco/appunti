#awk #sed #ls #grep #ps #screen #nohup
* `awk`: e' scritto usando il linguaggio `awk` e permette di effettuare **manipolazioni** su testo e dati.
* `cat`: concatena file e scrivi su `stdout`
	* `cat -`: prende lo standard input e lo mette nello standard output.
* `cut`: rimuovi intere sezioni da linee di file
* `diff`: compara due file
* `grep`: stampa le righe dall'input che contengono un pattern
* `head`: mostra le prime righe di un file, esempio: `head -5`
* `less`: leggi un file in un terminale, senza dover leggerlo tutto, per intero. Infatti e' piùc veloce di `vi`
* `od`: dump files in octal and other formats
* `sed`: editor di stream. permette di effettuare modifiche su una stream di dati. e' efficiente perché legge il file una sola volta.
* `sort`: sort di file di testo
* `split`: spezza un file
* `tail`: mostra gli ultimi byte di un file
* `tr`: effettua modifiche sui caratteri in un file (traduzione ed eliminazione di caratteri)
* `uniq`: fa vedere linee duplicate in un file
* `wc`: statistiche di un file, `(#righe, #parole, #caratteri)`
* `tar`: archiviazione

altri comandi:
* `whoami`: mostra il login name
* `date`: data odierna
* `cal`: mostra un calendario
* `w`: chi e' loggato?
* `pwd`: current working directory
* `ls`, `cd` per navigare il file system.
* `printenv`: mostra le **variabili d'ambiente** definite
* `touch`: **crea un file vuoto**
* `alias/unalias`

> se la shell viene eseguita come root il carattere di prompt cambia da `$` a `#`.

### ls
```bash
ls -lthr
```
* `-l` **long listing format**: mostra tutte le informazioni di file e directory
* `-t` **time**: mostra prima i file più nuovi
* `-r` **reverse**: mostra al contrario, quindi avro' per ultimi i file piu vecchi
*  `-h` **human readable**: grandezze dei file espressi in KB
* `-a` **hidden files**

```bash
ls -l
ls -lR # ricorsivo
ls -lh # file size in formato 
ls -lS # sort per file size
ls -lt # sort per time
ls -F # metti un carattere alla fine di ogni entry (per la concatenazione con altri comandi in pipe)
```
### sed
> usa un linguaggio di programmazione per effettuare operazioni su una stream dati
```bash
sed 's/hello/world/' - # lavora sull'input da tastiera
sed -i 's/hello/world/' file.txt # -i effettua i cambiamenti direttamente sul file.


sed -n '45p' file.txt # -n sopprime l'output, '45p' voglio solo l'output della linea 45
sed -n  '1p ; $p' one.txt two.txt three.txt # -n sopprime l'output, '1p ; $p' vuol dire prima riga e ultima riga (di three.txt)

```

```bash
sed '30,35d' input.txt > output.txt # elimina le righe da 30 a 35
sed '/^foo/q42' ... # se almeno una linea inizia con foo, allora restituisci 42 (q: quit)
```

### awk
> simile a sed in quello che fa. E' usato per manipolare dati e generare dei report. Indicato per file `.csv`, `.yaml`, ecc...

sintassi generale
```bash
awk options 'selection _criteria {action }' input-file > output-file
```

```bash
awk '/manager/ {print}' employee.txt # prende employee.txt e stampa le righe che contengono la stringa manager

awk '{print $1, $4}' employee.txt # stampa prima e quarta parola di ogni riga

awk '{print NR, $0}' employee.txt # stampa il numero di riga e poi la riga
```

### alias
```
alias rm='rm -i'
```
Posso associare ad una stringa un comando, per esempio: quando chiamo `rm`, mi verrà sempre chiesto di confermare l'eliminazione di un file.

Gli alias si trovano nel file `.bashrc`. Se rimuovo un alias da `.bashrc`, per vedere gli effetti del cambiamento devo avviare una nuova shell. Altrimenti posso usare `unalias`
### esempi di pipes
#### esempio 1
```bash
w | less # mostra chi e' loggato e usa less per leggere
w | grep -v "danilo" # mostra le righe non contenenti "danilo"
w | grep "danilo" | sed s/danilo/davide/g # rimpiazza "danilo" con "davide"
```

#### esempio 2
```bash
w | wc
w | awk -F" " '{print $1}' | less # estrai la prima colonna con awk e mostrala usando less
```

#### esempio 3
```bash
echo 'Hello pise' > myfile.txt # crea un nuovo file
echo ' come stai' >> myfile.txt # append
```
#### esempio 4
```bash
printenv | grep ^PATH | sed 's/^PATH=//g' | tr ":" "\n" | sort | uniq | wc
```

* prendo le mie variabili di ambiente
* prendi la riga che inizia per `PATH`
* sostituisci `PATH` con `/` (???)
* sostituisci `:` con `\n`
* fai sort e togli le righe uguali (????)
* conta le righe???

Posso creare anche degli script bash, devo dare lo `shebang`: `#!/path/to/interpreter`

## Gestione Processi
**comandi e scorciatoie** da tastiera per gestire i processi in bash:

* `&`: esecuzione in **background**, va aggiunto alla fine. Subito dopo la shell mostra il numero di jobs in background e il suo **process id**.
* `./my_script.sh`": esegui uno script che si trova nella **directory corrente**
* `ps`: mostra i processi avviati dall'utente
	* `ps -ef`: mostra tutti i processi avviati!
* `kill pid` /  `kill %job_number`
* `Ctrl+z`: sospende il job corrente
* `bg`: esegui un job sospeso in background
* `fg`: posso portare in **foreground** (riprendere il controllo) del job in background
* `jobs`: lista di jobs in esecuzione


### screen e nohup
`screen` permette di gestire **sessioni di terminale**. queste continuano ad essere eseguite anche quando mi disconnetto dal terminale (utile in una sessione ssh). Quindi in un singolo terminale bash, screen mi permette di gestire più sessioni contemporaneamente. 
Posso **attaccarmi** a uno screen e **staccarmi** quando voglio, anche da terminali diversi.

* `screen`: inizia una sessione
* `Ctrl+A` e poi `D` per staccarmi
* `screen -ls` mostra le sessioni attive
* `screen -r <session_id>` riattaccati ad una sessione
* `Ctrl+D` termina la sessione

`nohup` (no hang up), generalmente viene usato cosi: `nohup command &` e fa eseguire un comando in background, anche se il terminale viene chiuso! Ma un comando eseguito con `nohup` non può più essere portato in `foreground`.

## Creare e gestire utenti
```bash
sudo useradd -s /path/to/shell -d /home/{dirname} -m -G {secondary-group} {username}
```
* `-m` indica che se non esiste la cartella utente, deve essere creata.
* `sudo passwd {username}`: dopo che creo un utente, devo configurare la sua password

## regex
* `^foo`: indica linee che iniziano per "foo"
* `bar$`: indica linee che terminano con "bar"
* `[0-9]\{3/}`: 3 numeri da 0 a 9