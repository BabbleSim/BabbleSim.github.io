## Fetching BabbleSim

The BabbleSim project consists of many components, most of them optional
and each in its own separate git repository.
The easiest and recommended way to fetch them is by using
[Androids repo](https://source.android.com/setup/build/downloading#installing-repo)

```
mkdir ~/bsim/ && cd ~/bsim/
repo init -u git@github.com:BabbleSim/manifest.git -m everything.xml -b master
repo sync
```

<div class="note-container">
<div class="note-title">Note</div>
<div class="note">If you use BabbleSim with Zephyr, add to your
<span class="monospaced-font">~/.bashrc</span> or
<span class="monospaced-font">~/.zephyrrc</span> file<br>
<span class="monospaced-font">
export BSIM_OUT_PATH="~/bsim/"<br>
export BSIM_COMPONENTS_PATH=${BSIM_OUT_PATH}/components/<br>
#replace those paths as necessary
</span>
</div>
</div>

<div class="note-container">
<div class="note-title">Note</div>
<div class="note">To build BabbleSim, you must have the 32-bit C library
installed in your system (in Ubuntu 18.04 install the
<span class="monospaced-font">gcc-multilib</span> package)</div>
</div>

<div class="note-container">
<div class="note-title">Note</div>
<div class="note"><span class="monospaced-font">ext_2G4_channel_Indoorv1 </span>
requires the FFTW3 library
(in Ubuntu 18.04 install the
<span class="monospaced-font">libfftw3-dev</span> package)</div>
</div>

For a list and description of the provided manifests, see the
[manifest repository documenation](https://github.com/BabbleSim/manifest)

You may fetch each git repository manually or by other means.
In this case, consult the [folder structure page](folder_structure_and_env.md)
for information about how to place them.