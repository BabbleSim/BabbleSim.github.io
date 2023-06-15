## Release flow

### Release flow since 2018.

BabbleSim's components master branches are tested,
and kept so that all non-deprecated components master branches tips can be used with each other.

A release is a snapshot of the BabbleSim base and selected components
master branches which work correctly with each other.

For all purposes, customers can assume that releases do not change.


#### Release flow procedure (internal)

* Ensure all local component repos and authoritative remotes master branches are aligned. (`git fetch upstream master`)
* For all components that require a new tag
    * Update the version file to the upcoming tag, and commit it
    * Tag the repository, and push the tag to the authoritative repo
* Chose a new overall release version (vX.Y.Z):
    * If only minor bugfixes or minor new features or improvements are included, just a patch version increase (Z++)
    * If more significant bugfixes, features or improvements (mostly if they could be required by a customer), a minor version increase (Y++.0)
    * If very significant changes, specially when compatibility with previous released components is broken, a mayor version increase (X++.0)
* For both repo and west manifests repos, on the master/main branch:
    * For all manifests that track particular versions
        * Go thru all its components and ensure they are pointing to their latest tag
        * Commit, and push to test manifest-repo master
        * repo/west init from test repo, and sync/update, to ensure no sha/tag is malformed.
        * Push to manifest-repo master
        * Create a new vX.Y.Z branch on that exact same commit
        * Push that vX.Y.Z branch to manifest-repo
