`ss`: sta per socket statistic.
* `ss -s` mostra le socket aperte per categoria. `INET` e' la somma di tutte le socket aperte.
* `ss -u` mostra le socket udp **connesse**
* `ss -ua` mostra le socket in tutti gli stati
* `ss -uan` mostra le socket senza traduzione della porta usando `/etc/services`
* `ss -uanE` mostra le socket alla distruzione

## `udpserver.c`
```c
int sockfd = socket(AF_INET, SOCK_DGRAM, IPPROTO_UDP);
if(sockfd == -1) err;
```
* `AF_INET`: usa `IPv4`
* `SOCK_DGRAM`: usa datagrammi
* `IPPROTO_UDP`: usa `UDP`. Poteva essere omesso e inferito dai primi due (dato che ho specificato che uso datagrammi).