### Debugging a simulated device

Each device in the simulation runs as an independent Linux program communicating
with the rest of the simulation components using libPhyCom.
libPhyCom is designed so that all devices in the simulation will be blocked
waiting for any other device when necessary.

This means that it is possible to instrument any of the devices in a fully
transparent manner for the rest. This includes running any number of devices
in debuggers.
If an instrumented device is stopped in, say, a breakpoint, all other devices
will be waiting blocked in libPhyCom.

When devices are run as native host applications (like for example using
[Zephyr's nrf52_bsim](https://docs.zephyrproject.org/latest/boards/posix/nrf52_bsim/doc/index.html)
) you can use gdb to debug them.
It is up to you to chose the debugger you prefer.
If you do not have any preference, you may want to use gdb with
[Eclipse CDT](https://www.eclipse.org/cdt/)
as a GUI for it.
To debug segmentation faults, uninitialized variables issues, or memory leaks
you may want to consider
[valgrind](http://www.valgrind.org/)'s
default tool memcheck.
You may also want to use [gcov](https://gcc.gnu.org/onlinedocs/gcc/Gcov-Intro.html#Gcov-Intro)
for test coverage analysis, or
[valgrind's callgrind](http://valgrind.org/docs/manual/cl-manual.html)
for rough runtime profiling.