boinc-client
************


boinccmd, boincmgr in a singularity container, for running things like seti@home, for burn-in test, etc.

Sometime have run enough HPL and don't need an HPL number, want an alternate test of the machine.
Some BOINC project would use GPU, instead of having to compile an nvidia version of HPL :)

~~~~

boinc is really meant to be a resource monitor/manager, 
not trivial to make it run as a batch job, has to fight with it a lot
There is a CONDOR-B scheduler where set of boinc clients can be used as a batch cluster using condor-style submit?
This container just provides the binary to run boinc, 
still need to run the process as daemon in the background.
Thus, usable in a workstation.
Not so easy to use in a batch scheduler for automated burn-in test :(

