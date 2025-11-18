
Registro immutabile, condiviso e decentralizzato la cui fiducia si basa sulla verifica crittografica (e sulla banca centrale autorizzata).

rete peer to peer di nodi

### blocco
E' formato da un'header, sezione delle operzioni e un hash che si riferisce al blocco precedente.

> **Nota**: Se qualcuno cambia un blocco (con intenzione malevola), ne cambia l'hash!

### firme
Sistema a chiave pubblica, privata e generatore. Quella privata serve a criptare, e la chiave pubblica ne garantisce l'autenticità e non possono essere ripudiate.

### aggiungere blocchi
per aggiungere un blocco bisogna calcolare un'hash particolare e deve avere una certa sequenza di `0` alla fine.

Minare vuol dire buttare a caso un `nonce` e calcolarne l'hash finche non ho trovato un hash che va bene.
Il `nonce` deve soddisfare la `proof of work`.

chi cerca hash per blocchi viene ricompensato in valuta e commissioni.

### rete p2p
i nodi diffondono l'informazione.

### smart contract
codice open source che gestisce le transazioni. Eseguito in modo distribuito dagli attori della blockchain

### stake
 Una quantita di token bloccati (ossia stake). Viene aggiunta da un'utente alla blockchain.
Il prossimo blocco da validare e aggiungere alla blockchain e' scelto randomicamente tra i validatori.

I validatori si occupano di confermare e firmare il blocco.

### avalanche
basato su proof of stake. Permette di creare una propria block chain appoggiandosi al software di avalanche.

E' un'insieme di block chain: `X,P,C`.

Sistema a `gossip`: i nodi si interrogano in piccoli gruppi e si cerca un consenso

No mining!!!!

### smart contract
Ha bisogno di un database per le informazioni persistenti.
Si scrive in solidity, il codice e' compilato e caricato nella blockchain, incluso il sorgente.

Il log delle azioni dell smart contract puo' essere letto e rieseguito per verificare. 
