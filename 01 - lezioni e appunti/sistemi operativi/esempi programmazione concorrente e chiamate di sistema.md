---
cssclasses:
  - wide-page
---

Analisi del codice su microsoft teams.

>[!note] OSS.
> il comando `man` funziona anche sulle chiamate di sistema per `c`. Posso quindi scrivere `man execl`, `man write`, `man fork`, ecc... .
## Chiamate di sistema
### `write`
`write` e' una funzione che scrive una sequenza di byte dentro un file. L'accesso al file avviene grazie ad un file descriptor. Nel codice seguente scrivo dentro lo **standard output**. 
```c
#include <unistd.h>
#include <stdio.h>

int main() {
	char msg[] = "Hello World!\n";
	write(STDOUT, msg, sizeof(msg));

	return 0;
}

```
`write` e' un wrapper per la chiamata di sistema con codice `SYS_write`. infatti posso chiamare direttamente: 
```c
int main(int argc, char **argv)
{
	char msg[] = "Hello World!\n";
	int nr = SYS_write;
	syscall(nr, STOUD, msg, sizeof(msg));
	return 0;
}
```

In [[programmazione in c con le system calls]], e' descritta la procedura di una chiamata di sistema.
### Fork dei processi

In questo esempio uso la chiamata `fork` per duplicare il processo. Il figlio eseguirà quindi lo stesso codice del padre, a partire dal main. L'esecuzione del codice prende strade diverse dal momento in cui `fork`, quando viene eseguito dal figlio, restituisce valore 0.

La chiamata `wait` invece aspetta che il figlio cambi stato, nel senso che riceva un segnale di **terminazione**, di **stop** o di **resume** (non so che intenda di preciso). E' una chiamata bloccante, l'esecuzione riprende dopo che il figlio ha ricevuto un segnale.

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main(){
	int pid, child_status;

	if ((pid = fork()) == 0) {
		printf("Sono il processo figlio, la fork ritorna PID %d\n", pid);
	} else {
	wait(&child_status); // Aspetta che child riceva un segnale: STOP, TERMINATO, o di RESUME.
						 // e' una chiamata bloccante
	
		printf("Sono il processo padre, il figlio ha PID %d, e ha stato: %d\n", pid, child_status);
	}
}
```

### segnali

Questo codice definisce il comportamento del processo nel caso in cui venga ricevuto il segnale `SIGINT`, che viene mandato di solito quando sul terminale faccio `CTRL+C`.  `signal` permette di registrare una funzione, detta `handler`, per gestire un particolare segnale.
```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>

void signalHandler(int signum) {
	printf("Ricevuto interrupt con segnale %d, ossia: %s", signum, strsignal(signum));
	exit(signum);
}

int main() {
	signal(SIGINT, signalHandler); // ora il segnale SIGINT verra' gestito dalla funzione signalHandler
								   // SIGINT e' il segnale che viene mandato da CTRL+C

	while(1) {
		printf("Dormo...\n");
		sleep(1);
	}

	return 0; // mai raggiunta lol
}
```

Esempio per `SIGALRM`:
```c
void alarm_handler(int signal) {
	// fai qualcosa...
	exit(0);
}

int main(int argc, char **argv) {
	signal(SIGALARM, alarm_handler);
	alarm(1); // schedula l'invio di un'allarme tra 1 secondo
	while(1) {
		printf("Dormo...\n");
		sleep(1);
	}

	return 0;
}
```

### Pipe
Nella: [[lez. 4 (struttura di un sistema operativo, introduzione a bash)]], e' stato visto come e' possibile concatenare insieme piu processi, collegando tra loro input e output. Vediamo come sfruttare la Pipe usando:
* `pipe(fd)`: crea un canale di comunicazione tra due processi. Come input ha bisogno di un puntatore ad un array di 2 interi dove verranno messi i **file descriptor** per la pipe in input e in output.
* `exece(path, arg, ..., NULL)`: la chiamata appartiene alla famiglia delle chiamate che si basano su `exec`. Questa in particolare permette di fornire una lista di argomenti da passare al programma da eseguire.
```c
#include <stdio.h> 
#include <stdlib.h>
#include <unistd.h> 

#define STDIN 0
#define STDOUT 1
#define PIPE_RD 0 // pipe verso l'input
#define PIPE_WR 1 // pipe verso l'output

