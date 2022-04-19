## Cluster configuration

These are the tasks conerning how the cluster should look like in order to make slurm tasks doable.

set up an hpc cluster with the following configuration.

### **headnode**

turn-in : a full technical discription about, how the headnode is different from the other 
nodes, system services, network configuration, users/groups management, resources mangement. 
send the text discription as well as network configuration files, slurm configuration files 
of its demons (slurmctld, slurmdbd, slurmd), the output of `systemctl status *service*`, \*service\* is demon that just is running only in headnode. all the system sercies should be
running fine.
check bcm and slurm documentations

## **login node**

most of the HCP clusters now have loging nodes, the users can access the clusters through
them as well as submit jobs from, login nodes have small changes in thier configuration concerning networking, system and slurm (pay attention to this one) that distinguish them from the compute nodes and the headnode.

turn-in: a dicription about what distinguishes a login node from other types of nodes, you can
send its network configuration files, the configuration file of `slurmd`, the ouput of `cmsh -c "device; use headnodename; roles; ls `, the output of the `systemctl status slurmctld`, `systemctl status slurmd`, `systemctl status slurmdbd` you can guesse where those command should executed, also a summary of what you have observed.
the documention is your friend.

## **3 compute nodes**

turn-in: as usual, a dicription about what distinguishes a compute node from other types of 
nodes, the output of `cmsh -c "device; use anycomputenode; roles; ls `,
`systemctl status slurmd` and configuratin file of `slurmd`, any other notes are welcomed
the documention is your friend.

if you havn't read about ldap and sssd, it's time to do so.

after having a typical HPC cluster as well as a deep understading of all the administartion aspects of it, now we can make it useful for the users, here are some straight forward tasks concerning users, groups and resouces management.

## **users and groups**

create 3 users, give them nice names ;), and two groups, `gpuproject-gp`, `cpuproject-gp`. each group
should have only one user. `gpuproject-gp` group have an exclusive access to the directory `/projects/gpuproject`,
`cpuproject-gp` group have an exclusive access to the directory `/projects/cpuprojects`, still one user without a group!, let it alone :D. make sure of those last things by trying to access the directories with users don't the access rights.

turn-in: the output of, `cmsh -c "user; ls"`, `cmsh -c "group; ls"`, `ls -l /projects/gpuproject`
, `ls -l /project/cpuproject`, use the command `id` on each user and send the output.

cmsh, chmod, chown are your friends and the documention ofc.

## **set up job queues**

create 3 job queues, defq, gpuq, cpuq. aleast 1 compute node for each.

- defq: all the users are allowed to run their jobs in, 1 hour as a time limit, users can only
use one node at each submit.

- gpuq: only `gpuproject-gp` group members are allowed to run their jobs in, 4 hours as a time limit, just one node for each submit.

- cpuq: only `cpuproject-gp` group members are allowed to run their jobs in, 2 hours as a time limi, just one node for each submit.

these job queue rules should be aplied and working as intended, read carfully https://slurm.schedmd.com/slurm.conf.html and your slurm configuratin file, as well as bcm documention about the subject.

turn-in: the slurm configuration file, the output of `sinfo`, try to submit jobs with users
that don't have acces to certain, send the output of `sinfo` after each submission, some submissions should be successful, send the output of the raised message from slurm. any other
tests or notes are welcomed

hints: each device has roles, each category has roles.

cmsh is your best friend, slurm and bcm documentions are you friends too.

For the up comming tasks: check https://slurm.schedmd.com/accounting.html

Thank you!