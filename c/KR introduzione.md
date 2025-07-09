# scrivere elegantemente e correttamente

## es 1
* bisogna scegliere accuratamente il tipo delle variabili, `c` e' di tipo `int` perché `getchar` ritorna un `int` dato che può ritornare il valore `EOF`.
* **ogni espressione in c ritorna un valore**
```c
int main(int argc, char **argv){
	int c;
	c = getchar();
	while(c != EOF) {
		putchar(c);
		c = getchar();
	}
}
```

Lo stesso codice diventa:
```c
int main(int argc, char **argv){
	int c;
	while((c = getchar()) != EOF)
		putchar(c);
}
```

# input output
In `c` un `text stream` e' una sequenza di caratteri divisa in linee, dove ogni linea finisce con un carattere `\n`. 

* in alcune macchine gli `int` sono rappresentati a `16bit`, quindi hanno un range limitato di valori. `long` e' come `int`, ma garantisce una lunghezza di minimo `32bit`.
* `%.0f` stampa un **float o double** senza la parte decimale.
* **OSS**: `%6.2f` stampa sei cifre di cui 2 decimali

Il seguente codice conta i caratteri inseriti fino all'`EOF`.
```c
int main(int argc, char **argv){
	double nc;
	for (nc = 0; gechar() != EOF; ++nc)
		;
	printf("%.0f\n", nc);
}
```
