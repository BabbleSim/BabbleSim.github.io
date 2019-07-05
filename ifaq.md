## Infrequently asked questions

### When running a Phy or a device in valgrind I get a memory leak in `getpwuid()` of 40 bytes

If running in Debian or Ubuntu 16.04, edit ```/usr/lib/valgrind/default.supp``` and add at the end the following

```
{
   getpwuid() issue
   Memcheck:Leak
   match-leak-kinds: reachable
   fun:malloc
   fun:realloc
   fun:load_blacklist
   fun:bindresvport
   fun:__libc_clntudp_bufcreate
}
```

You may want to append the whole ```/usr/lib/valgrind/debian.supp``` there


For Ubuntu 18.04 set it as follows:
```
{
getpwuid() issue
Memcheck:Leak
match-leak-kinds: reachable
fun:malloc
fun:realloc
fun:load_blacklist
fun:bindresvport
}
```