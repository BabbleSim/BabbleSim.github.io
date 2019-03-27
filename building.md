## Building BabbleSim

BabbleSim uses [GNU Make](https://www.gnu.org/software/make/)
to build the provided components.

If you have already [fetched BabbleSim](fetching.md), building a
component and its dependencies is as easy as running
`make <component>` in the top simulator folder.

You can also build all components present in the `components/` folder with:

```
make everything -j 8
```

You can run `make help` for more information.

Note that by default, the top level makefile will create the folders
`bin/` and `lib/` in the simulator top folder, and will install the different
binaries and libraries there.
You can override the folder in which the compile output is stored by setting
`BSIM_OUT_PATH`.
See [folder structure](folder_structure_and_env.md) for more details.

In the `lib/` directory are stored both shared and static libraries, ready for
use or further linking.

Note that, as many external components of BabbleSim need to be compiled as 32bit
bynaries, the basic BabbleSim libraries are built both targetting x86 and x64.

Therefore **you must have the 32-bit C library installed in your system**
(in Ubuntu 16.04 install the gcc-multilib package)

When building a component, other components it depends on will be built first.
During incremental builds make will check for dependencies and rebuild what is
necessary (Although the makefiles themselves are not listed as dependencies).

If a component cannot be built, the top level makefile will continue building
any other requested component which does not depend on it. For example, if the
FFTw3 library is missing, `ext_2G4_channel_Indoorv1` will fail to build, but all
other components will be built properly, as none depends on this channel model.

Note that most BabbleSim components assume they are executed from the `bin/`
directory and will fail by default if placed in other places (as they
won't be able to find the libraries in `lib/`)

### External components

If you want to develop an external component, it is up to you which build system
you want to use. One simple option is to follow the same structure as the other
components stored in this GitHub organization.
Otherwise, all you will normally need, is:

* To prebuild the BabbleSim libraries your component depends on using
  BabbleSim's build
* Include the needed headers directories in your compile,
  (for ex. `-I${BSIM_COMPONENTS_PATH}/libPhyComv1/src/` ),
* And include `${BSIM_OUT_PATH}/lib/library_you_need.a` in your linking
