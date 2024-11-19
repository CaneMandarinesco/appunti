---
cssclasses:
  - wide-page
---
Link utile: https://filippo.io/linux-syscall-table/
### Everything is a file

| Nome descrittivo | Short Name | File Number | Descrizione                   |
| ---------------- | ---------- | ----------- | ----------------------------- |
| Standard in      | stdin      | 0           | Input da tastiera             |
| Standard out     | stdout     | 1           | Output verso la console       |
| Standard error   | stderr     | 2           | Error Output verso la console |
> Ogni processo ha questi 3 file aperti

### Hello World
```c
#include <unistd.h>
#include <stdio.h>
#include <sys/syscall.h>

// e' buona pratica fare cosi
#define STDOUT 1

int main(int argc, char **argv)
{
	char msg[] = "Hello World!\n";
	int nr = SYS_write;
	// fai la chiamata di sistema
	syscall(nr, STDOUT, msg, sizeof(msg));
	return 0;
}
```

> fare le chiamate di sistema direttamente dal codice sorgente e' deprecato.

## Cosa succede quando eseguo una `syscall`?
> Questo e' un piccolo approfondimento, non occorre capire esattamente cosa succede ad ogni linea di codice, ma più o meno, in generale, i passaggi eseguiti.

1. Chiamo `read()` contenuta in `glibc`. Funge da wrapper per la chiamata di sistema 
```c
ssize_t
__libc_read (int fd, void *buf, size_t nbytes)
{
  return SYSCALL_CANCEL (read, fd, buf, nbytes);
}
```

2. Viene chiamata la macro `SYSCALL_CANCEL` che permette di effettuare la chiamata in modo sicuro. (**codice sorgente di** `glibc`)
```c
#define SYSCALL_CANCEL(...)                                      \
  ({									                         \
    long int sc_ret;							                 \
    if (NO_SYSCALL_CANCEL_CHECKING)					             \
      sc_ret = INLINE_SYSCALL_CALL (__VA_ARGS__); 			     \
    else								                         \
      {									                         \
	int sc_cancel_oldtype = LIBC_CANCEL_ASYNC ();			     \
	sc_ret = INLINE_SYSCALL_CALL (__VA_ARGS__);			         \
        LIBC_CANCEL_RESET (sc_cancel_oldtype);				     \
      }									                         \
    sc_ret;								                         \
  })
```

3. Macro `INTERNAL_SYSCALL_CALL`, configura i registri e invoca l'istruzione syscall, passa il controllo da utente a kernel. (**codice sorgente di** `glibc`)
```
#define INTERNAL_SYSCALL(name, nr, args...)				        \
	internal_syscall##nr (SYS_ify (name), args)
```

> ci sono diverse syscall in base a quanti argomenti devo passare.

esempio per 2 argomenti, da notare che vengono chiamate le istruzioni assembly per impostare i registri del processore. Inoltre in quest'operazione non mi interessa il tipo di dato che sto maneggiando.
```c
#define internal_syscall2(number, arg1, arg2)				\
({									                        \
    unsigned long int resultvar;					        \
    TYPEFY (arg2, __arg2) = ARGIFY (arg2);			 	    \
    TYPEFY (arg1, __arg1) = ARGIFY (arg1);			 	    \
    register TYPEFY (arg2, _a2) asm ("rsi") = __arg2;		\
    register TYPEFY (arg1, _a1) asm ("rdi") = __arg1;		\
    asm volatile (							                \
    "syscall\n\t"							                \
    : "=a" (resultvar)							            \
    : "0" (number), "r" (_a1), "r" (_a2)				    \
    : "memory", REGISTERS_CLOBBERED_BY_SYSCALL);			\
    (long int) resultvar;						            \
})
```

