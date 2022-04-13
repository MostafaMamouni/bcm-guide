# Quick guide to setup an HPC cluster with bright cluster manager the free version (easy8)

Firs we need to have an account in brightcomputing.com, so we can request a license key as well as the IOS image.
This will take severl minutes from creating an accouny and get approved to downloading the IOS image.

## Get the ISO image of easy8

- Here is the link to create the account https://customer.brightcomputing.com/Customer-Create-Account
- After entering the customer portal, go to *Easy8 — Request Free License* option, fill the required chunks and click request.

<img width="1438" alt="Screen Shot 2022-04-13 at 06 48 45" src="https://user-images.githubusercontent.com/32802212/163118423-6e3b9b7f-f9e0-431c-8d1b-8ac8397552af.png">


<img width="1037" alt="Screen Shot 2022-04-13 at 06 50 16" src="https://user-images.githubusercontent.com/32802212/163118351-4606d92c-34f1-4914-bdef-5d02502b39ec.png">


then you will receive an email contain the license key, and a link to set some settings and download the IOS image,

<img width="950" alt="Screen Shot 2022-04-13 at 06 52 03" src="https://user-images.githubusercontent.com/32802212/163118477-3673b7d4-95d7-4de2-81de-0f871c139724.png">

go to the link and choose what linux distro that suits you and click download.wait several minutes untill your download will get ready

<img width="1004" alt="Screen Shot 2022-04-13 at 06 56 03" src="https://user-images.githubusercontent.com/32802212/163118574-7464eb2f-f57b-461a-b351-ab0e9aa28c00.png">

<img width="711" alt="Screen Shot 2022-04-13 at 07 04 01" src="https://user-images.githubusercontent.com/32802212/163119507-523cd64d-ff3a-4a96-a16e-faf96489948b.png">


wait several minutes untill your download will get ready

<img width="780" alt="Screen Shot 2022-04-13 at 07 04 10" src="https://user-images.githubusercontent.com/32802212/163119309-91d7cd35-f8c7-4e0e-8639-187af2062ff6.png">

reload the web page from time to time untill you get the following page, click on the ISO file to download the ISO image

<img width="1155" alt="Screen Shot 2022-04-13 at 07 26 53" src="https://user-images.githubusercontent.com/32802212/163123220-b1881a0d-11df-4a0b-81e9-62063aa7292f.png">

## Preparing for the insatllation

- we will use the *bare metal* installation method, that means installing the IOS image on machine with no
operation system, this can be done either on physical machine or a virtual one. For the physical machines
you can use USB or a DVD to store the ISO image and boot it from them. but in this turorial we will use virtual machines with VirtualBox, here the steps
to create the VMs.

NOTE: we will create two VMs, the "headnode" and "node01" with the same steps.

<img width="1136" alt="Screen Shot 2022-04-13 at 07 31 32" src="https://user-images.githubusercontent.com/32802212/163124624-cbdc6578-826b-4bcc-97e6-41ba777c7e8c.png">
<img width="1136" alt="Screen Shot 2022-04-13 at 07 32 59" src="https://user-images.githubusercontent.com/32802212/163124649-28e1cec0-a8df-41a5-8966-69999b71147b.png">
<img width="1136" alt="Screen Shot 2022-04-13 at 07 33 25" src="https://user-images.githubusercontent.com/32802212/163124691-cfe6a45b-4917-474d-b554-e9dec96b91a1.png">
<img width="1136" alt="Screen Shot 2022-04-13 at 07 33 32" src="https://user-images.githubusercontent.com/32802212/163124719-121c6962-9d11-4a7d-9c6e-23f2bd112541.png">
<img width="1136" alt="Screen Shot 2022-04-13 at 07 33 39" src="https://user-images.githubusercontent.com/32802212/163124764-591934c7-4358-4543-9c68-d47947c15e1e.png">
<img width="1136" alt="Screen Shot 2022-04-13 at 07 33 47" src="https://user-images.githubusercontent.com/32802212/163124801-bb6ad816-68f8-4cbc-b02c-3a9bd0bf4f66.png">
<img width="1136" alt="Screen Shot 2022-04-13 at 07 34 12" src="https://user-images.githubusercontent.com/32802212/163124816-681c9334-2ca8-42a9-a8ef-f983308899b8.png">

