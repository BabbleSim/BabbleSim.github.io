### 2.4GHz Interferers models

Two interfers models for this physical layer are included:

* [A burst interferer](https://github.com/BabbleSim/ext_2G4_device_burst_interferer):
  This device  generates bursts of intereference of a given type, power,
  centered at a selected frequency, and from a programmable start to a
  programmable end time. The interference can be selected to be white noise of
  several bandwiths, a CW (carrier wave), BLE or WLAN modulated.

* [A WLAN activity model](https://github.com/BabbleSim/ext_2G4_device_WLAN_actmod):
  Although very simplistic, it generates patterns of channel occupancy similar
  to what a real WLAN would generate while its underlying traffic pattern would
  remain static. More information can be found in
  [its documentation](https://github.com/BabbleSim/ext_2G4_device_WLAN_actmod/blob/master/docs/Description.pdf)
