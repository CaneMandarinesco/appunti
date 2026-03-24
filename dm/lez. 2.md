* **NumPy**: gestione di array e calcoli numerici
* **Matplotlib**: librerie per la creazione di grafici e disegno di forme.

## KNN
* **definizione**: assegna tag in base agli oggetti piu vicini sul grafico
* **fitting**: `fit` implementa l'apprendimento, consiste nella *memorizzazione dei dati*.
* **predict**: `predict` guarda i $k$ piu vicini, con *distanza euclidea* per assegnare il tag.
* **efficienza**: selection sort (*ad ogni iterazione identifica il piu piccolo*), oppure albero binario per la ricerca dei $k$ piu vicini.
* **moda**: l'elemento piu frequente in una lista.

## numpy e pandas
* `np.array()`: definisce array e matrici per essere usati da numpy.
* `np.arange`: genera un intervallo di numeri per numpy.
* Sia `y=[...]` una lista numpy e `t in y`. Allora `y == t` e' la lista dove un `true` in i-esima posizione indica la presenza di t in y in posizione i-esima.
* Sia `y=[a,b,a,c]` e `t=a`, allora `np.where(y==t)` ritorna gli indici in cui si trova `t`
* `np.unique` ritorna l'array senza duplicati e con `return_counts=True`, ritorna il numero di occorrenze per ogni oggetto. Utile per identificare il massimo.
* `np.concatenate`: concatena le liste
* `np.setdiff1d`: differenza tra liste (come insiemi???)
* `df.iloc`: `iloc` contiene i valori del data frame a cui posso accedere con posizioni integer based.
## modellazione
* suddivisione data set in `X_train` , `y_train` ed `X_test` ed `y_test`.



