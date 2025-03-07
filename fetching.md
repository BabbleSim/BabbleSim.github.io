## Fetching BabbleSim

The BabbleSim project consists of many components, most of them optional
and each in its own separate git repository.

There are several ways to fetch it, using android's repo, west or plain git.<br>
You can find instructions on how to install git, repo and west in [dependencies](dependencies.md).

### Using Android's repo

The easiest and recommended way to fetch them is by using
[Androids repo](https://gerrit.googlesource.com/git-repo/)

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

### Using Zephyr's west tool, as part of a Zephyr or nRF Connect SDK workspace

New enough Zephyr and NCS repositories (main branch newer than 2023/04/20)
include BabbleSim in their main manifest, but disabled by default.
You can enable that group by adding to the west workspace group filter babblesim:
```
west config manifest.group-filter -- +babblesim
west update
```
After that you will find the simulator in the `tools/bsim` folder of your workspace.

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
export BSIM_OUT_PATH=${HOME}/bsim/bsim/ #If fetched with the standalone west instructions<br>
export BSIM_OUT_PATH=${ZEPHYR_BASE}/../tools/bsim/ #If fetched as part of the Zephyr or NCS workspace<br>
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
