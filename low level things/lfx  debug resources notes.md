* https://www.youtube.com/watch?v=FdNIiQxwJuk&pp=ygUPbGZ4IGxpbnV4IGRlYnVn
* https://docs.kernel.org/process/debugging/index.html
* https://elinux.org/Debugging_The_Linux_Kernel_Using_Gdb
* https://wiki.ubuntu.com/Kernel/KernelDebuggingTricks
* https://docs.freebsd.org/en/books/developers-handbook/kerneldebug/
* https://lwn.net/
* ftrace: https://lwn.net/Articles/365835/


## kernel panic
**Q: What are the reasons why kernel would panic? Explain how you would go about debugging and fixing a bug that causes panics.**

A kernel panic is ca
A kernel panic is caused by an hardware failure or by a bug inside the kernel. It can caused by a null pointer deference, a deadlock, a race conditions, driver errors, memory errors and so on.

A kernel panic implies that the machine halts, since it denotes a sever failure. If not, the kernel just issues the oops to the user ance continues the execution. This is possible because kernel can resist to some kind of errors. To debug this type of errors, the developer must refer to the oops. An oops has information about: error description, register status, back trace.

> `*(int *)0 = 0;` this line of code causes a NULL pointer deference. (from Linux Device Drivers).

A common type of oops is the NULL pointer deference, You can analyze this oops just looking at what registers where NULL, and in general some register will have unexpected values.

With some utilities you can translate the oops address to mnemonic names, you can compile the kernel to do su with the flag `CONFIG_KALLSYMS`.

Additionally i would use `ftrace` to analyze the sequence of procedures called.

Tools that i would use:
* `KASAN`: since kernel panic could happen due to addressing errors. Viene mostrato un report per ogni bug di memoria rilevato, il report include informazioni specifiche come lo stac ktrace relativo all'allocazione dell'oggetto e alla free.
* `KMSAN`


