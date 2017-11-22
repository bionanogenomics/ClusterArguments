# ClusterArguments

Running the assembly command-line on a cluster requires a properly configured clusterArgument.xml file (-C parameter).  Users need to configure the clusterArguments_custom.xml for their own cluster.

Basic configuration recommendation is listed below. Very likely, more configuration of the “clusterArgument_custom.xml” file is required to run the assembly command-line successfully on a custom cluster. 

The clusterArguments_custom.xml assumes 2 types of nodes present in 2 SGE (Sun Grid Engine) queues (or parallel environments) and customers must assign all compute nodes to one of these 2 parallel environments: 
a) smplarge: nodes with 256 GB memory (or more). User cluster must have at least 1 such node. 
b) smp: nodes with 32 GB memory (or more). It can include the same nodes as in smplarge. 
c) The names of the parallel environment (“smp” or “smplarge”) can be changed by users.  For example,  the custom cluster may already have suitable configured parallel environments which include all 256 GB+ or 32 GB+ nodes, so changing the names in clusterArgument_custom.xml to the existing ones
on the customer cluster could be done, instead of defining new parallele environments (“smp” and “smplarge”) on the customer cluster. Also the names can include wildcards, eg “smp*” to match all parallel environments’ names that start with smp*.
d) Defining new parallel environments should be done in accordance with the DRMAA cmpatible batch system. For SGE compatible batch systems, this can be done using the “qconf” command. The Linux command “qconf –aq smp” would be used to create a new parallel environment (or queue). The command will pop up an editor, which
allows the “qname” to be set to “smp” and “hostlist” to be set to a list of compute nodes. Similarly the Linux command “qconf –mq smp” can be used to modify an existing SGE queue called “smp”, for example, to add additional nodes by modifying the “hostlist”. 

The clusterArguments_custom.xml assumes that each node can support jobs using 48 threads (56 threads for the 256 G nodes). If the customer nodes support a different number of threads, improved performance can be obtained by adjusting the “SGE slots” values <-pe> in the clusterArguments_custom.xml file, by scaling the values (such as 47, 27 and 12) up or down proportionately.

In addition, all compute nodes added to smp or smplarge need to have their Linux OS configured  with /proc/sys/vm/overcommit_memory set to 1 (unlimited memory overcommit), and vm.max_map_count increased from 64 KB to 256 KB on the 256 GB nodes, otherwise memory allocations for some jobs may fail.

