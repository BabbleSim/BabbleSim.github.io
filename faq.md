## Frequently asked questions

### Does the 2G4 Phy simulation support LE Coded Phy

No. Today only 1 and 2Mbps BLE modulations are supported, as well as 2, 3 & 4Mbps propietary modulations.

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

---------------------------

More can be found in [the infrequently asked questions](ifaq.md) page