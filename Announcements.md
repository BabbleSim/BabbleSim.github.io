## Past announcements

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