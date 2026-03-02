## One Time Pad
**OTP**: scelgo $K$ randomicamente e cifro e decifro facendo lo **XOR**. 
* $E(x,k) = x \oplus k$
* $D(y,k)=y \oplus k$ 
* Dunque: $D(E(x,k), k) = x$, ossia $(x \oplus k) \oplus k = x$.

**Non e' pratico**: Ho bisogno di chiavi lunghe tanto quanto il messaggio da inviare. (Come faccio se devo spedire un file da 1GB?)

**Intuizione**: se $K$ e' random, il risultante $C$ cifrato, sembra randomico tanto quanto $K$.

**Sicurezza**: un cifrario e' sicuro se dato un grande numero di coppie di plaintext-cyphertext, non posso imparare nulla su come il cifrario si comporti su altri plaintext o cyphertext.

**Attack model**: definisco un'attaccante (cosa puo' fare e cosa non) che a partire da un set di dati cerca di capire come funziona il cifrario.

**Security Goal**: cosa voglio scoprire con l'attacco?

## Modelli di attacco
**A che servono**:
* capire cosa mi aspetto dal mio cifrario. Sapendo come funziona l'attacco, creo il cifrario in modo da resistere a questo attacco.
* capire a cosa e' debole l'attacco, cosi l'utente sa in quali ambienti utilizzare il cifrario
* capire quali attacchi sono fattibili per rompere un cifrario.

**I Modelli di attacco sono differenti dalla realta**: sono un'approssimazione per eccesso o pure per difetto di cosa succede in un'attacco reale.

**Modelli utili**: si devono basare su cosa puo' fare un'attaccante, veramente, nella realtà. Non deve sottostimare le capacita di un'attacante.

## Modelli Black-Box
**Query**: mando input ad una funzione, ricevo un'output, ma non ho accesso alla funzione, a come e' fatta.

**Black Box**: si basa sul fatto che l'attaccante puo' fare query alla funzione, senza poter vedere come funziona all'interno.

> [!note] lista di modelli
> La seguenti lista va dal piu debole al piu forte.


**Modello Cyphertext-only**: l'attaccante *guarda il cifrato, ma non so quali sono i corrispettivi testi in chiaro*, ne tanto meno quale **sottoinsieme** di plaintext e' stato usato per generare i cifrati.

**Modello Know-Plaintext**: l'attaccante guarda il cifrato e conosce i rispettivi plaintext. Ho una **lista** di coppie $(x,y)$

**Modello Chosen-Plaintext**: l'attaccante fa query per i plaintext che vuole e ottiene i corrispettivi cyphertext.

**Modello Chosen-Cyphertext**: l'attaccante puo' fare query sul cyphertext 

E altre pippe su altre cose...

## Randomness

**Entropia**: misura dell'incertezza, o il disordine in un sistema.

**Entropia della distribuzione di probabilità**:
$$
-p_{1} \times \log(p_{1}) -p_{2} \times \log(p_{2}) - \dots - p_{N} \times \log_{p_{N}}
$$

**Entropia di una stringa** di $128$ bit: $2^{128} \times (-2^{128} \times \log(2^{-128})) = 128$

**Entropia e distribuzione uniforme**: la distribuzione uniforme, massimizza l'entropia, tutti gli eventi hanno stessa probabilita di accadere 

**Entropia e distribuzioni non uniformi**: l'entropia e' piu bassa.

**Come genero random bit?** Devo usare RNG E PRNG.
**RNG (Random Number Generator)**: risorsa di entropia, data dal Random Number Generator
**PRNG (Pseudo RNG)**: usa l'entropia per produrre random bit di alta qualita.

**Da dove viene la Randomness**? Dall'ambiente analogico, che e' impredicibile.

**RNG e Sistemi Operativi**:  Un RNG prende entropia dal sistema operativo che legge i sensori (IO, Rete, Attivita sul Disco) ma puo' essere facilmente manipolabile.

**Sorgente analogica e digitale**: 
* RNG produce in generale **pochi** bit a partire da una *sorgente analogica*.
* PRNG produce tanti `random-looking` bit a partire da una *sorgente digitale* (per esempio un RNG)

![[Pasted image 20260227230923.png]]

## PRNG
**Entropy Pool**: un **PRNG** riceve bit ad intervalli regolari da un **RNG** e li usa per aggiornare l'`entropy pool`, un'area di memoria. Funziona come sorgente di entropia per il PRNG

