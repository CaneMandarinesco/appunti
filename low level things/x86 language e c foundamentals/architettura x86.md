Un computer ha:
* ALU
* Control unit
* storage: memoria principale e cache
* input device
* output device

> [!note] pipeline stall
> Se avviene un `cache` miss, allora la pipeline e' bloccata per qualche ciclo!

La ALU fa operazioni aritmentiche ed e' in grado di rappresentare numeri in decimale, virgola fissa, virgola mobile.

> **Nota**: ci sono delle linee di controllo come $EAX_{in}$ ed $EAX_{out}$ per caricare o leggere i regsitri.

Nella control unit, un registro mantiene l'indirizzo della microistruzione da eseguire. Ogni micro-istruzione ha un campo `branch address` che specifica la prossima micro-istruzione.

### memoria cache
La cache e' una `CAM` (Content Addressable Memory), ossia richiedo il valore del campo, e non il suo **indirizzo di memoria** (ossia la chiave). Ci sono 3 tipi di cache `L1, L2, L3`.

La $L1$ e' implementata con flip-flop, dunque efficiente. La $L2$ ha una capacita piu grande, ma e' piu lenta di $L1$.

Di solito esistono due cache $L1$: una per dati, e una per istruzioni.

Dalla $L2$ sono prevelevati dati e istruzioni, mentre la $L3$ e' una cache opzionale.

Se l'informazione indirizzata non si trova in cache, allora una istruzione di `blocco` viene richiesta! Ma come mappo da memoria a blocco nella cache?

* `direct mapping`: faccio $k \pmod {128}$ se per esempio ho 128 entry nella cache.
* `associative`: ho bisogno di una mappatura!
* `set-associative`

In Ogni indirizzo della memoria...

e altre cose inutili!

### register set
![[Pasted image 20251005165530.png]]
![[Pasted image 20251005165706.png]]

Il registro `EAX` viene usato come `accumulator`, ossia come output ausiliario per le operazioni della `ALU`.

Il registro `EBX` viene usato come `index register` per base addressing e come puntatori per il `DS` (Data segment)

Il registro `ECX` viene usato come contatore in alcune istruzione, e `CL` come shift e rotazione per alcune istruzioni

Insomma, i registri sono general purpouse, ma alcuni sono specifici per certi tipi di istruzioni.

I registri `ESI` (source index reg)  ed `EDI` (destination index reg) sono usati per o