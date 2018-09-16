## Fetching BabbleSim

The BabbleSim project consists of many components, most of them optional
and each in its own separate git repository.
The recommended way to fetch them is by using Androids repo

```
mkdir ~/bsim/ && cd ~/bsim/
repo init -u git@github.com:BabbleSim/manifest.git -m everything.xml -b master
repo sync
```

For a list and description of the provided manifests, please see the
[manifest repository documenation](https://github.com/BabbleSim/manifest)