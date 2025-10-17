```bash
ss -s
```
mostra statistiche sull'uso delle socket aperte.

```bash
ss -ua
```
mostra le socket `udp`, con `-a` voglio tutte le socket, anche quelle non connesse.

```bash
ss -uanE
```
`-n` mostra le porte come valori numerici, senza tradurle nel servizio corrispondente. `-E` mostra le socket alla loro distruzione!