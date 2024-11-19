# cap. 3
## floats
non tutti i numeri con la virgola sono rappresentati correttamente, per esempio `0.1` viene interpretato da python come un numero molto vicino a `0.1`, ma che e' diverso da `0.1`. 

> nota: in python 2, `print` arrotonda automaticamente le variabili passate come argomento

nella maggior parte dei casi comunque questi errori di arrotondamento non sono fatali per i nostri calcoli, **chill**.

## scopi delle variabili
```python
def f():
	y = 1           # creo una nuova variabile di nome y
					# appartenente a f()
	c = x + y       # x non e' stata dichiarata, ma viene
					# recuperata fuori dalla funzione
	print('c =', c)
	return x

x = 3               # x e' dichiarata fuori da f
y = 2
z = f(x)
print('z =', z)
print('x =', x)
print('y =', y)
```

se creo una variabile in un contesto interno al programma non viene sovra scritta a quella precedente e posso anche accedere ad una variabile che non e' definita nel contesto corrente: per esempio, dentro`f` posso accedere alla variabile `x`.  

esiste uno `stack` delle variabili dove man mano vengono inseriti i riferimenti alle variabili, e vengono eliminate quando il contesto di esecuzione e' terminato.  anche le funzioni vengono inserite nello `stack`!

> ogni contesto ha il suo `stack frame`!

### esempio
```python
def f():
    print(x)

def g():
    print(x)
    x = 1       # x e' locale rispetto a g!

x = 3
f()
x = 3
g()
```

il codice riporta il seguente errore:
```
UnboundLocalError: cannot access local variable 'x' where it is not associated with a value
```
perche' nella funzione `g()` la presenza dell'assegnazione di `x`, indipendentemente da dove e' posizionata, indica che in `g()`, `x` e' una variabile locale, e quando eseguo `print(x)`, la variabile non e' stata ancora inizializzata. 

# cap. 4
## fibonacci
```python
def fib(x):
	# somma di fib(x-1) + fib(x-2)
	global numCalls
	numCalls += 1
	if x == 0 or x == 1:
		return 1
	else:
		return fib(x-1) + fib(x-2)
```
## divide-and-conquer
applichiamo il metodo divide and conquer ad un algoritmo che determina se una parola e' palindroma o no:
```python
def palindromi():
    def isPalindrome(s):
        def toChars(s):
            '''
            converti s in una stringa a lettere maiuscole e rimuovi
            tutti i carateri non letterali
            '''
            s = s.lower()
            ans = ''
            for c in s:
                if c in 'abcdefghijklmnopqrstuvwxyz':
                    ans = ans + c
            return ans
        
        def isPal(s):
            if len(s) <= 1:
                return True
            else:
                return s[0] == s[-1] and isPal(s[1:-1])
        
        return isPal(toChars(s))
```

`divide and conquer` si basa sul fatto che spesso un problema complesso può essere diviso in tanti piccoli problemi piu semplici da risolvere. in questo esempio l'algoritmo, che risiede in `isPal` distingue due casi:
* `len(s) <= 1` -> la mia parola e' per forza palindroma
* `else` -> controllo se `s[0] == s[-1]` (ovvero se prima e ultima lettera sono uguali) e se `s[1:-1]` (ovvero `s` ma senza il primo e l'ultimo carattere) e' palindroma richiamando `isPal`

in questo tipo di algoritmi troviamo sempre una funzione che richiama se stessa ma con parametri diversi, e sempre **piu' semplici**. 

### esempio a lezione: palindromi con loop
```python
def palindromo( a ):
    i, n = 0, len(a)
    
    while i < n//2:
        if a[i] != a[-i-1]: 
            return False
        i += 1
    return True

print(palindromo('radar'))    
print(palindromo('rAdar')) # False!
```

## files
```python
# per scrivere
nameHandle = open('file', 'w')
nameHandle.write(name + '\n')

# per leggere
nameHandle = open('file', 'r')
for line in nameHandle:
	print(line[:-1]) # scrivi ogni riga del file sul terminale senza il carattere `\n`
	pass

nameHandle.close()
```

* `handle.read()` -> tutto il file
* `handle.readline()` -> linea
* `handle.write(s)` -> scrivi alla fine del file
* `handle.writeLines(sl)` -> scrivi una sequenza di elementi 

# cap. 5
## tuple e liste
```python
t1 = ()
t2 = (1, 'two', 3)
print(t1) # ()
print(t2) # (1, "two", 3)
```

posso fare:
* concatenazione: operatore `+`
* moltiplicare il numero di elementi: operatore `*`

come le stringhe, sono **immutabili**! le liste invece sono **mutabili**.

```python
def removeDups(L1,L2):
    for e1 in L1:
        if e1 in L2:
            L1.remove(e1)

L1 = [1,2,3,4]
L2 = [1,2,5,6]
removeDups(L1,L2)
print(L1)
```

ci si aspetterebbe che l'output sia `[3,4]` e invece si ottiene `[2,3,4]`. 
quando rimuovo `e1` da `L1` l'indice di iterazione e' `0`, quindi dopo diventa `1` ma avendo eliminato `e1` da `L1` il prossimo elemento sara' `3` dato che `2` si trova in posizione `0`.  

## dict.
e' un insieme non ordinato, fatto di `keys` e `values`
* `d.keys()`
* `d.values()`
* `d.get(k,v)` -> vi se `k in d` altrimenti `v`
* `del d[k]`

# cap. 9 complessita!

## merge sort
> `log(n)`


> **complessità spaziale**: se non dipende dall'input e' costante quindi `O(1)=O(2)=... `

> quando in un algoritmo la `n` aumenta in modo esponenziale, allora la sua complessità e' `log(x)`


# cap. 10 complessità algoritmi
python usa la **ricerca lineare** (controllo ogni singolo elemento) per determinare  se un elemento e' in una lista, quindi la complessità del caso peggiore e' sempre `Theta(n)`. 

se uso il **binary search** la complessita' diventa: `O(log(n))`