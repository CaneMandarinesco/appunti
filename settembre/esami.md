* reti
* fondamenti: 5 settembre
* lmp: 15 settembre
* (algebra lineare)


|                 | Lunedi                        | Martedi | Mercoledi                                         | Giovedi | Venerdi                     | Sabato               | Domenica                                            |
| --------------- | ----------------------------- | ------- | ------------------------------------------------- | ------- | --------------------------- | -------------------- | --------------------------------------------------- |
| **fine luglio** | **Fond**: Introduzione        |         | **Reti: intro**<br>+ **Fond**: Macchine di turing |         | **Reti: APP**               | **Fond:**<br>Ripasso | **Reti**: Es. HTTP<br>Es. Capitolo 1<br>Es. DNS     |
| 1a: **agosto**  | **Fond**: Es. Macchine Turing |         | **Reti: Livello TRA.**                            |         | **Reti**: Es. UDP + Ripasso |                      | **Reti: Livello RETE**                              |
| 2a              |                               |         | **Reti**: Es. Socket UDP, TCP, rete, ping         |         | **Reti: Livello LINK**      |                      | **Reti:** Es. traceroute, ip addr, arp, virtual net |
| 3a              |                               |         | **Reti**: **ripasso**                             |         | **Reti: Wireles**           |                      | Reti: Es. invio host, indirizzi, nc, vari           |
| 4a              |                               |         |                                                   |         |                             |                      |                                                     |
|                 |                               |         |                                                   |         |                             |                      |                                                     |
|                 |                               |         |                                                   |         |                             |                      |                                                     |
|                 |                               |         |                                                   |         |                             |                      |                                                     |

# reti

| argomento        | n. slide | n. pagine | Fatto |
| ---------------- | -------- | --------- | ----- |
| *Introduzione*   | 3        | ...       | ❌     |
| **Livello APP.** | 4        | ...       | ❌     |
| Es. HTTP         | 1        | 12        | ❌     |
| Es. Capitolo 1   | 1        | 10        | ❌     |
| Es. DNS          | 1        | 22        | ❌     |
| **Livello TRA.** | 4        | ...       | ❌     |
| Es. UDP          | 1        | 4         | ❌     |
| **Livello RETE** | 5        | ...       | ❌     |
| Es. Socket UDP   | ...      | ...       | ❌     |
| Es. Socket TCP   | ...      | ...       | ❌     |
| Es. Livello Rete | 1        | 18        | ❌     |
| Es. Ping         | 1        | 8         | ❌     |
| **Livello LINK** | 3        | ...       | ❌     |
| ~~Es. AS~~       | 1        | 8         | ❌     |
| ES. traceroute   | 1        | 6         | ❌     |
| ~~Es. link~~     | 1        | 38        | ❌     |
| Es. ip addr      | 1        | 8         | ❌     |
| Es. arp          | 1        | 10        | ❌     |
| Es. virtual net  | 2        | 54        | ❌     |
| **Wireless**     | 2        | ...       | ❌     |
| Es. invio host   | 1        | 8         | ❌     |
| ~~Es. CSMA~~     | 1        | ...       | ❌     |
| Es. indirizzi    | 1        | 3         | ❌     |
| Es. aggiunta nc  | 1        | 3         | ❌     |
| Es. vari         | 1        | 28        | ❌     |
# fondamenti

| argomento                                                                            | n. pagine | n. slide | Fatto |
| ------------------------------------------------------------------------------------ | --------- | -------- | ----- |
| Introduzione                                                                         | 18        |          | ❌     |
| Macchine di Turing                                                                   | 14        |          | ❌     |
| Es: Macchine di Turing                                                               | 10        |          |       |
| Linguaggi                                                                            | 15        |          | ❌     |
| Church-Turing                                                                        |           | 3        | ❌     |
| Halting Problem                                                                      | 9         |          | ❌     |
| Es: accettabilita linguaggi                                                          | 5         |          |       |
| Macchina Turing Universale<br>+ Pascal Min.                                          |           |          | ❌     |
| Grammatiche: (Gerarchia <br>Chomsky, L accettabile, <br>context-free, automi a pila) |           |          | ❌     |
| Linguaggi e Complessita                                                              | 28        |          | ❌     |
| Es: linguaggi e complessita                                                          | 9         |          |       |
| Esercizi: Linguaggi e compl.                                                         | 9         |          | ❌     |
| Linguaggi e Problemi                                                                 |           |          | ❌     |
| Classe P                                                                             |           |          | ❌     |
| Classe NP                                                                            |           |          | ❌     |
| Es: P e NP                                                                           |           |          | ❌     |

**Parte 1, calcolabilità:**

- Ancora sulle Macchine di Turing: trasduttori e riconoscitori, Macchine ad un nastro e a k nastri, Macchine  deterministiche e non deterministiche. Macchina di Turing universale.   
- Linguaggi e funzioni. Linguaggi e linguaggi complemento. Accettabilità e decidibilità di linguaggi. Calcolabilità di funzioni. La tesi di Church-Turing e modello di calcolo equivalenti alla macchina di Turing.

- Linguaggi non decidibili. L'Halting Problem.

- Riducibilità tra linguaggi

- Grammatiche formali. Gerarchia di Chomsky. Grammatiche e calcolabilità. Linguaggi di tipo 2 e automi a pila. Linguaggi di tipo e e automi a stati finiti.

**Parte 2, complessità:**

- Misure di complessità: DTIME, DSPACE, NTIME, NSPACE.   
- Classi di linguaggi. Gap theorem e funzioni time-constructible. Teoremi di compressione e di accelerazione. Definizione delle classi di complessità spaziale e temporale: P, NP, PSPACE, NPSPACE, EXP, coNP, FP. Inclusione, inclusione propria, coincidenza. Riducibilità polinomiale. Chiusura. Completezza.

- Problemi e codifiche. Problemi decisionali e loro complemento.   
- La classe NP. Linguaggi NP-completi e Teorema di Cook. Chiusura rispetto alla riduzione polinomiale. Esempi di dimostrazioni di NP-completezza: 3-SAT, Vertex Cover, 3-Colorability, Colorability, Independent Set, Clique, Hamiltonian Circuit/Path, Longest Path, Commesso Viaggiatore. Problemi NP-intermedi: il teorema di Ladner.