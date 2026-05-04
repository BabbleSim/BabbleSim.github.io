## Past announcements

### 2025/11/11 : New v3.0 BabbleSim release

A new release of BabbleSim has just been done (v3.0) and it is being propagated to Zephyr's manifest.
Like always this release remains backwards compatible, so you can safely update to it.
When / if another component requires a given BabbleSim updated version you will get to know when that component is updated.
This release includes preliminary Higher Bands (5/6GHz) support.


Release:

* [West](https://github.com/BabbleSim/bsim_west/commit/dcf997c28b5cc2dc21ae876b64b31ff72b786210)
* [Repo](https://github.com/BabbleSim/manifest/commit/548ed9d0099db3960ff8f52c5b6c3e399c6db8c1)

Updates in Zephyr:

* https://github.com/zephyrproject-rtos/zephyr/pull/99207
* https://github.com/zephyrproject-rtos/docker-image/pull/278

Main changes since v2.7:

* Phy-device v2.1 API introduced
* Phy updated to use internally this v2.1 API, so channel and modem IF are updated accordingly


### native simulator and babblesim talks on youtube

The native simulator talk from the OSS Europe 2025 is now on youtube:
https://www.youtube.com/watch?v=umYKNDufY5E

You can also find on youtube the old Babblesim introduction tech talk https://www.youtube.com/watch?v=D0v3rlla9c8

-------------

### 2025/06/18 : New v2.7 BabbleSim release

A new release of BabbleSim has just been done (v2.7) and it is being propagated to Zephyr's manifest.
Like always this release remains backwards compatible, so you can safely update to it.
When / if another component requires a given BabbleSim updated version you will get to know when that component is updated.

This release includes some runtime performance improvements as well as preliminary HDT support.

Release:

* [West](https://github.com/BabbleSim/bsim_west/commit/2ba22a0608ad9f46da1b96ee5121af357053c791)
* [Repo](https://github.com/BabbleSim/manifest/commit/3d80646f340fee03f485680b943dee967dca32c2)

Updates in Zephyr:

* https://github.com/zephyrproject-rtos/zephyr/pull/91846
* https://github.com/zephyrproject-rtos/docker-image/pull/246

Main changes since v2.6:

* ext_2G4_phy_v1: Runtime performance optimizations
* ext_2G4_libPhyComv1: Add BT LE HDT support
* ext_2G4_channel_Indoorv1: Add BT LE HDT support

-------------

### 2025/05/16 : New minor v2.6 BabbleSim release

A minor release of BabbleSim has just been done (v2.6) and it is being propagated to Zephyr's manifest.
Like always this release remains backwards compatible, so you can safely update to it.
When / if another component requires a given BabbleSim updated version you will get to know when that component is updated.
In this case, the changes provide some general improvements.

Release:

* [West](https://github.com/BabbleSim/bsim_west/commit/193b8ba94cdc6ecbc3bb7fe80b87dee456e5eab0)
* [Repo](https://github.com/BabbleSim/manifest/commit/d68b86757d98572b56b82400bacaa5fafeb95bbb)

Updates in Zephyr:

* https://github.com/zephyrproject-rtos/zephyr/pull/90012
* https://github.com/zephyrproject-rtos/docker-image/pull/231

Main changes since v2.5:

* ext_2G4_phy_v1: Now supports DISCONNECTS during abort re-evaluations,
* ext_2G4_phy_v1: Also support bitrates multiple of 250Kbps and 333Kbps

-------------

### 2025/01/20 : New minor v2.5 BabbleSim release

New minor v2.5 BabbleSim release
A minor release of BabbleSim has just been done (v2.5) and it is being propagated to Zephyr and the NCS manifest.

Like always this release remains backwards compatible, so you can safely update to it. When / if another component requires a given BabbleSim updated version you will get to know when that component is updated.
In this case, the changes enable the nRF54 CRACEN HW models.

Release:

* [West](https://github.com/BabbleSim/bsim_west/commit/a88d3353451387ca490a6a7f7c478a90c4ee05b7)
* [Repo](https://github.com/BabbleSim/manifest/commit/fe42582630d9093854de2208b592ceaa49bd7e1c)

Updates in Zephyr and NCS:

* https://github.com/zephyrproject-rtos/zephyr/pull/84236
* https://github.com/zephyrproject-rtos/docker-image/pull/219
* https://github.com/nrfconnect/sdk-nrf/pull/18264

Main changes since v2.4:

* libRandv2: New API to fill buffers & Wextra warning fixes
* libCryptov1: Add new AES-ECB API

-------------

### 2024/09/02 : New minor v2.4 BabbleSim release

A minor release of BabbleSim has just been done (v2.4) and it is being propagated to Zephyr and the NCS manifest.
Like always this release remains backwards compatible, so you can safely update to it.
When / if another component requires a given BabbleSim updated version you will get to know when that component is updated.
Release:

* [West](https://github.com/BabbleSim/bsim_west/commit/1f242f4ed7fc141fdfcfeca8d21c6d9e801179d7)
* [Repo](https://github.com/BabbleSim/manifest/commit/a816ffd4825c394cd2d4e071bcbee75d830d4f39)

Updates in Zephyr and NCS:

* https://github.com/zephyrproject-rtos/zephyr/pull/80510
* https://github.com/zephyrproject-rtos/docker-image/pull/215
* https://github.com/nrfconnect/sdk-nrf/pull/18264

-------------

### 2024/09/02 : New Babblesim release (v2.3)

There has been a minor release update to Babblesim (v2.3) which includes new APIs in the libCrypto library which are used by the nRF54L HW models.
This release is, as always, backwards compatible with previous releases so you can safely update to it.

Zephyr's manifest points now to this release, and eventually this change will be downmerged to NCS.

If you are getting bsim from Zephyr's manifest, in your Zephyr workspace, in Zephyr's latest main, do:

```
west update
cd ${BSIM_OUT_PATH}
make everything
```

If you are getting bsim using the repo manifest, tracking the main manifest master branch, you can just do:

```
cd ${BSIM_OUT_PATH}
repo sync
make everything
```

If you do not update the bsim installation, but use updated HW models, the HW models (in zephyr.exe) will give you a runtime warning as they fall back to the old libCrypto API.
If you do not use the nRF54L CCM model, this is harmless.

If you use it, you will encounter CCM encryption errors depending on module configuration (specially for 802.15.4).

-------------

### 2024/03/13 : Coded Phy support added to BabbleSim and nRF HW models

Coded Phy is now supported in BabbleSim, the [nRF HW models](https://github.com/babbleSim/ext_NRF_hw_models),
and will soon be also in [Zephyr's simulated targets](https://github.com/zephyrproject-rtos/zephyr/pull/70158)
Note that using this functionality requires updating the simulator itself to the latest version.

Notes:

* For the Phy:
    * Changes in the Phy will result in random draw changes. That is, you may see bit or synchronization errors change between this version of the Phy and a previous version with all being equal.
    * There has been a correction in the bit error calculation. The BER is now capped to 50%
* If you have your own modem performance models, note there has been an API change in the Phy-modem IF, as now the model digital performance gets also the coding rate as parameter.
* v2 channel activity dumps are now enabled (v1 dumps are still produced). v2 have more information than v1 dumps.

-------------

### 2023/09/29 : nRF5340 models added

The [nRF HW models](https://github.com/babbleSim/ext_NRF_hw_models) now include models of the nRF5340, and a
[target board has been added in Zephyr](https://docs.zephyrproject.org/latest/boards/native/nrf_bsim/doc/nrf5340bsim.html)
which can be used to target either the cpuapp or cpunet cores, or build images using both.

-------------

### 2023/03/10 : West manifest added

Over time, some users have requested being able to get the babblesim components with west. Those may be interested to check
[the instruction on how to fetch it](fetching.md)

-------------

### 2023/03/09 : New Phy-dev API for 2.4GHz added to Babblesim

A new "v2" device-phy API for the 2.4GHz babblesim components has been upstreamed.
It provides a superset of the old API functionality aiming at providing
support for more protocols, and features.

The old device-phy IPC is NOT deprecated, and remains ABI compatible.
The updated ext_2G4_libPhyComv1 remains API compatible.
The commitment on future dev<->phy IPC binary compatibility for the old API remains in place.

Users are, at this point, not encouraged to port their devices to use the new API;
As this new APIv2 should be considered in Alpha state,
changes to this API should be expected.

All affected components have been tagged v2.0 and the commit which introduces the new API has been clearly marked.

What you can expect:

* If you are a normal user, you can update your local babblesim repositories to the latest and
  everything should continue working just as before.
* You can keep running your old devices using the new version of the Phy without any problems
  (without even a need to recompile), or you can rebuild them with the updated ext_2G4_libPhyComv1.
  The old APIs remain there unchanged.
* Zephyr's nrf52_bsim will, at some point in the near future, start using updated nrf52 HW models which will rely on the new API.
  When that happens you can expect a new notification; at that point you will need to update your babblesim components.
* Devices using the old API can communicate without any problem with devices using the new API.
* The Phy has been updated to provide a lot of the new functionality the new API supports.
  When using the updated Phy thru the old API you should expect the same results.
* For more advanced users:
    * The modem-phy interface has had a very trivial API change.
      If you have your own proprietary modem models you will need to update them and rebuild them.
      Just check any of the provided modem models for the needed change.
    * The channel modem API has had ABI incompatible changes (one structure has more fields).
      If you have your own proprietary ones you will need to rebuild them with the latest Phy headers.
      You may need to adapt your channel models code (unlikely).

Background:

* The old dev-phy API could not support some of the features and protocols which are being added.
  So an update to the API was required. This update has been done with backwards compatibility as the highest priority.
* The new API provides a superset of the old API functionality, and instructions on how to map the old API to the new one
  can be found in the respective headers. But remember: You do NOT need to use the new API.
* As new functionality is being added both to the Phy and devices using it,
  it is likely that shortcomings in the *new* API may be identified,
  and that therefore it may receive updates. Therefore the "alpha" remark.

-------------