int main(int argc, char** argv) {

	__pid_t cat_pid, sort_pid; 
	int fd[2]; // array per input e output della pipeline, fd sta per file descriptor

	pipe(fd);  // crea pipe in entrata e in uscita, metti i file descriptor in fd

	// duplica il processo
	cat_pid = fork();

	if ( cat_pid == 0 ) {
		close(fd[PIPE_RD]);                            // non mi serve leggere l'input
		close (STDOUT);                                // chiudi lo standard output
		dup(fd[PIPE_WR]);                              // ora lo STDOUT del processo viene reindirizzato verso PIPE_WR
		execl("/bin/cat", "cat", "names.txt", NULL);   // esegui il binario di cat, con argomento il file names.txt, bisogna aggiungere alla fine un argomento di valore NULL
		exit(0); // non andrebbe aggiunto per sicurezza?
	}

	// duplica il processo
	sort_pid = fork();

	if ( sort_pid == 0 ) {
		close(fd[PIPE_WR]);                              // chiudi PIPE_WR, non mi serve
		close (STDIN);                                   // chiudi lo STDIN
		dup(fd[PIPE_RD]);                                // rendirizza STDIN verso PIPE_RD
		execl("/usr/bin/sort", "sort", NULL);

		exit(0); // non andrebbe aggiunto per sicurezza?
	}

	
	close(fd[PIPE_RD]); // e' buona prassi chiudere i file
	close(fd[PIPE_WR]);
	
	/* wait for children to finish */ 
	waitpid(cat_pid, NULL, 0);      // ora aspetta che finscono l'esecuzione
	waitpid(sort_pid, NULL, 0);  
			
	return 0; 
}
```


### Bash!
Questa e' un'implementazione molto minimale di una bash. 


```c
void free_args(char** args) {
    int i = 0;
    while (args[i]) {  // finche' non arrivi all'argomento nullo
        free(args[i]); // libera memoria occupata dal singolo argomento
        i++;
    }
}
```


funzioni utilizzate:
* `getline(input, len, stream)`: legge un intera riga da uno stream dati.
* `strtok(s, delimitatore)`: prende una stringa s in input e la divide in token, delimitati da uno o più caratteri definiti dal `delimitatore`. per usare `strtok` si fa prima una chiamata del tipo `strtok("Hello World, This is C.", " ")`, dove lo spazio e' il mio carattere delimitatore, e la funzione ritorna quindi il primo token: `Hello`, infatti subito dopo il carattere `o` c'e' uno spazio. Ora per ottenere gli altri token bisogna chiamare `strtok(NULL, " " ")`, fino a che non esaurisco i token. Il motivo per cui devo passare `NULL` e' che la stringa da manipolare e' gestita in modo statico dalla libreria, ogni volta che passo un elemento non nullo alla chiamata `strtok` cambio la stringa su cui opero.
```c
void read_command(char* cmd, char** args) {
    char* input = NULL;
    size_t len = 0;
    printf("myshell> ");
    ssize_t read_len = getline(&input, &len, stdin);

    if (read_len == -1) {  // L'utente ha premuto CTRL+D
    	//Quando l'utente preme CTRL+D, la funzione getline restituirà un valore negativo. 
    	//Puoi utilizzare questo per rilevare un EOF e agire di conseguenza.
        printf("\n");  // Per andare a capo dopo CTRL+D
        exit(0);  // Chiudi il shell
    }
    
    char* token = strtok(input, " \n"); // Dividiamo input su spazi e eventuali "\n" (alla fine della stringa)

    int i = 0;
    while (token) {
        args[i] = strdup(token); // Usa strdup per duplicare e assegnare memoria al token
        if (i == 0) {
            strcpy(cmd, token);  // il primo token dovrebbe sempre essere il nome 
        }
        token = strtok(NULL, " \n"); // passo NULL, invece di input! strtok mi da in automatico il prossimo token
        i++;
    }
    args[i] = NULL;
    free(input);
}

```

```c
int main() {
    while (1) {
        char cmd[256], *args[256];
        int status;
        pid_t pid;

        read_command(cmd, args);

        if (strcmp(cmd, "exit") == 0) {
            exit(0);
        }

		// duplica il processo, per eseguire il comando letto
        pid = fork();

        if (pid < 0) {
            perror("fork failed");
            exit(1);
        }

        if (pid == 0) {
	        // esegui il comando inserito
            if (execvp(cmd, args) == -1) {
                perror("execvp failed");
                exit(1);
            }
        } else {
	        // aspetta che il comando finisca l'esecuzione
            if (wait(&status) == -1) {
                perror("wait failed");
                exit(1);
            }
        }

        //gestisce lo "spreco di memoria" introdotto da strdup
        // prima di perdere il riferimento alle stringhe in args, libera il loro contenuto
        free_args(args);
    }
    return 0;
}
```