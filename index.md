BabbleSim is a simulator of the physical layer of shared medium networks.

Its main objective is to be an aid to develop network protocols and network
devices, and to be able to develop, debug and regression test target code
in a controlled environment.

BabbleSim's physical layer simulation is, by design, meant to support very
heterogenous devices and methods to simulate or run real devices code.
In a simulation, a device may be as simple as a python script, or as
complex as a simulated multi processor system with a multitude of peripherals.

Completely different types of devices may be run together and communicate
in a simulation over a given Phy.
BabbleSim allows this by setting almost no requirements on the way the
rest of the device is simulated.
For a device to be able to run in a simulation all it needs is to relay
to the BabbleSim Phy its transmissions and reception attemps.
You can find an introduction to the
[BabbleSim architecture here](architecture.md)

Different types of shared mediums have different Phys. For ex. BLE radios,
over the 2.4GHz ISM band, use the
[2.4GHz Phy](2G4.md)

In a simulation, several devices are run together with one Phy for each
type of shared medium.
A device is, in general, a network device, for ex. a phone, a wireless headset,
an interferer...

The Phy is in charge of:

* Emulating the channel/shared medium and modem (analog and digital
  demodulation) for each of the devices
* Handling the devices coordination in that medium.

<center>
<object data="Phy_device_split.svg" type="image/svg+xml">
<p style="text-align:center">Diagram of the simulation split between the devices and the phy</p>
</object>
<p style="text-align:center">Phy-device interface</p>
</center>

### BabbleSim is fast

All components of the simulator are designed to run fast, to allow
running long system level simulations with multiple devices in short
time. The simulation speed limit is set by the execution speed of the devices.
The Phy and channel emulation overhead are quite minor: Simulations
with a few simple devices with BLE like activity will run in the order
of 100x-1000x faster than real time in a modern computer with the 2G4_phy

### BabbleSim is modular

By its nature, BabbleSim is highly modular. Reflecting this modular
approach, BabbleSim is organized in a set of separate git repositories.
In general one fetches and builds only the repositories/components one
is interested on.
In BabbleSim a component is understood as a device, phy, or library, which is
typically placed in the `components/` folder.

[You can find the BabbleSim repositories here](https://github.com/BabbleSim).
Typically all users will fetch the
[base repository](https://github.com/BabbleSim/base).

You can find more information on how to fetch the different components
[in this page](fetching.md).

### It is easy to debug any device

By design, the Phy will wait for any device and block other devices
when neccessary.
That means that you can run any device (or several of them), in a
[debugger or instrument them](debugging.md) without affecting the
simulation results.

### Folder structure

[Here](folder_structure_and_env.md)
you can find both the recommended folder structure, as well as
how to work with off-tree components.

### Building

[Find here instructions on how to build](building.md)

### How to use

The best way to understand how BabbleSim works is by trying it.
[Here you can find an example on how to run a simple case](example_2g4.md).

### What BabbleSim includes

To ease implementation and to allow developing consistent devices,
BabbleSim incudes a set of optional libraries which provide
functonalities like tracing, results dumping, command line argument
parsing, random number generation, etc.

Moreover, a set of simple debug aid/ancillary devices are included in the
[base repository](https://github.com/BabbleSim/base).

For BLE (BT Smart) development, it includes:

* A physical layer for BLE devices: The [2G4 Phy](2G4.md).
* [Interferers models](2G4Interf.md)
* 3 selectable channel models:
    * A [cable + N to N attenuator](https://github.com/BabbleSim/ext_2G4_channel_NtNcable)
    * A [N to N atenuator with independent attenautions](https://github.com/BabbleSim/ext_2G4_channel_multiatt) per path
    * A model of [indoors propagation in the 2.4GHz band](https://github.com/BabbleSim/ext_2G4_channel_Indoorv1)
* 2 selectable modem models:
    * A [model which will receive any packet with a configurable BER](https://github.com/BabbleSim/ext_2G4_modem_magic)
    * A [generic BLE modem performance model](https://github.com/BabbleSim/ext_2G4_modem_BLE_simple)
* A device which can re-play back the activity of any device in a a previous simulation run:
  [ext_2G4_device_playback](https://github.com/BabbleSim/ext_2G4_device_playback)

#### Zephyr's NRF52_bsim and the NRF52 HW models

Even though BabbleSim does not set how the CPU is emulated
(if at all), or how the HW should be modelled, the BabbleSim GitHub
organization does contain a git repository with
[models of the NRF52 HW](https://github.com/BabbleSim/ext_NRF52_hw_models).<br>
These models can be used together with
[Zephyr's](https://zephyrproject.org)
[nrf52_bsim board](https://docs.zephyrproject.org/latest/boards/posix/nrf52_bsim/doc/index.html)
to execute Zephyr and use BabbleSim's 2G4 Phy to simulate the BLE communication
over the 2.4GHz ISM band.
A quick guide on how to get this device up and running can be found
[in this board's page](https://docs.zephyrproject.org/latest/boards/posix/nrf52_bsim/doc/index.html#building-and-running)

### Design choices

This is BabbleSim's list of [design choices & objectives](objectives.md)

### License and contributing to BabbleSim

[Here you can find our contribution guidelines and an introduction to BabbleSim's license](contribution_guidelines.md)

### More information

* [Babblesim's architecture](architecture.md)
* [How to fetch BabbleSim](fetching.md)
* [BabbleSim folder structure](folder_structure_and_env.md)
* [How to build](building.md) BabbleSim
* [An example with the 2G4 Phy](example_2g4.md)
* [How to debug](debugging.md)
* [Architecture of HW models used for embedded SW testing](arch_hw_models.md)
* [Component naming convention](components_naming.md)
* [Tracing and results/output files](tracing_and_results.md)
* [The 2G4/BLE Phy](2G4.md)
* [Converting BLE activity to pcap files (eg. for Wireshark)](csv2pcap.md)
* [Importing BLE activity into the Ellisys BT analyzer SW](import_Ellisys.md)
* [Selecting channel and modem for the 2G4 Phy](2G4_select_ch_mo.md)
* [Design choices and objectives](objectives.md)
* [Frequently asked questions](faq.md)
* [Infrequently asked questions](ifaq.md)
* [Announcements](Announcements.md)
