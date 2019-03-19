## About tracing

All components provided with BabbleSim follow the same convention regarding
traces written to stdout and stderr.
They do so by utilicing `libUtilv1` tracing facilities.
It is recommended that external components follow this same convention for
consistency.

To allow for easy filtering, and identification, each trace is prefixed with:

* `d_<%2i>`: for devices, where `%2i` is the global device number in the
  simulation with at least 2 digits
* `p_<phy_id>`: for the Phy

Subcomponents of devices or the Phys may append their own prefix afterwards
Libraries should use the tracing of the program that uses them

## File generation

If a component generates files during its execution, this should be stored in
the `results/` folder, and the file names should be prefixed with `d_<%i>.`
where `%i` is the global device number.

`libUtilv1/bs_results.h` provides functions to create files following this
convention.
