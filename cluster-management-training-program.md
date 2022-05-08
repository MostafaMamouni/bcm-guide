# Introduction to system administration of HPC clusters

Out aim in this training is to explain an HPC cluster as a concept and use Bright Cluster Manager
for a demonstration since it is the fastest way to build an HPC cluster and start practicing
the administrating of the cluster, then if you don't will to work with Bright Cluster Manager
you can migrate to open source tools, but it is important to understand very well all the aspects
of managing an HPC cluster.

>>Note: we will use virtual machines for all the tasks, don't worry!, since we are dealing
just with the software layer, you won't have any difficulties to build a physical
HPC cluster. you need only to set up it as described in your configuration files at the head
node, then you need just power on the nodes and let the headnode build the cluster.
In this training we won't discuss how to set up a physical cluster but we will mention if there is something that differs from the a virtual cluster

To manage an HPC cluster, it is necessary to understand the following parts:

## Deployment (2 days)

**prerequisites**: a good understanding of linux systems as well as the networking.

The HPC clusters can consist of tens all the way to thousands of nodes that are bare metal (no
operating system is installed) at their initial stat, therefore, installing the operating systems
on each node manually is not practical whether on short term or on the long term, Luckily we have [pxe](https://en.wikipedia.org/wiki/Preboot_Execution_Environment) that automates the process of installing OSs
on the bare metal nodes through the network, but it also needs a handful of configurations and dependencies to install.
To make the deployment even faster the community has developed many tools like [cobbler](https://github.com/cobbler/cobbler) and [warewulf](https://github.com/hpcng/warewulf)
that automate the booting of the nodes through the network using pxe. For this training and for the simplicity
we will use Bright Cluster Manager, please refer to [this](https://github.com/MostafaMamouni/bcm-guide/blob/main/bcm%20installatoin%20quick%20guide.md#preparing-for-the-installation) to see how to set up a VM node to pxe boot, for physical nodes it differs based on the vendor of the server, but usually you need to press f12 before the node start booting from the drive.

**tasks**

build a virtual HPC cluster that consist of a headnode, a login node and 3 compute nodes.
check [this](https://github.com/MostafaMamouni/bcm-guide/blob/main/bcm%20installatoin%20quick%20guide.md) for a quick start. don't forget to request the license of easy8, otherwise you won't be able to use `cmsh`

- At the headnode.
- networking: two network interfaces should be configured, one is connected to the
internet the other one is connected to the private network of the cluster
- users and groups: ldap and sssd (will be discussed later) should be automatically
configured by CMDaemon (Cluster Management Daemon).
- resources management: set up slurm following **7.3 Installation Of Workload Managers** section in [BCM documentation](https://support.brightcomputing.com/manuals/9.1/admin-manual.pdf). the headnode should have *slurm-server* and *slurm-submit* roles, the
login node should have only *slurm-submit* role, the rest of the compute nodes should
have only *slurm-client*.
- Add a login node that has two network interfaces one is connected to the
internet the other one is connected to the private network of the cluster
- Add 3 compute nodes that have only one network interface which is connected to the
private network of the cluster.

advanced usage/features should be about adding an InfinitBand network and configure [bmc](https://www.techtarget.com/searchnetworking/definition/baseboard-management-controller)
custumizing system images to deploy, and how to set up other services like clouding and 
kubernetes and other tools.

## Users and groups management (1 day)

**prerequisites**: a good understanding of linux systems as well as the networking.

In order for a user (not root) to submit a job by using slurm for example, the user itself
should be recognized among all the compute nodes, therefore we need to add the user on those nodes with exactly the same credentials, which is again not practical.

Brieflly, to solve this problem we need to have a centralized database of users and groups using [OpenLDAP](https://www.openldap.org/) then share the information of the users and groups
among the other nodes by using [sssd](https://sssd.io/).

**tasks**

As mentioned, ldap and sssd should be automatically installed and configured, and users/groups
management will be handled with just `cmsh` at the headnode.

To simulate a real life scenario, 

- Create 3 users, give them nice names ;).
- Create two groups, `gpuproject-gp`, `cpuproject-gp`. each group should have only one user. 
- `gpuproject-gp` group have an exclusive access to the directory `/projects/gpuproject`,
- `cpuproject-gp` group have an exclusive access to the directory `/projects/cpuprojects`
- The remaining user will be used later.

check **6.1 Managing Users And Groups With Bright View** section in [BCM documentation](https://support.brightcomputing.com/manuals/9.1/).

## Resources management with slurm (2 days)

In order to organize usage of the available resources in an HPC cluster, a workload manager]
should be installed and configured, and the users should learn how to use it so they can submit
their job on the cluster.

A workload manager can make the usage of the resources efficient and can apply policies as well as
set limits on users for a fairly usage of the resources.

**tasks**

slurm should be configured at the post installation stage of the headnode

By using only `cmsh`:

- Create 3 job queues, defq, gpuq, cpuq.
- add for each queue at least node.
- defq: all the users are allowed to run their jobs in, 1 hour as a time limit, users can only
use one node for each submit.
- gpuq: only `gpuproject-gp` group members are allowed to run their jobs in, 4 hours as a time limit, just one node for each submit.
- cpuq: only `cpuproject-gp` group members are allowed to run their jobs in, 2 hours as a time limi, just one node for each submit.

these jobqueue rules should be applied and working as intended, read carefully https://slurm.schedmd.com/slurm.conf.html and your slurm configuration file, as well as BCM documention about the subject.

## Installing and testing compilers & packages (2 days)

The objective of this part is to install and test different versions of compilers and packages.

**tasks**

- Install and test: 

  - GCC compiler for different version using easybuid
  - Different versions of Python, R, Anaconda, ...
  - Different MPI versions
  - Scientific packages OpenFoam, OpenModelica, ...
  - Visualization tools; Paraview, VTK, ...
  - Profiling tools; Valgrind, Vtune, ..

- See examples here: https://github.com/HPC-Simlab/Tutorials

## Maintenance

In general, HPC clusters are using system monitoring tools such as grafana, ganglia etc ...
to keep track on the physical state of the devices, network state and the resources usage.
There is also [NHC](https://github.com/mej/nhc) that is great for automatically detect
failures within the different services in the system, storage and workload managers.

Maintenance is up to the administrator and how he or she likes to handle things in the cluster
but it is highly recommended to back up the OS image of the headnode (backup point)
and any important configuration file as well as users data (storage), develop scripts for repeated tasks and never stop reading/trying different technologies concerning the HPC clusters.

I hope that this training was helpful to have a basic understanding of how HPC clusters work,
you can always extend you knowledge by reading articles about the subject plus the
documentation of services or the software that you are using to manage your cluster.

Thank you!
