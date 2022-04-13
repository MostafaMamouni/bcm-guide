# Quick guide to setup an HPC cluster with bright cluster manager the free version (easy8)

Firs we need to have an account in brightcomputing.com, so we can request a license key as well as the IOS image.
This will take severl minutes from creating an accouny and get approved to downloading the IOS image.

## Get the ISO image of easy8

- Here is the link to create the account https://customer.brightcomputing.com/Customer-Create-Account
- After entering the customer portal, go to *Easy8 — Request Free License* option, fill the required chunks and click request. then you will receive an email contain the license key, and a link to download the IOS image

## Preparing for the insatllation

- we will use the *bare metal* installation method, that means installing the IOS image on machine with no
operation system, this can be done either on physical machine or a virtual one. For the physical machines
you can USB or a DVD to store the ISO image and boot it from them.

## Installation

- After the start up of the machine, an *ISO boot menu* will show up to choose either
booting from the hard drive or text/graphical installation.
- Because it's first time, so we will choose the graphical installation for this guide.
- In the welcome screen choose insatllation.
- At bright EULA page check agree check box
- At the kernel modules page, Some modules will be automatically seleceted to be loaded depend on
the machine's hardware, but you can add/remove some of them in this page.
- At the hardware info page, you can click next if no hardware is missing otherwise go back to the kernel
modules page and add the corespondens kernel module of the missing hardware.
- At the installation source page, choose the insatllation device and click next
- Here are the settings that can configured in cluster settings page:
    - **Cluster name**
    - **Administrator email**: Where mail to the administrator goes. This need not be local.
    - **Time zon**e
    - **Time servers**: The defaults are pool.ntp.org servers. A time server is recommended to avoid problems due to time discrepancies between nodes.
    - **Nameservers**: One or more DNS servers can be set.
    - **Search domains**: One or more domains can be set.
    - **Environment modules**: Traditional Tcl modules are set by default. Lmod is an alternative.
    - **Console**: The display mode of the console is configured for the:
    - **Head node**: Text by default. Graphical is an alternative.
    - **Compute nodes**: Text by default. Graphical is an alternative
- In the workload manager page, select the preferd workload manager.
- In the network topology page, select the suitable topology for your cluster based on
you network environment.
- Configure the hostname, password(for the root user), and the hardware manfacuturer settings for the headnode in its configuration page.
- set the following settings of the compute nodes:
    - Number of racks
    - Number of nodes
    - Number of the start
    - Node base name
    - Node digits
    - hardware manufacturer
- Configure The BMC (Baseboard Management Controller) if you have it in your network.
- Set up the network interfaces for the headnode
- Set up the network interfaces for the the compute nodes (consider adding an additional network instaface to the nodes that are intended to be the connection nodes so the user wont the headnode for submiting their jobs)
- In the disk layout page, select a drive for the head node and choose its type partitioning.
- In the additional software page, Choose to install an additional software from the avaiable ones.
- Check if it's all right in the sammury page and click start.
- After all the requirements get checked in the deployment page, clik reboot

## After the installation

- After the reboot, you need to request a license for your cluster so you can benefit from the easy8's features and `cmsh` (cluster manager shell). this can be done by typing `request-license` on the terminal, it will ask you to provide the license key and other infos.
- To connect to the other nodes which are `bare metal` machines, you need just to configure thier booting type to be an IPXE boot, so the headnode will detect them and start provisioning the software image to them.