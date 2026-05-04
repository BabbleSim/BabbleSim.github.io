# Optimizing runtime speed

Here you have a few tips which may help you speed up your BabbleSim simulations:

* In the Phy disable traces if you don’t need them with the -nodump command line option.
* When building your embedded SW, note that in general, options that have a runtime performance impact in embedded targets will also have it in simulation.
* If you are using Zephyr:
    * By default CONFIG_COVERAGE is enabled by compile.sh, if you are not using it, disable it.
    * By default CONFIG_NO_OPTIMIZATIONS is enabled to ease debugging. But for speed you may want to set CONFIG_SPEED_OPTIMIZATIONS=y instead.
    * Do not enable CONFIG_ASAN or UBSAN unless you want to debug (they have a performance penalty)
    * Disable logging if possible, if not, at least minimize it. If not possible, reduce the verbosity when calling zephyr.exe with -v=0.
    * In general, options that have a runtime performance impact in embedded targets will also have it in simulation.
    * In your Zephyr test/app: Wherever possible use Zephyr kernel synchronization primitives instead of busy waiting with k_busy_wait(). If you need to busy wait for long times increasing the duration to each k_busy_wait() call.
* You will have significantly better performance running in a bare-metal Linux installation than in a virtual machine (WSL2 is a virtual machine). If you cannot, at least, ensure the Intel/AMD virtualization instructions are enabled (both in your Virtual machine and in the computer UEFI/BIOS)
* In your test, end the test as soon as you are done, instead of waiting for a timeout from the Phy. If one device is done in a simulation while others need to continue without it, have that device disconnect from the simulation while others are let to continue.
* Run with reduced verbosity for all simulated devices and the Phy (by default it is `-v=2`, but you can lower it to `-v=0`)

Beware of other programs running in the computer. The impact can be much bigger than you think. Typical offenders are Chrome/Electron based apps.
