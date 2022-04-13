# Quick guide to setup an HPC cluster with bright cluster manager the free version (easy8)

## Introduction

before diving into the guide let's see quickly how an HPC cluster works,
in general, the structure of an HCP cluster would look like

![image](https://user-images.githubusercontent.com/32802212/163142984-a41ac96d-54eb-4bf3-9f0c-645c4c51cef3.png)

the cluster is composed of headnodes, computenodes, and the storage that is mounted to the nodes through the network. Those three components are
connected with an interconnection network (ethernet, infinitband etc ..).

the headnodes are responsible for managing everything related to the cluster like the deployment, networking, and resources management as well as handling
connections from users via ssh or any remote access tool, this task can be handled by another type of nodes we can name it loginnodes.

usually, HPC clusters are running a Linux system, and because they can scale from tens to thousands of nodes, a lot of tools are created to automate
the management of the clusters (system deployment, networking, workload management) some of them are free (opensource) and others are paid, the difference is, the paid tools are easy to install, use and more automating plus they have support. one of the paid tools is [Bright Cluster Manager](https://www.brightcomputing.com/brightclustermanager) which has a free version called easy8 that is limited to up to 8 nodes. we will use it to get familiar with how an HPC cluster works, and then we can use other opensource tools.

Now let's get started building a virtual cluster based on bright cluster manager

First, we need to have an account in brightcomputing.com, so we can request a license key as well as the IOS image.
This will take several minutes from creating an account and getting approved to downloading the IOS image.

## Get the ISO image of easy8

- Here is the link to create the account https://customer.brightcomputing.com/Customer-Create-Account
- After entering the customer portal, go to *Easy8 — Request Free License* option, fill the required chunks and click request.

<img width="1438" alt="Screen Shot 2022-04-13 at 06 48 45" src="https://user-images.githubusercontent.com/32802212/163118423-6e3b9b7f-f9e0-431c-8d1b-8ac8397552af.png">


<img width="1037" alt="Screen Shot 2022-04-13 at 06 50 16" src="https://user-images.githubusercontent.com/32802212/163118351-4606d92c-34f1-4914-bdef-5d02502b39ec.png">


then you will receive an email contain the license key, and a link to set some settings and download the IOS image,

<img width="950" alt="Screen Shot 2022-04-13 at 06 52 03" src="https://user-images.githubusercontent.com/32802212/163118477-3673b7d4-95d7-4de2-81de-0f871c139724.png">

go to the link and choose what Linux distro suits you and click download. wait several minutes until your download will get ready

<img width="1004" alt="Screen Shot 2022-04-13 at 06 56 03" src="https://user-images.githubusercontent.com/32802212/163118574-7464eb2f-f57b-461a-b351-ab0e9aa28c00.png">

<img width="711" alt="Screen Shot 2022-04-13 at 07 04 01" src="https://user-images.githubusercontent.com/32802212/163119507-523cd64d-ff3a-4a96-a16e-faf96489948b.png">


wait several minutes until your download will get ready

<img width="780" alt="Screen Shot 2022-04-13 at 07 04 10" src="https://user-images.githubusercontent.com/32802212/163119309-91d7cd35-f8c7-4e0e-8639-187af2062ff6.png">

reload the web page from time to time until you get the following page, click on the ISO file to download the ISO image

<img width="1155" alt="Screen Shot 2022-04-13 at 07 26 53" src="https://user-images.githubusercontent.com/32802212/163123220-b1881a0d-11df-4a0b-81e9-62063aa7292f.png">

## Preparing for the installation

- we will use the *bare metal* installation method, which means installing the IOS image on the machine with no
operating system, this can be done either on a physical machine or a virtual one. For the physical machines,
you can use a USB or a DVD to store the ISO image and boot it from them. but in this tutorial, we will use virtual machines with VirtualBox, here are the steps
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

So based on that our cluster will have two networks, one is the external network which will include only the head/logins nodes, and a private network that includes all the compute nodes plus head/login nodes, in real life the nodes will be connected to a switch, inside the switch will be two [VLANs](https://en.wikipedia.org/wiki/Virtual_LAN) which will represent the two networks, but in VirtualBox, we will simply add network adapters and set there types to the following:

network settings for the head node:

select the headnode and click settings

<img width="1136" alt="Screen Shot 2022-04-13 at 07 47 37" src="https://user-images.githubusercontent.com/32802212/163127820-8c9a6201-8936-4791-bc18-492ea9d973f0.png">

choose network
<img width="761" alt="Screen Shot 2022-04-13 at 07 48 43" src="https://user-images.githubusercontent.com/32802212/163127890-f20be1c6-8554-43ec-a1f4-46c5fb205410.png">

At "attached to" option, change NAT to bridged adapter, so we can configure a static ip address of the headnode, and be able to remote access it via ssh
<img width="761" alt="Screen Shot 2022-04-13 at 07 50 30" src="https://user-images.githubusercontent.com/32802212/163128783-34d9e5f7-9b1b-4e82-8944-4aabfd73bf08.png">

note that the first adapter will be used by the external network, but for the internal one, we will add another one with a simple modification.
click on "adapter 2", check the box "Enable Network Adapter", at the "Attached to" option select internal network, and keep the default name. this should look like

<img width="761" alt="Screen Shot 2022-04-13 at 07 51 40" src="https://user-images.githubusercontent.com/32802212/163129546-b272a662-fd35-44e6-b1f6-2429cac4f1c8.png">

click "ok" to save the settings.

For node01 we need just to change its first adapter from NAT to internel network,

<img width="761" alt="Screen Shot 2022-04-13 at 08 07 29" src="https://user-images.githubusercontent.com/32802212/163130210-1c083051-5371-4ea7-ac7f-3fb966b85f69.png">

click "ok" to save the settings.

we need an aditional modification within the node01 so it can boot from the network via [IPXE](https://en.wikipedia.org/wiki/IPXE).
in settings->system, check the network's box and drag it to the top
<img width="761" alt="Screen Shot 2022-04-13 at 08 11 12" src="https://user-images.githubusercontent.com/32802212/163131090-90c25e67-57fc-4d5e-a283-f985cccc8a1b.png">
<img width="761" alt="Screen Shot 2022-04-13 at 08 11 45" src="https://user-images.githubusercontent.com/32802212/163131111-cbaf979e-2f75-4799-b9ae-4ecac1937666.png">

## the start-up

now we can bring up the cluster, by powering on the head node first and boot it with the ISO image.
In the VirtualBox window, double on the headnode or click the arrow start
<img width="1136" alt="Screen Shot 2022-04-13 at 08 19 49" src="https://user-images.githubusercontent.com/32802212/163132456-ca1d57c2-5b52-491e-82dd-0756932ad40f.png">

click the browse icon
<img width="748" alt="Screen Shot 2022-04-13 at 08 20 50" src="https://user-images.githubusercontent.com/32802212/163133108-9837f14d-08b0-4807-87f6-16dd0ec020e4.png">

click "Add disk image" icon

<img width="795" alt="Screen Shot 2022-04-13 at 08 24 24" src="https://user-images.githubusercontent.com/32802212/163133792-c981571d-1f13-4829-94d2-777885b27a3e.png">

find the ISO image (usually it will be in the downloads folder), select it, click open

<img width="912" alt="Screen Shot 2022-04-13 at 08 34 35" src="https://user-images.githubusercontent.com/32802212/163135577-1592b516-cfbd-452c-b4a8-5222646d3655.png">

choose the loaded ISO image and click "choose"

<img width="795" alt="Screen Shot 2022-04-13 at 08 34 59" src="https://user-images.githubusercontent.com/32802212/163135697-c0ab5e89-a29f-49cf-8741-4d78d6a1d135.png">

finally, click start

<img width="748" alt="Screen Shot 2022-04-13 at 08 49 50" src="https://user-images.githubusercontent.com/32802212/163138271-c81faf2f-06d3-4fa2-9aa7-2914ab29036a.png">

if everything went well this ISO boot menu will show up

![VirtualBox_headnode_13_04_2022_08_52_35](https://user-images.githubusercontent.com/32802212/163139197-e229e336-a4e1-4361-8282-d6c1aa4fb3e2.png)

select "start bright cluster manager graphical installer" and procced to the installation section

## Installation

for the installation steps, please refer to https://support.brightcomputing.com/manuals/9.2/installation-manual.pdf section *3.3 Head Node Installation: Bare Metal Method*

Be careful with the network configuration, after the installation you should be able to connect to the internet, so you can update system packages and upgrade them, by `sudo yum update` and then `sudo yum upgrade`, after that bring your product key and install your license, please refer to  https://support.brightcomputing.com/manuals/9.2/installation-manual.pdf section *4.3.3 Requesting A License With The request-license Script*

## power on a compute node

after installing the headnode properlly and while it is running, now, we can power on node01 by double-clicking it in Virutaulbox, it will ask you to load an ISO but we setup node01 to be booted from the network, so we can just click cancel, after few moments the VM should look like:

<img width="717" alt="Screen Shot 2022-04-13 at 10 41 27" src="https://user-images.githubusercontent.com/32802212/163163564-9da77126-07da-4be9-a151-0a6d8c55bf4d.png">

if everything went well, the VM should look like:

![image](https://user-images.githubusercontent.com/32802212/163165600-d0caab30-8863-4194-8f59-f2f7997f437d.png)

choose "AUTO" and procced by hitting enter.
you will be asked to accept the given configuratoin or choose manually, choose accept the configuratoin looks right

![image](https://user-images.githubusercontent.com/32802212/163166215-1fbea6b1-4d99-4fa6-b328-a11ed1afafda.png)

then choose "AUTO" for the install mode.

![image](https://user-images.githubusercontent.com/32802212/163166300-57c39d3e-c5d8-4e9e-a796-c43932681eda.png)

after node installation the screen should look like

![image](https://user-images.githubusercontent.com/32802212/163166457-aba1bdb1-3cde-4a99-a864-71e94b27bbd8.png)

At this point, you have a very fresh mini HPC cluster based on birght cluster manager, please refer to https://support.brightcomputing.com/manuals/9.2/admin-manual.pdf for advanced usage

if you need help, contact mostafa.mamouni@um6p.ma

Thank you














