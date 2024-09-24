## Architecture of HW models used for embedded SW development and testing

This general description covers the overall design choices, architecture and design patterns of these models. It applies to the [nrf5x HW models](https://github.com/BabbleSim/ext_NRF_hw_models), but it is not limited to them.

Particular details for each model belong with the model source code. For specifics for any of these, please refer to the corresponding HW models source/documentation. If you are interested in the nRF HW models, you are recommended to disregard this page up to the "Time and time drift" section and read instead the [nRF HW models description](https://github.com/BabbleSim/ext_nRF_hw_models/blob/main/docs/README_HW_models.md)

These HW models are meant to be used with either a [Zephyr "board"](https://docs.zephyrproject.org/latest/boards/native/doc/bsim_boards_design.html) or another babblesim device which integrate them.

Note that just like with many other BabbleSim based components, there is no hard requirement for a HW model to follow the design described here, to be able to use other BabbleSim components. Some other HW models exist which do not follow this description. How a device HW model is designed depends on the use and needs for that HW model.

### Overall requirements
The purpose of these models is to provide a good enough simulation of the HW to be able to run the corresponding embedded SW, to be able to debug that SW, and run long simulations fast.
We are not interested in modelling the particularities of the HW where they are not relevant for the SW execution. Therefore many details can be simplified or omitted all together. Specially regarding HW functionality which is not used and for which there is no plans for using it.

Some HW blocks may be provisionally or permanently omitted all together, or modelled in a way that requires a significantly different version of the FW that uses it (for ex. a different driver) or full stubbing of some FW APIs. What HW is modelled and which not depends on the overall target of the simulation platform, cost/benefit and priorities, and may change over time as new, more precise HW models are added.

Regarding time accuracy, these models focus mostly on the radios (as it is relevant for the protocol stack and platform synchronization), and the HW timers. Other peripherals can be overall more rough. Overall these models have a time granularity of 1us, and do not intend to be clock accurate. They should, in general, attempt to maintain the real order of interrupts.

When a SW action in real HW may cause an interrupt (or other sideeffects) to occur very close in time to the moment in which the operation was performed (for ex. unmasking a pending interrupt), the HW models do not, in general intend to make those sideeffects visible to the executing SW in the right moment (for ex. in the next or after a few instructions). If this is required by the executing SW (because for example the following instructions check the HW status), the sideeffect may be modelled as fully instantaneous even though it may have taken a few clock cycles in the real HW, or in others case the HW models may provide the sideeffect only after the CPU is put to sleep (or after the equivalent of Zephyr OS' `k_busy_wait` is called); how it is solved will be decided in a case by case basis.

Overall the HW models are designed focusing on execution speed and overall simplicity.

### Design specification
Overall these models are "event driven". There are several ways of building these kind of models, but for simplicity, speed, and to not need to rely on any kind of library or 3rd party engine, these models are built with a very nimble engine. For some models this is provided partly by the models themselves, and partly by the program that uses them (the "time machine" of either the _bsim board, the HW models testbench, or equivalent). And in some other models this is provided by the native simulator HW scheduler.
The reason for this division is due to the program that integrates them being the overall scheduler (see the [native simulator design](https://github.com/BabbleSim/native_simulator/blob/main/docs/Design.md) ), which initializes both models and "CPU", and is in charge of when each should run.

#### Threading
Most of the HW models and their scheduling are designed to run in one thread dedicated to the HW models. But a relatively thin interface for SW to interact with the HW models is designed to run in other threads.

In general the HW models code is not and does not need to be written to be thread safe. This means a call to one of the HW models functions cannot be done if another thread of the same process is currently also calling another of these functions. Meaning, only one function may be called at a time. This is not a problem if only one thread calls into the HW models. It won't be a problem either if by any other synchronization mechanism it is ensured only one thread calls into these HW models at a time.
This is the case in Zephyr as, in the way this HW models are integrated, the HW thread and the SW threads cannot execute in parallel; they are instead step locked: the HW models thread executes only when the "simulated CPU" (the SW threads) is sleeping.

The slim layer of code provided to SW to interact with the models (see [SW registers IF](#SW-registers-IF)) can be called directly from the SW threads context and must therefore be written with care to ensure it does not perform any operation it shouldn't (mainly it should not call into other HW models APIs which do not expect to be called from SW threads or raise immediate interrupts intended only to be raised from HW context).

The HW models code should never call out into SW code directly. The HW models interact with the SW threads by updating their modelled status registers and raising interrupts.

Note that both the HW models testbenches and the integrating boards include a test system, [bs_tests](https://docs.zephyrproject.org/latest/boards/native/doc/bsim_boards_design.html#bs-tests), which provides a special test task which executes directly in the HW models thread.

#### Event scheduling
In reality any action performed by a HW peripheral will take some amount of time.
For our purposes any HW process which takes too short for the SW to realize, will be modelled as being instantaneous in simulation time. Such processes will just be implemented as a C function (or a set of them), which will change the models status as needed.

Other processes do take a considerable amount of time, like for example sending a radio packet.
Such processes will be modelled in a bit more complex way:

The process model may use one or several "events"/timers.
When needed these timers will be set at a point in the future where some action needs to be performed.
Whenever that time is reached, an event scheduler will call a function in that model tasked with continuing executing that task/process. That function reschedules the event timer to the next moment in which it wants to be called back.
In this model, all of these events|timers types and their callbacks are known in design/compile time. Meaning, there is no dynamic registration of events types.
Normally each peripheral model will have 1 of such event timers, and it will be up to the peripheral model to schedule several subevents using only that one timer and callback if needed.

In the earlist versions of these types of HW models, the top level, `<something>_model_top`, collects all these events and exposes only 1 timer (with 1 callback) to the overall scheduler provided by the program that integrates it.
For never versions, the events are registered at build time from each HW model component by utilizing a dedicated macro.

Whenever a HW model updates its timer, it will call a function in the top level to update that top level timer if needed.

The overall event scheduler provided by the integrating program, will advance simulated time when needed, and call into the top level HW models "event/task runner" (`<something>_some_timer_reached()`) whenever its timer time is reached. This will in turn call whichever HW submodule task function was scheduled for this moment.

Note that several HW submodules may be scheduled to run in the same µs. In this case, they will be handled in different "delta cycles" in that same µs. That is, in different consecutive calls to `*_some_timer_reached()`. Each timer|event has a given priority, and therefore will always be called in the same order relative to other HW events which may be schedule in the same µs.
Note also, that any HW submodule may schedule a new event to be called in the same µs in which it is running. This can be done for any purpose, like for example to defer a side effect of writing to a register from a SW thread into the HW models thread.

Performance considerations: There is a runtime performance penalty for a HW model to schedule an event. So in general the models should be designed to avoid scheduling events unnecessarily or very close in time (for ex. a peripheral which updates a counter 10µs should not simply schedule events every simulated 10µs, and should instead consider if the updates can be bundled or deferred until the register is read).
There is a minor penalty to "registering" (adding) more events in the top level time machine/HW scheduler, as the time to evaluate which even is the next is linear with the number of events. In that way peripherals should try to limit how many events they register. The overall assumption should be that the top level loop executes very often.

#### SW registers IF
Each peripheral model which has HW registers accessible by SW, presents a structure which matches those registers' layout. This structure will be allocated somewhere in the process memory, but certainly not in the same address as in the real HW. Therefore any access to the real registers must be, in some way, corrected to access this structure instead. This may be achieved by providing a version of the macros which, in target point to the peripherals base addresses, which in simulation point to these HW model provided structures, or by modifying the HAL to fetch the pointer to that peripheral instead.

Writing to this structure in itself will only cause that memory location to be changed. For many registers this is perfectly fine, as that is just the same that happens in the real HW (a register is changed, which later may be used by the actual HW).
But for some registers, accessing them (writing and/or reading them) causes some side-effect, that is, something else to happen in the HW. For example, in real HW, writing a 1 to a "start" register may start some process.

For these type of registers with side-effects, the HW models must be triggered, this is achieved by calling a register specific side-effecting function in the HW models after the write/read itself. Calling this side-effecting functions will typically be done from the corresponding HAL function, inside a ifdef for the simulation target, next to the access to the register itself.

#### HW interrupts
For HW models to raise an interrupt all they need to do is call into the interrupt controller model function `hw_irq_ctrl_set_irq(<irq_nbr>)`.

The interrupt controller will update its status, and if the interrupt was not masked, one delta cycle later, awake the CPU by calling `posix_interrupt_raised()` (which is provided by the integrating application). This will in turn cause the HW thread to be paused, the simulated "CPU" to be awaken and the SW to start executing.

APIs are also provided to raise interrupts immediately (instead of 1 delta cycle later), but this should be used with extra care, as a) only the correct variant for HW or SW threads may be used, and b) infinite loops or undesired call loops may be created, and c) another SW thread may call into the models before the previous call returns.

#### Time and time drift
In general all HW models use the same overall "device" time reference. This time reference is provided by the integrating program, and represents time as known by that device components. It normally starts at 0 when the models are initialized, and moves monotonically ahead, with a 1 microsecond resolution.

Inside the same microsecond several events may be scheduled. These are said to be executed in different "deltas" in the same simulated time.

Apart from this device time, there is a related but separate time: "Phy time" (sometimes misnamed "real time"). This time represents the time scale as known by the Physical layer simulation, and is used when communicating with it.
The device time and the Phy time are the same only in very simple devices models. More complex models will allow an offset between these from the start (to emulate a device starting later than others), and a drift in these times. This drift emulates the fact that the time for a device is provided by an oscillator (typical a crystal based one), which will be off compared to reality (may be slightly faster or slower than its specified frequency). A very simple crystal oscillator model may only include a constant offset/drift in the time frequency (like the [trivial_xo](https://github.com/BabbleSim/ext_NRF_hw_models/blob/master/src/HW_models/trivial_xo.c)) or may include more effects like variation in this drift over time, allow on the fly retrimming of the oscillator frequency, and account for the oscillator settling time and its stability.

#### Command line arguments for models
Quite a few of these HW models take command line options. The way to describe the command line arguments follows Babblesim's libUtilv1 command line parsing convention. Please check that library API and examples from the nrf5x HW models for more info.

In older HW models these command line options were exposed to the integrating program as a list of command line options (contained in a macro), which this integrating program is meant to include in its own command line options list.
This list of options includes the necessary definitions and callbacks which will set defaults, handle the user choices and provide the help messages.
For newer HW models, these command line options are directly registered at runtime by the HW models.

What command line options a model may have is fully dependent on the model. For ex. an analog device model may include parameters to select characteristics based on some production variability, or aging ; A model of a of a NVRAM may accept a path to a file that represents the content of the NVRAM ; A specially complex model may have options to select running in a faster but more imprecise mode; etc.

#### Special HW models
In each of these device HW models there is two special ones which do not represent real HW and have a special role:

* bstest_ticker: It provides a HW timer dedicated for the [bs_tests](https://docs.zephyrproject.org/latest/boards/native/doc/bsim_boards_design.html#bs-tests) mechanism. This timer can/is used by bs_tests based tests to either schedule execution of a test function at some given points in time and/or at periodic intervals.
* fake_timer: it provides a HW timer used to enable SW doing busy waits (through Zephyr's k_busy_wait() API or equivalent). See [Zephyr's POSIX architecture Busywaits](https://docs.zephyrproject.org/latest/boards/native/doc/arch_soc.html#busy-waits) for more info.

#### Use of BabbleSim (which of its components are used for what)
Radio hardware models use the corresponding [BabbleSim physical layer simulation](https://babblesim.github.io/) to simulate it. The physical layer simulates the shared medium between devices, includes propagation and interference effects, and coordinates the simulated time with other simulated devices.

Typically the HW models also use:

* base/libUtilv1: Utility functions including command line argument parsing, logging, etc.
* base/libRandv2: Random number library.
* *libPhyComv1: Used to communicate with the physical layer simulation

#### Interfacing towards the Physical layer simulation
HW models of a modem that rely on the BabbleSim Physical layer simulation utilize the corresponding libPhyCom library to interact with it. For 2.4HGz/BLE modems this will be the [2G4 variant](https://github.com/BabbleSim/ext_2G4_libPhyComv1). In its documentation and headers much more information can be found.

These libPhyCom variants all extend the [base libPhyCom library](https://github.com/BabbleSim/base/tree/master/libPhyComv1), which provides the basic and common API to connect to any Phy simulation.

##### HWLowL
The HWLowL component provides a thin layer between the device and the Phy, which enables two things:

1. To enable running a simulated device in isolation, that is without a Phy.
2. Periodically resynchronize the device time with the Phy simulation time even if no radio activity is ocurring.

The first enables running the device without bothering to start a Physical layer simulation process. This is helpful when one intends to test functionality that does not require the use of the radio, like for example all Zephyr twister based tests. It should be noted that when this option is used the simulation will fail if the radio attempts to interact with the Phy, that is, if the radio attempts to send or recive a packet.

The second is used to ensure that a device which is not transmitting or receiving for a long period of time, does not execute for a very long time decoupled from other devices in the simulation. In general this is not a problem, as the devices time will be aligned again to the Phy once there is radio activity, but may be a problem if the devices test code exchange information with other devices in some other way like thru the [backchannels](https://github.com/BabbleSim/base/blob/master/libPhyComv1/src/bs_pc_backchannel.c).

#### Tracing
HW models use the [bs_trace API](https://github.com/BabbleSim/base/blob/master/libUtilv1/src/bs_tracing.h) to print trace/log debug, warning and error messages.

#### Random number generation
HW models should only use a controlled pseudorandom source for random numbers to ensure simulations are reproducible. This is done using the [libRandv2](https://github.com/BabbleSim/base/tree/master/libRandv2/src) API. The seed for this pseudorandom generation is controller by the integrating program thru a command line parameter, and will default to a value based on the "device number" in the Phy.

#### Logging signals/activity, aka dumping
Some of these HW models utilize a mechanism to dump to disk files different types of relevant activity. Each model can dump to some dedicated file activity its designer considers relevant. The overall control is provided by the integrating program, where the user can select through command line parameters which of this dump files are actually produced. By default these files are not dumped to speed up the simulation and save disk space.

Typical activity may be information which is useful to debug or understand the state of the HW model or its use, or that may be useful to characterize or tune some parameter or behavior of the HW or SW, for example through Montecarlo simulations.

#### Testing of these HW models
Some of these models sets do not include any dedicated test and rely directly on the integrating projects to test them, for example as a collateral effect of testing the FW that uses them.

Some other models are tested through a dedicated program/test bench included in the same BabbleSim component as the HW models. This dedicated test program follows what is described in [How are these models integrated](#How-are-these-models-integrated), and relies on a HW only version of the [bs_tests](https://docs.zephyrproject.org/latest/boards/native/doc/bsim_boards_design.html#bs-tests) component. This wrapping test bench can be understood as a very cut down/minimal version of the [Zephyr _bsim devices](https://docs.zephyrproject.org/latest/boards/native/doc/bsim_boards_design.html) without the CPU simulation or FW under test.

#### How are these models integrated
For information on how to integrate these models in Zephyr based devices please check [Zephyr's *_bsim devices documentation](https://docs.zephyrproject.org/latest/boards/native/doc/bsim_boards_design.html#design).

##### Models interface towards a device event scheduler
As described before, overall the models are "event driven": They rely on an overall scheduler triggering (calling) them in the appropriate times.

This overall scheduler is provided by the program that integrates them (For new models, this is the native simulator. For older models this is the `<something>_bsim` Zephyr board "time machine", or HW test bench "time machine"; the interface the models expect from this is described in `time_machine_if.h`).

For older models:
The top level `<something>_model_top.c` exposes one event timer: `<something>_hw_next_timer_to_trigger`. This timer represents, in microseconds, when the models need to perform the next action.
It is the responsibility of that overall scheduler to ensure the HW models top level scheduling function (`<something>_hw_some_timer_reached()`) is called whenever that time is reached. (Note that HW models expect to be called exactly at that time, not earlier or later.)
This timer may be changed after each execution of the models, or whenever a HW register is written. When the models change this timer, they will call `tm_find_next_timer_to_trigger()` to notify that overall scheduler of the change.
The models are initialized by calling first `<something>_hw_pre_init()`. Later `<something>_hw_initialize()` shall be called with the selected command line options.
No other HW model function can be called before these two.
`<something>_free_all()` shall be called to clean up after the simulation is done: to deallocate and close any resource the models may be using.

For newer models based on the native simulator, the HW events are registered in the hw scheduler, and coded which needs to be executed during program initialization or exist, is similarly registered using the native simulator APIs.

##### Models libraries
In general, when these models are provided as a separate BabbleSim component, they include at least a Makefile which can be used with the overall BabbleSim build system.

With this makefile at least a library including this HW models will be built. This library can then be included in an integrating program which would also include the SW under test, test bench facilities, other possible HW models, and any overall wrapping code (see [Zephyr *_bsim devices](https://docs.zephyrproject.org/latest/boards/native/doc/bsim_boards_design.html#design) for information on Zephyr based devices).

This make file will also provide the means to build the dedicated HW test application if it exists (see [Testing of these HW models](#Testing-of-these-HW-models) ).

### Two typical HW models
#### Non-timed HW model
Some HW models model HW that does not take a relevant amount of time to perform its actions.

There can be different types of HW like this:

* It may be a HW model that interfaces only other HW. Typically this will be implemented as a component which provides a set of functions callable from other HW models and which perform their actions directly.
* It may be a HW model that also has a SW interface. In this case the HW model will instantiate a SW-interface-registers structure that matches the real peripheral interface (see [SW registers IF](#SW-registers-IF)).
If this is limited to exposing status the model may simply update its status registers in its SW-interface-registers structure. But it may also have registers with side-effects that SW accesses; in this later case the models may perform all actions required by the side-effects directly in the call from the side-effecting function (Care should be taken here as that function will execute in a SW thread context).
* The model may need to raise interrupts. In that case, the model should use `hw_irq_ctrl_set_irq()` to pend the interrupt (which will cause the interrupt to be handled 1 delta cycle later in the SW execution context) (see [Zephyr's posix arch soc and board design introduction](https://docs.zephyrproject.org/latest/boards/native/doc/arch_soc.html#soc-and-board-layers))

#### Timed HW model
Timed HW models, apart from implementing what [non-timed HW models](#Non-timed-HW-model) do, will also expose at least one event timer to the top level time/event scheduler, and have a registered function which will be called when the event time is reached. This type of models will normally need to keep some internal status apart from the SW-interface-registers structure. How the model is implemented depends greatly on what it does.

And example of a simple time model would be the [nRF52 RNG](https://github.com/BabbleSim/ext_NRF_hw_models/blob/master/src/HW_models/NHW_RNG.c) (random number generator), in which Timer_RNG is the event timer, `nrf_rng_timer_triggered()` is the functioned to be called once the event time is reached, `nrf_rng_regw_sideeffects_{TASK_START, TASK_STOP, INTENET, INTENCLR}` are the functions handling the sideeffects for the corresponding register write, and `nrg_rng_init()` is the initialization function.