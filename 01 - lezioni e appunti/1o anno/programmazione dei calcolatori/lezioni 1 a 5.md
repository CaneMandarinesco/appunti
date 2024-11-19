### tipi di dati numerici
sono supportati: interi, float e complessi
operazioni importanti da ricordare:
* `x//y`: divisione intera. tra interi ritorna intero e tra float ritorna float
* `x%y`: resto della divisione
* `x**y`: x elevato alla y

### float(x)
`float(x)` restituisce un float a partire da un numero o da una stringa.
se l'input e' una stringa può anche essere *NaN* (Not A Number), in quel caso si ottiene un errore di tipo `ValueError`
* per infinito: `float('inf') = inf`
* se il numero e' troppo grande per essere gestito da python abbiamo un `OverflowError`

### es 1. x e' pari o dispari?
```python
if x%2 == 0:
	print(x, ' e\' pari e: ', x, '= 2*', x//2 )
else:
	print(x, ' e\' dispari e: ', x, '= 2*', x//2, '+ 1')
```

### es 2. x e' una vocale?
```python
while i < len(x):
	if x[i] in 'aeiouAEIOU':
		numero_vocali = numero_vocali + 1
```


### es 3. puo' x essere convertito in float?
```python
x = input

# 1a versione
i = 0
while i < len(x) and ( (x[i] in '0123456789') or (x[i] == "." and numero_punti == 0)):
	if x[i] == ".":
		numero_punti += 1
	i+=1

if i == len(x):
	x = float(x)
else:
	x=0

# 2a versione
i = 0
while i < len(x) and ( (x[i] >= '0' and x[i] <= '9') or (x[i] == '.' and numero_punti == 0) ):
    if x[i] == '.':
        numero_punti += 1
    i += 1

```

### es 4. x precede y?
il codice originale mi pare rotto e non si capisce cosa deve fare secondo me. quindi riporto una mia versione del programma, fatta come esercizio.

### es 5. sostituire spazi con `_`
> [!warning] attenzion!
> le stringhe sono immutabili! non e' consentito modificare un carattere di una stringa

```python
x = 'programmazione python e c'
y = ''
n = len(x)

for c in x:
	if c == ' ':
		y = y+'_'
	else:
		y = y+c
```
questo codice viene seguito in tempo **quadratico**, perche' ad ogni iterazione di `c in x`, quando scriviamo `y+carattere` viene **creata una copia** di y, operazione che in base alla lunghezza di n, puo' essere molto costosa.

### es 6. palindromo

```python
def palindromo( a ):
    b = ''
    for c in a:
        b = c+b
    return a == b
    
print(palindromo("ciao")) # False, ovviamente ciao non e' palindroma
print(palindromo("radar")) # True
print(palindromo("rAdar")) # False
```

come nell'esercizio sopra, anche questa e' **quadratica**: $o(n^2)$ 

### es 7. palindromo con indici negativi
```python
def palindromo( a ):
    i, n = 0, len(a)
    
    while i < n//2:
        if a[i] != a[-i-1]: # per esempio per i=0, allora comparo il primo elemento con l'ultimo
            return False
        i += 1
    return True

print(palindromo('radar'))    
print(palindromo('rAdar')) 
```
ha complessità temporale **lineare**: $n$, perché' non sto più eseguendo una copia di una stringa, per poi fare un confronto tra le due, ma sto piuttosto comparando $n$ volte (al caso peggiore) i caratteri 
### es 8. slicing e palindromo con slicing
operazione per estrarre una serie di caratteri da una stringa.
```python
a = '0123456789'

a[2:5] # dall'elemento in 2 fino all'elemento in 5 escluso
# output: 234

a[1:8:2] # dall'elemento in 1 fino all'8, a salti di 2
# output: 1357

a[8:1] # errore
# output: non ritorna niente
a[8:1:-1] # dall'elemento in 8 fino al 1o a salti di -1 (andando in dietro)
# output: 8765432 (1 escluso!)
a[1:8:-1] # errrore
# output: non ritorna niente

a[:4:2] # fino a 4 a salti di 2
# output: 02 (4 escluso!)

a[1::2] # da 1 alla fine a salti di 2
# output: 13579

a[::-1] # dall'inizio alla fine a salti di -1
# output: 9876543210
```

```python
def palindromo(a):
	return a == a[::-1]
```
quest'ultima implementazione ha complessità' temporale lineare: $n$, perché' stiamo con `a[::-1]` stiamo copiando $n$ caratteri da `a`.


