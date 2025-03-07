## BabbleSim design objectives

When designing BabbleSim, and while mainaining it forward, the following were and will be the main targets:

* An industry tool, for industry needs:
    * Allow for real target code to run in simulations
    * Minimize 3rd party dependencies in common components
    * Permissive license
    * Compatible with multiple Linux distributions[^1].
* Highly modular:
    * Minimize all types of requirements to possible external components/devices
    * No requirements for the programming or scripting language used
    * Allow in tree and out of tree external components
  	* Get and use only the components you need
* Reproducible
    * Two equal runs shall yield the same result
	* Any randomness is controller with random seeds
* Fast
    * Fast execution
	* Fast compile/run iterations
* Allow to easily debug or instrument code
* Easy to automate and integrate in CI/regression flows
* Stable APIs: Do not break 3rd party devices
    * Maintain legacy compatibility in all APIs (In general interfacess are not removed or changed, but new ones are added)
	* When a library change breaks compatibility, a new version is created. The old library version is kept.
	* The Device-Phy interface is kept ABI compatible while possible


[^1]: The target being: the 2 latest RedHats (8 & 9), 2 latest Ubuntu LTS (22.04 & 24.04), and the current Debian stable (12, bookworm).