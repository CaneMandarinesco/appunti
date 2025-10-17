
# DNS

| lo 16-bit         | hi 16-bit        |
| ----------------- | ---------------- |
| ID richiesta      | FLAG             |
| N domande         | N RR Risposte    |
| N RR Autoritativi | N RR Addizionali |
| sezione           | domande          |
| sezione           | risposte         |
| sezione           | autoritativa     |
| sezione           | addizionale      |
Dove:
* `FLAG` contiene per esempio `rd` (recursion desired) e `ra` (recursion allowed) ed `AA` (risposta autoritativa)
* La sezione autoritativa contiene informazioni rispetto ai nameserver che gestiscono l'area che si vuole risolvere
* La sezione addizionale contiene campi aggiunitivi, come quelli utili per contattare i server atuoritativi
* La sezione delle risposte contiene il campo cercato.

# UDP

| lo 16 bit          | hi 16 bit  |
| ------------------ | ---------- |
| src port           | dest port  |
| lunghezza dei dati | Checksum   |
| payload: data.data | .data.data |

### TCP
Contiene vari campi:
* `Ver`, `ToS`, `ID`
* `Flag`, `Offset`
* `Checksum`
* `port src`
* `port dest`


| lo 16 bit              | hi 16 bit      |
| ---------------------- | -------------- |
| `Ver\|ToS\|len header` | `len totale`   |
| `ID`                   | `checksum`     |
| `TTL`                  | `FLAG\|Offset` |
| ACK                    | seq            |
| `porta src`            | `porta dest`   |
| campi addizionali...   |                |
| payload                |                |
* `ID` serve per identificare il pacchetto
* `ToS` per dire al nucleo della rete che tipo di servizio offre il pacchetto.
* `TTL`: ogni 