**DRBG**: **Deterministic** Random Bit Generator, espande i bit della entropy pool in una sequenza molto piu grande. Il PRNG non da mai in input al DRBG 2 valori uguali, perche' otterrei lo stesso output.

**Implementare il PRNG**:
* `init()`: inizializza la entropy pool ad una valore di default, e lo stato interno.
* `refresh(R)`: aggiorna la entropy pool usando i dati $R$. Anche chiamata `reseeding`, dove $R$ e' il `seed`.
* `next(N)`: ottieni $N$ random bits ed aggiorna la pool, in modo che la prossima `next` ritorni bit differenti.

**Sicurezza del PRNG**: 
* **Backtracking Resistance**: non devo essere in grado di recuperare i bit generati in precedenza.
* **Prediction Resistance**: non devo essere in grado di predire i nuovi bit.

## Fortuna PRNG
**Windows**: viene utilizzano in Windows.

Memoria:
* **32 entropy pools**: $P_{1}, \dots, P_{32}$, tali che $P_{i}$ usata ogni $2^i$ reseeds.
* **stato interno**: $K \in \{ 0,1 \}^{16 \times 8}$ chiave, $C \in \{ 0,1 \}^{16\times 8}$ contatore. Entrambi da 16 byte.

`init`: $K, C$ impostati a 0. $P_{i}$ vengono svuotati.

`refresh(R)`: aggiungi $R$ ad una delle entropy pools.
* `refresh` chiamata regolarmente dal sistema operativo
* il sistema sceglie gli RNG da usare per produrre $R$.

`next(N)`: aggiorna $K$ usando dati da una o piu entropy pools scelta in base a quante volte ho aggiornato $K$. 
* Gli $N$ bit vengono ritornati cifrando $C$ usando $K$ come chiave. 
* Se $C$ "non basta" allora Fortuna cifra $C+1$, $C+2$, ecc...

**Difficolta di implementazione**: ci sono molti dettagli che puoi facilmente sbagliare, e non ci sono test per verificare che funzioni!

**PRNG Crittografato e non**: Un PRNG non-crittografato produce una distribuzione uniforme per videogiochi e simulazioni, ma potrebbe essere predicibile e non utilizzato in contesti crittografici.  

Secondo "Serious Cryptography": i PRNG base, esposti dalle api standard di programmazione sono per la maggior parte non-cryptographic. Es: `random` module in python.

## Mersenne Twister
**MT**: algoritmo non crittografico PRNG. Usato in vari linguaggi.

**Stato Interno**: array $S$ di 624 parole a *32-bit*.
**Evoluzione**: L'array inizialmente e' $S_{1}, S_{2},\dots , S_{624}$, che evolve a $S_{2}, S_{3}, \dots, S_{625}$, ecc... usando l'equazione:
$$
S_{k+624} = S_{k+397} \oplus \mathbb A((S_{k} \land \text{ 0x80000000}) \lor (S_{k+1} \land \text{ 0xffffffff}))
$$
$A$: trasforma $x$ in $x \gg 1$ se $\text{MSB} = 0$, altrimenti $(x \gg 1) \oplus \text{0x9908b0df}$.

**XOR**: e' l'unico operatore che combina tra loro bit in $S$. $\land$ e $\lor$ servono a combinare i bit dalle 2 costanti nella formula. Dunque $S_{625}$ e' formato dallo XOR di $S_{398}$, $S_{1}$ e $S_{2}$.

**Linear Combination**: $X \oplus Y \oplus Z$ e' un esempio di combinazione lineare. Una formula con soli XOR ed e' **predicibile**, se inverto un bit in $X$ il valore finale cambia indipendentemente da $Y$ e $Z$.

E quindi boh! se ho capito bene, MT e' debole ad analisi delle combinazioni lineari.


## Random Bits in UNIX
`/dev/urandom`: interfaccia userland per un PRNG crittografico.

```bash
 dd if=/dev/urandom of=<output file> bs=1M count=10
```

**Leggere in modo sicuro da** `/dev/urandom`:
 * usa `O_NOFOLLOW`, `O_CLOEXEC`
 * se `errno == EINTR` riprova correttamente.
 * `FD_CLOEXEC` via `fcntl` se non esiste la flag `O_CLOEXEC`
 * controlla che `/dev/urandom` sia un dispositivo a caratteri
 * `RNDGETENTCNT` 

