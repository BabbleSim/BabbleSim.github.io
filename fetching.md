## Fetching BabbleSim

The BabbleSim project consists of many components, most of them optional
and each in its own separate git repository.
The easiest and recommended way to fetch them is by using Androids repo

```
mkdir ~/bsim/ && cd ~/bsim/
repo init -u git@github.com:BabbleSim/manifest.git -m everything.xml -b master
repo sync
```

For a list and description of the provided manifests, please see the
[manifest repository documenation](https://github.com/BabbleSim/manifest)

You may fetch each git repository manually or by other means.
In this case, consult the [folder structure page](folder_structure_and_env.md)
for information about how to place them.