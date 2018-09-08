## BabbleSim architecture

In BabbleSim the phy and each device run in their own independent processes.

In a simulation there will normally be 1 Phy, and a set of devices.
Starting a simulation consists on executing that Phy and devices.
Once all devices have connected to the Phy, the simulation will start to be executed.

Each device instructs the phy each time it wants to transmit, receive or perform an RSSI measurement.
The Phy is in charge of emulating the shared medium, and of blocking each device until their transmission or reception has finished.
This will be done until the simulation end: until the simulation duration has been reached or all devices have disconnected.

The communication between the device and the phy is done with a well defined protocol which is defined for each type of Phy.

A C library with both a blocking and a non blocking API is provided to ease connecting the device into a Phy.
For those so inclined, it is not even necessary to use this library: The device-phy protocol can also be implemented by hand.
(TODO: link to UML sequence diagrams and decription of the interface for 2G4)

As the only requirement for a device to connect to a Phy is to either call functions in this C library (or implement this protocol in some other way), it is possible to have devices built in almost any programming or scriptong language; to mix 32 and 64 binaries; to model them at completly different levels of abstraction; or to even run some of them in simulators/emulators.


#### How devices and the Phy know about each other.

Simulations are identified by a "simulation identification string" and the user who launched them. This is provided to each device and the Phy when launching them typically with the `-s` command line option.

A Phy inside a simulation is identified by a "Phy identification string". This can also be provided by the devices and Phy but it defaults to a sensible value (e.g. "2G4" for the 2G4 Phy). 

Each device is indentified towards a simulation Phy, by its device number. This is typically given with the `-d` option.
In general a device also has a "global device number" which identifies it uniquely inside a simulation. Typically and by default these to will be equal.

The Phy is told how many devices it should expect in the simulation with the `-D` option.