4. Ingresso nel kernel, l'istruzione `syscall` provoca automaticamente l'esecuzione di `entry_SYSCALL_64` (scritta in assembly). (**codice sorgente del kernel linux**)
```asm
SYM_CODE_START(entry_SYSCALL_64)
	UNWIND_HINT_ENTRY
	ENDBR

	swapgs
	/* tss.sp2 is scratch space. */
	movq	%rsp, PER_CPU_VAR(cpu_tss_rw + TSS_sp2)
	SWITCH_TO_KERNEL_CR3 scratch_reg=%rsp
	movq	PER_CPU_VAR(pcpu_hot + X86_top_of_stack), %rsp

	[...] // dopo tante righe di codice...

	call	do_syscall_64	

	[...] // altre righe di codice, rilascio il contenuto dei registri
		  // ed esco
```

5. `do_syscall_64` e' una funzione invece scritta in `c`. (**codice sorgente del kernel linux**)
```c
__visible noinstr bool do_syscall_64(struct pt_regs *regs, int nr)
{
	add_random_kstack_offset();
	nr = syscall_enter_from_user_mode(regs, nr);

	instrumentation_begin();

	if (!do_syscall_x64(regs, nr) && !do_syscall_x32(regs, nr) && nr != -1) {
		/* Invalid system call, but still a system call. */
		regs->ax = __x64_sys_ni_syscall(regs);
	}

	instrumentation_end();
	syscall_exit_to_user_mode(regs);

	/*
	 * Check that the register state is valid for using SYSRET to exit
	 * to userspace.  Otherwise use the slower but fully capable IRET
	 * exit path.
	 */

	/* XEN PV guests always use the IRET path */
	if (cpu_feature_enabled(X86_FEATURE_XENPV))
		return false;

	/* SYSRET requires RCX == RIP and R11 == EFLAGS */
	if (unlikely(regs->cx != regs->ip || regs->r11 != regs->flags))
		return false;

	/* CS and SS must match the values set in MSR_STAR */
	if (unlikely(regs->cs != __USER_CS || regs->ss != __USER_DS))
		return false;

	/*
	 * On Intel CPUs, SYSRET with non-canonical RCX/RIP will #GP
	 * in kernel space.  This essentially lets the user take over
	 * the kernel, since userspace controls RSP.
	 *
	 * TASK_SIZE_MAX covers all user-accessible addresses other than
	 * the deprecated vsyscall page.
	 */
	if (unlikely(regs->ip >= TASK_SIZE_MAX))
		return false;

	/*
	 * SYSRET cannot restore RF.  It can restore TF, but unlike IRET,
	 * restoring TF results in a trap from userspace immediately after
	 * SYSRET.
	 */
	if (unlikely(regs->flags & (X86_EFLAGS_RF | X86_EFLAGS_TF)))
		return false;

	/* Use SYSRET to exit to userspace */
	return true;
}
```

6. `do_syscall_64` consulta `sys_call_table` (ovvero un file, `syscall_64.tbl`) per **mappare** il numero della system call alla procedura corretta ( secondo lo standard POSIX ).
```c
0	common	read			sys_read
1	common	write			sys_write
2	common	open			sys_open
3	common	close			sys_close
4	common	stat			sys_newstat
5	common	fstat			sys_newfstat
[...] // altri codici, fino a 500 e oltre!
```


7. esegui la funzione `__x64_sys_read` che richiama `ksys_read` per la logica di lettura.

Riassumendo le fasi sono:
1. Chiamo `read()` contenuta in `glibc`. Funge da wrapper per la chiamata di sistema. (**codice sorgente di** `glibc`)
2. Viene chiamata la macro `SYSCALL_CANCEL` che permette di effettuare la chiamata in modo sicuro. (**codice sorgente di** `glibc`)
3.  Macro `INTERNAL_SYSCALL_CALL`, **configura i registri** e invoca l'istruzione syscall, passa il controllo da utente a kernel. (**codice sorgente di** `glibc`)
4. Ingresso nel kernel, l'istruzione `syscall` provoca automaticamente l'esecuzione di `entry_SYSCALL_64` (scritta in assembly). (**codice sorgente del kernel linux**)
5. `do_syscall_64` e' una funzione invece scritta in `c`. (**codice sorgente del kernel linux**)
6. `do_syscall_64` consulta `sys_call_table` (ovvero un file, `syscall_64.tbl`) per **mappare** il numero della system call alla procedura corretta ( secondo lo standard POSIX ).
7. esegui la funzione `__x64_sys_read` che richiama `ksys_read` per la logica di lettura.

