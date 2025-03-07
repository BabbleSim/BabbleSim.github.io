## Dependecies

Babblesim works in Linux. If you want to run it in Windows, you can either use WSL2 or another virtual machine.

For fetching BabbleSim components you will need `git`, and may want to use either [Android's `repo`](https://gerrit.googlesource.com/git-repo/) or [`west`](https://docs.zephyrproject.org/latest/develop/west/install.html).
In Ubuntu 24.04 you can install git and repo with:

```
sudo apt install git repo
```
You can install west with
```
pip3 install --user -U west
```

BabbleSim uses [GNU Make](https://www.gnu.org/software/make/) and [gcc](https://gcc.gnu.org/) to build the provided components.
You will also need the 32bit C libary. In Ubuntu, you can install these with
```
sudo apt install gcc gcc-multilib make
```

Optionally, you may want to have the libfftw3 library to be able to build the `ext_2G4_channel_Indoorv1` channel model.
```
sudo apt install libfftw3-dev libfftw3-bin
```

For inspecting 2G4 Phy activity you may want to install [Wireshark](https://www.wireshark.org/), or the
[Ellisys Bluetooth Analyzer Software](https://www.ellisys.com/support/download.php)

If you want to convert the 2G4 Phy activity to Ellisys traces you will also need `awk`
```
sudo apt install gawk
```
To convert traces to Wireshark `.pcap` or `.pcapng` you will need python3.