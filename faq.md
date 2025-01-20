## Frequently asked questions

### Does the 2G4 Phy simulation support LE Coded Phy?

Yes

### Can I run BabbleSim in Windows?

Not directly. But:

* You should be able to use it in WSL2
* With some work, you can get it working in WSL1 (see below)
* You can run it in a Linux VM

### How can I run BabbleSim in WSL1?

The following has been reported by a user as working:
(thanks Andrei H)

First you need to add suppot for 32bit binaies in WSL1, check the following link:
[https://github.com/microsoft/WSL/issues/2468#issuecomment-374904520](https://github.com/microsoft/WSL/issues/2468#issuecomment-374904520)

You will also need to hack the ext_libCryptov1 library Makefiles as so:

```diff
diff --git a/Makefile b/Makefile
index 73f2b43..2af3dbe 100755
--- a/Makefile
+++ b/Makefile
@@ -12,3 +12,3 @@ DEBUG:=-g
 OPT:=
-ARCH:=-m32
+ARCH:=
 WARNINGS:=-Wall -pedantic

diff --git a/Makefile.library b/Makefile.library
index d78b61c..562225d 100755
--- a/Makefile.library
+++ b/Makefile.library
@@ -26,3 +26,3 @@ ${SOURCE_LIB_FOLDER}/${LIB_CRYPTO}: ${SOURCE_LIB_FOLDER}
         @echo "This is silent on purpose... (if you have some problem compiling it, run these by hand:"
-        cd ${SOURCE_LIB_FOLDER} && setarch i386 ./config -m32 -g -fPIC no-idea no-camellia no-seed no-bf no-cast no-rc2 no-rc4 no-rc5 \
+        cd ${SOURCE_LIB_FOLDER} && CFLAGS='-m32' CXXFLAGS='-m32' LDFLAGS='-m32' ./config -m32 -L/usr/lib32 -g -fPIC no-idea no-camellia no-seed no-bf no-cast no-rc2 no-rc4 no-rc5 \
  no-md2 no-md4 no-ripemd no-mdc2 no-dsa no-dh no-ec no-ecdsa no-ecdh no-sock no-ssl2 no-ssl3 no-err no-krb5 no-engine no-hw >& /dev/null \
```
Note: The ext_libCryptov1 also has a tinycrypt based version of the library (in the tinycrypt branch) which should not require any hack


### `repo` crashes/started crashing (and my default python is python2)

Unfortunately repo was changed to require python 3, and to assume `python` is the `python3` interpreter.
This is not helped by the fact that `repo` automatically fetches its latest version when fetching a new project.

If your system `python` resolves to `python2` trying to run the latest `repo` ummodified will crash.
In this case, if your system has python3 installed, a solution is to modify the first line of the repo script:
```diff
-#!/usr/bin/env python
+#!/usr/bin/env python3
 # -*- coding:utf-8 -*-
 #
```

If you don't have python3, you can download an older version of repo which works with `python2`:
[https://source.android.com/setup/develop#old-repo-python2](https://source.android.com/setup/develop#old-repo-python2)

### Can I run BabbleSim with the nRF HW models on an ARM64 host

Short answer: Not today.

Long answer: Some ARM64 targets (like Mac HW) do not support executing 32bit ARM instructions. Some others do.

Even for those which do, the userspace has significant differences. The current way of building the simulator libraries, HW models and embedded SW
assume an x86-64 host.

The BabbleSim libraries are built today both as 64 and 32bit binary versions to be usable from 64 and 32bit binaries.

The embedded SW for most embedded targets, and the nordic HW models (just like the real HW) assume that the addressing space is 32bits, that is that sizeof(void*) == sizeof(int) == 4. In a 64bit target this is not the case, and therefore code meant for a 32bit embedded system will either fail to build or crash if run as a 64bit binary.


### Can I fetch BabbleSim as a set of git submodules instead of using repo?

Expanded question: *Instead of fetching BabbleSim with `repo`, I would like to fetch it as a set of git submodules for my git project.
Unfortunately, the default folder structure places external components overlapping the base repo folder, and git submodules does not
support submodules placed inside other submodules.*

You can fetch all BabbleSim components you need as git submodules of another project. But you need to place things a bit differently than by default.
The default is to place the base component diretly at the root of the `components/` folder. Instead you can place it in another folder and set the environmnet
variable `BSIM_BASE_PATH` with that folder path. For extra confort (to save extra `make` calls), link all base components into the `components/` folder.
You should also add a link to the main Makefile, and check all these links as part of the top git repository which includes BabbleSim as submodules.


```
bsim/
├── base
│   ├── common
│   ├── device_empty
│   ├── device_handbrake
│   ├── device_pause_simu
│   ├── device_time_monitor
│   ├── libPhyComv1
│   ├── libRandv2
│   └── libUtilv1
├── components
│   ├── device_empty -> ../base/device_empty/
│   ├── device_handbrake -> ../base/device_handbrake/
│   ├── device_pause_simu -> ../base/device_pause_simu/
│   ├── device_time_monitor -> ../base/device_time_monitor/
│   ├── ext_component1                        #directly fetched as a submodule in bsim/components/
│   ├── ext_component2                        #directly fetched as a submodule in bsim/components/
│   ├── libPhyComv1 -> ../base/libPhyComv1/
│   ├── libRandv2 -> ../base/libRandv2/
│   └── libUtilv1 -> ../base/libUtilv1/
└── Makefile -> base/common/Makefile
```

See [folder structure and environment variables](folder_structure_and_env.md) for inspiration for other options.

---------------------------

More can be found in [the infrequently asked questions](ifaq.md) page