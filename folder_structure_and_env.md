## Folder structure

The default folder structure, which you will have if you just follow the default
instructions, is as follows:
```
.
├── components
│   ├── common           Set of common scripts, documentation, etc
│   ├── component_x      A BabbleSim component
│   │   ├── Depends
│   │   ├── docs
│   │   ├── Makefile
│   │   └── src
│   └── ext_component_y  An external BabbleSim component (hosted in its own repo)
│       ├── Depends
│       ├── docs
│       ├── Makefile
│       └── src
├── Makefile -> components/common/Makefile
├── bin                  Where executables are installed ready to use
├── lib                  Where libraries are stored ready to be loaded/linked to
└── results              Where simulations output is stored
```

By default the intermediate results of compiling a component will be placed
inside that same component folder. And the generated binaries and libraries
will be stored inside the simulator bin/ and lib/ folders.

But it is possible to place components and/or the output in different folders.
For this, a set of environment variables are defined.<br>
These wil be set automatically assuming the default folder structure when using the provided
Makefiles. But they can be set to something else before calling make.<br>



| Variable | Definition |
| :---     | :---       |
| `BSIM_COMPONENTS_PATH` | Path to the components folder.<br> By default the top level makefile assumes it is in the<br>same directory as the folder `components/`<br>The individual component makefiles, assume they are<br>placed inside `components/<component>/` |
| `BSIM_BASE_PATH` | Path to the folder where the base repo was cloned.<br>By default the same as `BSIM_COMPONENTS_PATH` |
| `BSIM_OUT_PATH` | Where the compilation results are installed. That is,<br> where `lib/`, `bin/` and `results/` will be created.|
| `COMPONENT_OUTPUT_DIR` | Where the intermediate results of compiling a particular<br> component will be placed<br>By default the folder of the component being compiled |
| For the 2G4 external components: ||
| `2G4_libPhyComv1_COMP_PATH`|Path where ext_2G4_libPhyComv1 was cloned.<br>By default assumed to be <br>`${BSIM_COMPONENTS_PATH}/ext_2G4_libPhyComv1` |
| `2G4_phy_v1_COMP_PATH` | Path where ext_2G4_phy_v1 was cloned.<br> By default assumed to be `${BSIM_COMPONENTS_PATH}/ext_2G4_phy_v1`|