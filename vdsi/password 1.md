> [!warning] studia /etc/passwd ed /etc/shadow


**password**: segreto condiviso tra utente e servizio.

**password overload**: molte persone usano la stessa password su piu siti.

**charset ristretto**: uso solo i caratteri della tastiera invece di quelli possibili. 
* se la password usa in modo uniforme tutti i bit: $\frac{1}{256^8}$
* password usa solo lettere e numeri: $\frac{1}{36^8}$

password con **bassa entropia**: sono password facili da ricordare, dove una sequenza di caratteri dipende da un'altra. 

**OSINT/SOCMINT**: con tecniche di social engineering posso capire in base agli interessi di una persona che parole potrebbe usare. (conseguenza della bassa entropia)

**shannon entropy**: misura la sicurezza di una password.

**attacco a dizionario**: si applica facilmente a password con bassa entropia su un set di password da testare.

# wordlist
**wordlist**: esistono wordlist pubbliche con le password piu probabili.
## crunch
**crunch**: tool che crea wordlist basandosi su **combinazioni matematiche**

**quando uso crunch?** se conosco la struttura della password.

## cewl
**cewl**: custom wordlist generator. Permette di creare wordlist **context-aware**
```bash
cewl https://www.target.xyz -w targetWL.txt
```

**quando uso cewl**? per generare password basate sulle parole dentro un sito. (utile se devo fare pentest in una azienda).

## anarchy
**username anarchy**: genera gli username potenziali usando nome e cognome.
**quando uso anarchy**?: pentest in una azienda dove di solito i nomi utente seguono un pattern.

# attacco online
**hydra**: tool con cui mando richieste di login (verso SSH, Web Login, ecc...)
* **latenza**: il server potrebbe limitare il mio troughput
* **visibilita**: ogni attempt viene loggato sulla macchina
* **lockout**

**con hydra**: vengo sgamato subito.

# attacco offline
**hashcat e john the ripper**: si usano quando ho il database delle password crittografate.
* **velocita**: dipende dalla mia macchina
* **invisible**: faccio tutto in locale

## nascondere le password
**hash function**: dato l'hash e' impossibile tornare al testo in chiaro se non si consce come funziona la funzione di hashing

**salt**: una stringa random che viene aggiunta alla password. Password identiche di utenti diversi non collidono.

**cosa salvo**? nel database delle password ho `user | salt | H(password||salt)`. Devo ricordare il **sale**!

**crackstation**: sitoweb fiko
## linux password
**/etc/shadow**: `id salt hash : other-stuff : ...`
* $\text{hash} = H_{id}(\text{salt} || \text{password})$.
* $H$ e' **yescrypt** 

**yescrypt**: invece di essere CPU intensive, usa tanta memoria per criptare. E' piu difficile per l'attaccante perche' ha bisogno di tanta ram per decriptare.

## tool di password cracking
**dizionario**: ...


**john the ripper**: si basa sulla potenza computazionale del processore.

> [!warning] john the ripper
> fare le challenge su sta roba


**unshadowing**: `unshadow /etc/passwd /etc/shadow` produce un file per john the ripper.

## single-crack
```bash
john --single hashesFile
```
il comando per ogni utente prova le combinazioni di password a partire dal nomeutente. hashesfile e' stato prodotto da unshadow.

`john.conf`: contiene le regole su come funziona john

`john -w=/path/to/wordlist hashesFile`: usa la wordlist data per sborrare su hashesFile

**rockyou**: un esempio di wordlist da usare.

**se l'attacco fallisce?** con `-rules=All` posso applicare delle regole.

## incremental
**incremental**: prova tutte le combinazioni possibili

**incremental limitato**: prova l'attacco su un insieme ristretto di combinazioni. posso specificare lunghezza minima e massima

`john --incremental:<set> hashesFile --min-length=<N> --max-length=<N>`

## custom
Uso le mie regole e la mia wordlist (presa magari dal sito dell'azienda).

**custom rules**:
* `c`: capitalize la prima lettera

**preprocessors**

**hashcat**

**attacco alle cose compresse**

## crack online
