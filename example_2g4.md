## A simple example with the 2.4GHz Phy

After you have [fetched](fetching.md) and [built](building.md) all simulator
components, you can run a simple example as follows.

Note: Here we assume ${BSIM_OUT_PATH} is the path where the simulator compile
output is stored.
In the previous examples this was `~/bsim/`

```
cd ${BSIM_OUT_PATH}/bin
./bs_2G4_phy_v1 -s=hello_world -D=2 -sim_length=50e6 &
./bs_device_handbrake -s=hello_world -d=0 -r=10 &
./bs_device_time_monitor -s=hello_world -d=1 -interval=100e3
```

In this simple example:

* "hello_world" is used as the simulation identification string.
* The 2.4GHz Phy is told that 2 devices will connect to it.
* We also tell the Phy to stop the simualtion after 50 seconds (50e6
  microseconds)
* The handbrake device is run as the first (`-d=0`) device which connects to
  the Phy. This device slows down a simulation to a given real time speed
  ratio. In this case, we set the ratio to 10x with `-r=10`.
* The time monitor device is run as the second (`-d=1`) device in the
  simulation. This device will monitor the speed of the simulation. We
  configure it to monitor with a 100ms interval (`-interval=100e3`).

As you can see, each of these devices is a separate program, run independently.
Each of them could have been run from a separate terminal.
All that is required for them to find each other, is that they are run by the
same user, in the same workstation, and share the simulation ID string.

Note that until the last device has joined the simulation, the Phy is blocking
the 1st device as neccessary.
So devices can be started at arbitrary times without affecting the result.

Now let's make a mistake:

```
./bs_2G4_phy_v1 -s=hello_world -D=2 -sim_length=50e6 &
./bs_device_handbrake -s=hello_world -d=0 -r=10 &
./bs_device_time_monitor -s=hello_world -d=0 -interval=100e3 &
```

As you can see, both devices are assigned the same number. The 2nd device will
realize about this, warn you, and stop.
But, the phy and the 1st device, are still waiting for the simulation to start.

#### Stoping a running/pending simulation

When running complex simulations with a multitude of devices, if some device
crashes at boot, or some mistake is done it can be tedious to fix. Instead you
may want to simply stop all running simulation components. You can do that with
the `stop_bsim.sh` script.
You will find it in
`${BSIM_COMPONENTS_PATH}/common/stop_bsim.sh`.
You can either run the script directly, which will stop all your ongoing
simulations, or provide to it a simulation ID as its only paramter, in which
case it will only stop the processes linked to that simulation.

### Debugging a simulated device

Each device in the simulator runs as an independent Linux program communicating
with the rest of the simulation components using libPhyCom.
libPhyCom is designed so that all devices in the simulation will be blocked
waiting for any other device when necessary.

This means that it is possible to instrument any of the devices in a fully
transparent manner for the rest. This includes running any number of devices
in debuggers.
If an instrumented device is stopped in, say, a breakpoint, all other devices
will be waiting blocked in libPhyCom.

When devices are run as native host applications (like for example using
Zephyr's nrf52_bsim) you can use gdb to debug them.
It is up to you to chose the debugger you prefers. But if you do not
have any preference, you may want to use gdb with Eclipse CDT as a GUI for it.
To debug segmentation faults, uninitialized variables issues, or memory leaks
you may want to consider valgrind's default tool memcheck.
You may also want to use gcov for test coverage analysis, or valgrind's
callgrind for rough runtime profiling.