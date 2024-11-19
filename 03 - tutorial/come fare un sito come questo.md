 cosa useremo:
* [quartz](https://quartz.jzhao.xyz/): il software che genera il sito web per noi
* account [github](https://github.com/): piattaforma per caricare online i file del sito web
* [git](https://git-scm.com/): per gestire i file, e caricarli online
* [obsidian](https://obsidian.md/): per scrivere i file che andranno su github

>[!warning]
>i comandi mostrati, come `pacman`, funzionano su arch linux.

> [!note] in caso di problemi:
> segui la guida su: https://quartz.jzhao.xyz/
> oppure guarda questo video: https://www.youtube.com/watch?v=6s6DT1yN4dw&t=227s

## conoscenze pregresse
nulla di particolare, ma ti farebbe comodo sapere:
* come scrivere testo in [markdown](https://www.markdownguide.org/) (nulla di complicato!)
* come funziona git (può essere abbastanza complicato)
## installare il software
se sei su arch linux:
```bash
sudo pacman -Syu
sudo pacman -S git # dovrebbe essere gia incluso su arch
sudo pacman -S npm nodejs
```
per installare `npm` e `nodejs`.  

> **npm**: software per gestire i pacchetti javascript.
> **nodejs**: esegue codice javascript

## per iniziare.
come riportato sulla wiki di quartz:
```bash
git clone https://github.com/jackyzha0/quartz.git
cd quartz
npm i
npx quartz create
```
* con `git clone ...`, viene creata una copia di **quartz** sul vostro computer. per "installare" quartz basta scaricare una cartella!
* `cd quartz` per entrare dentro la cartella con dentro il tuo sito web
* `npm i` e `npm quartz create` per inizializzare il progetto

## come scrivo il mio sito web?
per scrivere il sito web non serve sapere scrivere in *javascript*, *css*, ecc... basta scrivere usando il formato [markdown](https://www.markdownguide.org/)! per scrivere in markdown si puo' usare un editor di testo qualsiasi. **vscode** va benissimo, ma io preferisco usare [obsidian](https://obsidian.md/).  
obsidian, non e' solo un software per scrivere, infatti e' costruito in modo tale da comportarsi come un "secondo cervello".  
con obsidian puoi avere una *panoramica* di tutte le tue note e dei collegamenti tra queste grazie alla **graph view**:
![[Pasted image 20240214121422.png]]

## scrivere la pagina index
con il tuo editor preferito, apri la cartella `quartz/content`. qui e' dove risiedono le tue pagine web.  
all'interno dovrebbe gia' esserci un file: `index.md`. questo file e' la prima pagina che viene caricata quando si accede al sito, quindi questa pagina dovrebbe essere organizzata in modo tale da riassumere tutti i contenuti del tuo sito e contenere i link per raggiungere le pagine (fai riferimento alla mia pagina [[index]]).
puoi cominciare a sperimentare scrivendo su questo file, per aggiungere altre pagine basta aggiungere altri file `.md` nella cartella content. 

## caricare una preview del sito
una volta che hai scritto qualcosa sulla pagina index, puoi avviare una **preview** del tuo sito sul tuo computer con questo comando:
```bash
npx quartz build --serve
```
ora andando sul tuo browser, caricando l'indirizzo: http://localhost:8080/ potrai navigare sul tuo sito.

> [!note] attenzion!
> il tuo sito non e' ancora **pubblico**, infatti il link e' "visitabile" solo dal tuo computer!

## come rendere pubblico il sito web?
per prima cosa devi creare un **repository** su [github](https://github.com):
* cerca il tasto "crea nuovo repository"
* dagli un nome
* consiglio di impostarlo come *privato*
* clicca su "crea repository"

### impostare l'indirizzo remoto di git
ora bisogna dire a *git* dove si trova su internet il tuo repository, quindi mentre ti trovi nella cartella `quartz` esegui:
```bash
git remote set-url origin REMOTE-URL
```
dove `REMOTE-URL` va sostituito con **l'url** che porta alla pagina del tuo repository su github (del tipo `https://github.com/nomeutente/nomerepository.git`), dove ovviamente devi sostituire `nomeutente` e `nomerepository` con i tuoi dati.  

opzionalmente, per avere quartz sempre aggiornato, puoi eseguire anche:
```bash
git remote add upstream https://github.com/jackyzha0/quartz.git
```
per dire a git di sincronizzare i file di quartz con quelli che si trovano sul repository online di quartz.  

### delploy.yml
ora dobbiamo creare il file che dice a github di costruire il sito e di rilasciarlo ogni volta che aggiorniamo i file.  creiamo quindi il file `quartz/.github/workflows/deploy.yml` e metteteci questa roba all'interno facendo copia e incolla:

```yaml
name: Deploy Quartz site to GitHub Pages
 
on:
  push:
    branches:
      - v4
 
permissions:
  contents: read
  pages: write
  id-token: write
 
concurrency:
  group: "pages"
  cancel-in-progress: false
 
jobs:
  build:
    runs-on: ubuntu-22.04
    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0 # Fetch all history for git info
      - uses: actions/setup-node@v3
        with:
          node-version: 18.14
      - name: Install Dependencies
        run: npm ci
      - name: Build Quartz
        run: npx quartz build
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v2
        with:
          path: public
 
  deploy:
    needs: build
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v2
```

### impostare github
ora dal vostro repository su github andate sulla tab `Settings`. sul lato sinistro, nella sidebar, clicca su `Pages` e sotto a `source` clicca il tasto e seleziona `GitHub Actions` e applica i cambiamenti.

### caricare i file online!
per caricare i file online occorre usare i comandi base di git :
```bash
# per cambiare il branch da main a v4
git checkout v4

# aggiungi i file nuovi
git add *
# crea il commit con un messaggio
git commit -m "messaggio"
# carica online
git push origin
```

e fatto! dopo qualche secondo il sito e' aggiornato e sara' navigabile al link che si trova nella sezione `Releases`.

ora gestire i commit con git a volte (per esempio, se si verificano problemi di merging) puo' diventare complicato quindi e' bene se impari ad usare git.

### credenziali di accesso
>[!warning] attenzion!
> se git vi chiede di inserire delle credenziali per eseguire il push dei cambiamenti vuol dire che bisogna creare le credenziali! infatti non e' possibile utilizzare la propria password per motivi di sicurezza ma bisogna generare un token.

su github vai alle impostazioni relative al tuo utente (con il quale hai creato il repository) e nella sidebar a sinistra clicca su `Developer Settings` e poi `Personal access tokens` e infine `Tokens (classic)`.   per generare il token clicca su `generate new token (classic)` e nella pagina che spuntate tutte le spunte tranne:
* copilot
* audit_log
* admin:enterprise
* write:discussion
* delete_repo
* notifications
* admin:public_key

ora potete utilizzare il token generato per eseguire l'accesso dalla shell.

