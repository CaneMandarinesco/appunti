#python #input #stringhe 
Per chiedere all'utente di inserire piu dati dalla tastiera usando pero' una sola riga (e premendo quindi una sola volta invio) si usa la funzione `raw_input`.

```python
num1, num2 = map(float, input('Enter a range: ').split(','))
```

### spiegazione
ho intenzione di leggere due numeri e di metterli dentro `num1` e `num2`. ho bisogno di un modo efficiente di convertire tutti i numeri che ottengo da: `input('...').split(',')` e di poterli assegnare alle mie variabili sfruttando la sintassi: `num1, num2 = ...` che si applica con oggetti iterabili.

Per fare questo posso usare la funzione `map`. Questa richiede una funzione ed una elemento iterabile (per esempio una lista). La funzione viene applicata su tutti gli elementi iterabili, i valori in uscita sono a loro volta iterabili.