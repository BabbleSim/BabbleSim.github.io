## Fetching BabbleSim

The BabbleSim project consists of many components, most of them optional
and each in its own separate git repository.

There is several ways to fetch it:

### Using Android's repo

The easiest and recommended way to fetch them is by using
[Androids repo](https://source.android.com/setup/build/downloading#installing-repo)

```
mkdir ~/bsim/ && cd ~/bsim/
repo init -u https://github.com/BabbleSim/manifest.git -m everything.xml -b master
repo sync
```

For a list and description of the provided manifests, see the
[manifest repository documentation](https://github.com/BabbleSim/manifest)

### Using Zephyr's west tool (decoupled from Zephyr itself)

```
mkdir ~/bsim/ && cd ~/bsim/
west init -m git@github.com:BabbleSim/bsim_west.git bsim
cd bsim
west update
```

For a list and description of the provided manifests, see the
[west manifest repository documentation](https://github.com/BabbleSim/bsim_west)

### As a set of separate projects, with git submodule, subtree, or vanilla git

You may fetch each git repository manually or by other means.
In this case, consult the [folder structure page](folder_structure_and_env.md)
for information about how to place them.

-------

<div class="note-container">
<div class="note-title">Note</div>
<div class="note">If you use BabbleSim with Zephyr, add to your
<span class="monospaced-font">~/.bashrc</span> or
<span class="monospaced-font">~/.zephyrrc</span> file<br>
<span class="monospaced-font">
export BSIM_OUT_PATH=${HOME}/bsim/ #If fetched with the repo instructions<br>
export BSIM_OUT_PATH=${HOME}/bsim/bsim/ #If fetched with the west instructions<br>
export BSIM_COMPONENTS_PATH=${BSIM_OUT_PATH}/components/<br>
#replace those paths as necessary
</span>
</div>
</div>

<div class="note-container">
<div class="note-title">Note</div>
<div class="note">To build BabbleSim, you must have the 32-bit C library
installed in your system (in Ubuntu 18.04 and newer install the
<span class="monospaced-font">gcc-multilib</span> package)</div>
</div>

<div class="note-container">
<div class="note-title">Note</div>
<div class="note"><span class="monospaced-font">ext_2G4_channel_Indoorv1 </span>
requires the FFTW3 library
(in Ubuntu 18.04 and newer install the
<span class="monospaced-font">libfftw3-dev</span> package)</div>
</div>
