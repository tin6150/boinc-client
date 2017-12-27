boinc-client
************


boinccmd, boincmgr in a singularity container, for running things like seti@home, for burn-in test, etc.

Sometime have run enough HPL and don't need an HPL number, want an alternate test of the machine.
Some BOINC project would use GPU, instead of having to compile an nvidia version of HPL :)

~~~~

boinc is really meant to be a resource monitor/manager, 
not trivial to make it run as a batch job, has to fight with it a lot, not worth the headache.
There is a CONDOR-B scheduler where set of boinc clients can be used as a batch cluster using condor-style submit?


