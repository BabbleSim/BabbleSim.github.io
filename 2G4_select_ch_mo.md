## How to select channel and modem (2G4 Phy v1)

By default the 2G4 v1 Phy (ext_2G4_phy_v1 component) will use the 2G4 "N to N cable" channel model
and the "2G4 magical" modem model for all devices.

But it is possible to select other channel models for the simulation,
as well as different modems models (for each device).

#### Selecting a channel

You can select which channel the Phy will use with the
```-channel=<channel_name>``` option. Then, if needed, you can pass
command line arguments to that channel
with ```-argschannel```. For ex.:

```
./bs_2G4_phy_v1 -s=Whatever -D=1 -channel=Indoorv1 -argschannel -preset=Small2 -dist=distances_file.txt
```

Note that you can pass the ```-help``` option as an argument to
both channel and modem libraries to get help on how to use a
particular model or library.
For ex.:

```
./bs_2G4_phy_v1 -s=Whatever -D=1 -channel=Indoorv1 -argschannel -help
```

#### Selecting modems

The Phy will instantiate a different modem for each device. Each of these
devices will have its own configuration (command line arguments). You can
specify which modem will be used for each device, together with the command
line arguments which will apply to that particular device modem. To simplify
things, you can also select the default modem (and the default modem command
line arguments) which will be given to all devices for which you do not
specify a modem in particular. So for example:

```
./bs_2G4_phy_v1 -v=9 -s=Whatever -D=10 -defmodem=Magic -modem5=BLE_simple -argsdefmodem -BER=1e-6
```

Will set devices 0..4 and 6..9 to use the Magical modem, with a bit
error rate of 1e-6. And will set the modem for device 5 as the BLE_simple modem