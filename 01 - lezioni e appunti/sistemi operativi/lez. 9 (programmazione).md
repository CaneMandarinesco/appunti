```c
#include <unistd.h>
#include <stdio.h>
#include <sys/syscall.h>
#define STDOUT 1

int main(int argc, char **argv)
{
	char msg[] = "Hello World!\n";
	int nr = SYS_write;
	// effettua la chiamata di sistema a read
	syscall(nr, STDOUT, msg, sizeof(msg));
	return 0;
}
```

implementazione della read nel kernel linux:
```c
SYSCALL_DEFINE3(read, unsigned int, fd, char __user *, buf, size_t, count)
{
	return ksys_read(fd, buf, count);
}
```

### Cosa succede????
* chiamo `read()` che si trova in `glibc`

### Process management  e sys call
esempio:
```c
void main(void) {
	int pid, child_status;
	char *args[] = ["/bin/ls", "-l", NULL];
	if (fork() == 0) {
		execv(args[0], args);
	} else {
		wait(&child_status);
	}
}
```

### segnali
devo definire la funzione per ogni segnale"
```c
sighandler_t signal(int signum, sighandler_t handler);
```