## BabbleSim architecture

In BabbleSim, the physical layer simulation (Phy) and each device run as
independent programs each in their own Linux process(es).

In a simulation there will normally be 1 Phy, and a set of devices.
Starting a simulation consists on executing that Phy and devices.
Once all devices have connected to the Phy, the simulation will
start.

Each device instructs the Phy each time it wants to transmit, receive, or
perform an RSSI measurement.
The Phy is in charge of emulating the shared medium, and of blocking each
device when necessary to align its transmissions and receptions with the others.
This will be done until the simulation ends: until the simulation duration has
been reached or all devices have disconnected.

The communication between the device and the phy is done with a well defined
protocol for that Phy. For the 2G4 Phy you can find it
[here](https://github.com/BabbleSim/ext_2G4_libPhyComv1/tree/master/docs)

A C library with both a blocking and a non blocking API is provided to ease
building devices which connect to a Phy.
For those so inclined, it is not even necessary to use this library: The
device-phy protocol can also be implemented by hand.

As the only requirement for a device to connect to a Phy is to either call
functions in this C library (or implement this protocol in some other way), it
is possible to have devices built in almost any programming or scripting
language; to mix 32 and 64 binaries; to model them at completly different
levels of abstraction; or to run some of them in simulators/emulators.
You may run any device in a debugger or instrument it in any other way without
affecting the simulation results.


#### How devices and the Phy know about each other.

Simulations are identified by a "simulation identification string" and the user
who launched them.
Moreover each device is indentified, towards a simulation Phy, by its device
number.

For cases in which several Phys are present in a simulation (due to having
several shared mediums, e.g. Ethernet & BLE), a Phy is identified by a
"Phy identification string".
In general a device also has a "global device number" which identifies it
uniquely inside a simulation.
The most typical case is that only 1 Phy is running, and that a device "global
device number" is equal to its device number in that Phy.

Typically the simulation identification string will be provided with the `-s=<s>`
command line option to the phy and devices; The device number for each device
is given with the `-d=<d>` option; And the Phy is told how many devices it
should expect in the simulation with the `-D=<D>` option.
Note that the Phy expects devices to be numbered from `0` to `D-1`.