then we need to decide what network topology our cluster will use, usually in HPC clusters the following network topology is used:

<img width="576" alt="Screen Shot 2022-04-13 at 07 15 45" src="https://user-images.githubusercontent.com/32802212/163120839-a914e980-c380-4e6f-ac3e-bdd637e892a9.png">

So based on that our cluster will have two networks, one is the exsternel network which will include only the head/logins nodes, and a private network that includes all the compute nodes plus head/login nodes, in real life the nodes will be connected to a switch, inside the switch will be two [VLANs](https://en.wikipedia.org/wiki/Virtual_LAN) which will represent the two networks, but in VirtualBox we will simply add network adapters and set there types to the following:

network settings for the head node:

select the headnode and click settings

<img width="1136" alt="Screen Shot 2022-04-13 at 07 47 37" src="https://user-images.githubusercontent.com/32802212/163127820-8c9a6201-8936-4791-bc18-492ea9d973f0.png">

choose network
<img width="761" alt="Screen Shot 2022-04-13 at 07 48 43" src="https://user-images.githubusercontent.com/32802212/163127890-f20be1c6-8554-43ec-a1f4-46c5fb205410.png">

At "attached to" option, change NAT to briged adapter, so we can configure a static ip address of the headnode, and be able to remoe access it via ssh
<img width="761" alt="Screen Shot 2022-04-13 at 07 50 30" src="https://user-images.githubusercontent.com/32802212/163128783-34d9e5f7-9b1b-4e82-8944-4aabfd73bf08.png">

note that the first adapter will be used by the externel network, but for the internel one we will add another one with a simple midification.
click on "adapter 2", check the box "Enable Network Adapter", at the "Attached to" option select internel network, keep the default name. this should look like

<img width="761" alt="Screen Shot 2022-04-13 at 07 51 40" src="https://user-images.githubusercontent.com/32802212/163129546-b272a662-fd35-44e6-b1f6-2429cac4f1c8.png">

click "ok" to save the settings.

For node01 we need just to change its first adapter from NAT to internel network,

<img width="761" alt="Screen Shot 2022-04-13 at 08 07 29" src="https://user-images.githubusercontent.com/32802212/163130210-1c083051-5371-4ea7-ac7f-3fb966b85f69.png">

click "ok" to save the settings.

we need an aditional modification within the node01 so it can boot from the network via [IPXE](https://en.wikipedia.org/wiki/IPXE).
in settings->system, check the network's box and drag it to the top
<img width="761" alt="Screen Shot 2022-04-13 at 08 11 12" src="https://user-images.githubusercontent.com/32802212/163131090-90c25e67-57fc-4d5e-a283-f985cccc8a1b.png">
<img width="761" alt="Screen Shot 2022-04-13 at 08 11 45" src="https://user-images.githubusercontent.com/32802212/163131111-cbaf979e-2f75-4799-b9ae-4ecac1937666.png">

## the start up

now we can procced to bring up the cluster, by power on the head node first and boot it with the ISO image.
In the VirtualBox window, double on the headnode or click the arrow start
<img width="1136" alt="Screen Shot 2022-04-13 at 08 19 49" src="https://user-images.githubusercontent.com/32802212/163132456-ca1d57c2-5b52-491e-82dd-0756932ad40f.png">

click the browse icon
<img width="748" alt="Screen Shot 2022-04-13 at 08 20 50" src="https://user-images.githubusercontent.com/32802212/163133108-9837f14d-08b0-4807-87f6-16dd0ec020e4.png">

click "Add disk image" icon

<img width="795" alt="Screen Shot 2022-04-13 at 08 24 24" src="https://user-images.githubusercontent.com/32802212/163133792-c981571d-1f13-4829-94d2-777885b27a3e.png">

find the ISO image (usually it will be in the downlaods folder), select it, click open

<img width="912" alt="Screen Shot 2022-04-13 at 08 34 35" src="https://user-images.githubusercontent.com/32802212/163135577-1592b516-cfbd-452c-b4a8-5222646d3655.png">

choose the loaded ISO image and click "choose"

<img width="795" alt="Screen Shot 2022-04-13 at 08 34 59" src="https://user-images.githubusercontent.com/32802212/163135697-c0ab5e89-a29f-49cf-8741-4d78d6a1d135.png">














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
