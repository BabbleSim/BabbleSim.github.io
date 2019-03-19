## Components naming convention

Components are normally named:
```<component_name> = [ext_]<nameOfComponent>[_v<VersionNumber>]```<br>
```[_v<VersionNumber>]``` is optional

For libraries or other similar components:

Changes of ```<VersionNumber>``` assume loss of backwards compatibility<br>
Same ```<VersionNumber>``` assume backwards compatibility

Components executables shall, in general, be named
```bs_<component_name><_anything else>```
where "component_name" shall match the component folder name, and
"anything else" is up to each component designer

Components which are meant only for one Phy type should
be prefixed by that Phy default name.<br>
Devices which are only meant for 1 Phy should be named
```device_<default_phy_id>_<rest_of_component_name>```

#### External components

External components folders names should start with the ```ext_``` prefix.
This prefix should not be used in the generated binaries or libraries though.
This prefix provides an easy recognizable indication to the users.

