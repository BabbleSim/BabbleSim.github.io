## Release flow

<center>
<object data="Release_process.svg" type="image/svg+xml">
<p style="text-align:center">BabbleSim release flow drawing</p>
</object>
<p style="text-align:center">BabbleSim release flow</p>
</center>

A release is a tested snapshot of the BabbleSim base and selected components
main branches.
For regression purposes, customers can assume that releases do not change.
The only allowed changes in a release branch are:

* Patches which do NOT break IPC binary compatibility. (A binary compiled with
  a drop of a release must work with another binary compiled with another drop
  of the same release)
* New selected, requested, features, but only before the release is frozen.
  These new features shall not change any used API or functionality.
* After release freeze only necessary bugfixes which are very well understood,
  and well tested to not cause any regression will be allowed.

Once a release is declared EoL no more changes will be done to it.

Each release will be accompanied by a change log. Backwards compatibility
will be documented in the changelog.

The simulator will be kept as backwards compatible as possible in between
releases.
Both for the IPC libraries and general libraries APIs.