Il mapping e' eseguito dalla funzione `x64_sys_call` (dentro un'altro file ancora):
```c
#define __SYSCALL(nr, sym) case nr: return __x64_##sym(regs);
long x64_sys_call(const struct pt_regs *regs, unsigned int nr)
{
	switch (nr) {
	#include <asm/syscalls_64.h>
	default: return __x64_sys_ni_syscall(regs);
	}
};
```
## Gestione dei Processi
* `pid_t fork()`:  duplica il processo corrente
	* nel padre ritorna il `pid_t` del figlio
	* nel figlio ritorna `0`, cosi ho modo di capire dal codice se sto eseguendo il processo figlio o il processo padre
* `pid_t wait(int *wstatus)`
	* attende che i processi figli cambino stato, magari per via di un `exit`
	* scrive lo status in `wstatus`

Esempio di uso di un `fork`:
```c
void main(void){
	int pid, child_status;
	if (fork() == 0) {
		// fai qualcosa, sto eseguendo il processo figlio!
	} else {
		// fai qualcos'altro, sto eseguendo il processo padre!
		wait(&child_status);
	}
}
```

Ora puo' essere utile introdurre `execv` per **rimpiazzare il programma binario** del figlio, cosi possono eseguire due programmi completamente diversi:
```c
void main(void) {
	int pid, child_status;
	char *args[] = {"/bin/ls", "-l", NULL};
	if(fork() == 0) {
		execv(args[0], args); // esegui il codice binario di `/bin/ls` con argomento -l
	} else {
		wait(&child_status);  // aspetta il figlio!
	}
}
```

### Segnali
L'utente finale (inteso come programmatore) puo' gestire autonomamente i segnali creando dei `signal handler`. 

```c
#include <stdio.h>
#include <signal.h>
#include <unistd.h>
#include <stdlib.h>

void alarm_handler(int signal) {
	printf("Catturato il segnale: %d\n", signal);
	exit(0);
}

int main(int argc, char **argv) {
	signal(SIGALRM, alarm_handler);
	alarm(1); // l'allarme verra' mandato tra 1 secondo

	printf("Loop infinito...\n");
	while(1) {
	}
	return 0;
}
```

### Comunicazione con la pipe
Alcune chiamate di sistema che useremo:
* `int open(const char *pathname, int flags)`: apre un file
* `int close(int fd)`: chiude il file.
* `int pipe(int pipefd[2])`: crea una pipe con due file descriptor, uno per l'input e uno per l'output.
* `int dup(int oldfd)`e `int dup2(int oldfd, int newfd)`

Le funzioni `dup` e `dup2` si usano in questo modo:
```c
int fd = open("dup.txt", O_WRONLY | O_APPEND);
int copy_fd = dup(fd);
write(copy_fd, "Ciao\n");
write(fd     , "Ciao\n");
```
Entrambi scriveranno sullo stesso file. Uso 2 file descriptor diversi per scrivere sullo stesso file.

```c
int file_fd = open("output.txt", O_WRONLY | O_CREAT, 0644);
dup2(file_fd, STDOUT_FILENO); // renderizza stdout su file_fd
printf("Ora l'output va su output.txt\n");
close(file_fd);
```
Ora il file descriptor per lo standard output (`STDOUT_FILENO`) e' direzionato verso il file descriptor `file_fd`
> `STDOUT_FILENO = 1`, ossia lo **standard output**.
 