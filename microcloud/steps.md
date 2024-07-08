# Pre-requisites and understanding the setup

## Setup

This is the setup for a physical microcloud cluster, you can refer the microcloud docs to create a completely virtual cluster.

### Hardware:
1. raspberry pi 5 (3x) and power for each of them
2. switch 16 (1x)
3. router for uplink connection (1x)
4. Ethernet cables (4x) 3 to connect the rpis and another one to connect to router.

### Software:
1. Each raspberry pi has ubuntu 24.04 LTS version installed.
2. Wifi network interface has been statically configured on the router
3. Ethernet interface has been set so that it does not allocate any ip address.

Network Connection Diagram:
(please refer diagram)
Each ethernet interface is connected to the switch and one port of the switch is connected to the router (the router connection is the uplink connection)

## What are the requirements?

### Hardware requirements:
1. Microcloud needs atleast 3 machines. (can support max 50 machines)
2. Each machine must have atleast 8 gigs of ram. 
3. Each Machine needs to have a local storage disk. Also additionally,
   each cluster should have atleast 3 additional disks.
   (the three additional disks can be attached to any machine in the cluster and will be used 
    for distributed storage by ceph).

### Networking requirements:
1. EACH machine needs to have TWO network interfaces mounted.
   one for intra-cluster communication and the other for external connectivity.
   additionally the network interface needs to support broadcast and multicast.
2. The IP addresses of the machines must not change after installation.

## Steps To install Microcloud:
On each system install the following:
```
snap install lxd --channel=5.21/stable --cohort="+"
snap install microceph --channel=quincy/stable --cohort="+"
snap install microovn --channel=22.03/stable --cohort="+"
snap install microcloud --channel=latest/stable --cohort="+"
```
Select One of the RPI as the head of the microcloud (we will initialize microcloud on this device)

On this device run the command `microcloud init`

Go through the initialization process and once it is done we have set up a microcloud cluster.

## What exactly is happening in the network side of things:
The raspberry pi setup we are using is a bit different from the tutorial that has been given in the 
[microcloud documentation](https://canonical-microcloud.readthedocs-hosted.com/en/latest/tutorial/get_started/)
According to the original setup of microcloud we need 2 networks:
1. local network between the nodes (needs to be pre configured with ip address)
2. Uplink network needs to be unconfigured.

What does this mean: Microcloud uses microovn to handle the network aspects of the system
This means that the local network needs to be pre configured so that intra-cluster traffic is connected all the time and there need not be any ip address allocation
however for uplink network we require ovn to allocate the ip address and this will fail if the router/modem providing internet access allocates the address to the systems hence we need to make sure that the ip remains unallocated by default from the router.

Allright so now whats going on in our raspberry pi setup?

We have 2 available network interfaces by default on the raspberry pi.
the wifi interface and the ethernet interface

since we have wifi interface it is logical to use that for uplink and ethernet for local traffic, unfortunately this system will not work out, for the sole reason that we cannot access the raspberry pi via ssh otherwise.

Therefore we have statically configured the wifi so that we can access the raspberry pis easily from an external machine incase we need to make changes to each device individually. But since the wifi has been statically configured we know that cannot be used for uplink network. There is also the "inference" that for the local network we should not have any internet access ie the local network should not be connected via the internet. However this is not the case, since the wifi networks are connected to a single router, the router can act as a switch between the devices and they can talk to each other without touching the external world at all. Therefore we have the local network already setup.

The other side of the coin is the ethernet port which needs to be connected to a switch and one more wire needs to run to the main router so that the cluster can talk to the outside world.

Also as of this commit, we are not going to be using distributed storage, since there are no external disks added to the system, during initialization microcloud will automatically detect the lack of disks and prevent you from setting up distributed storage.

It is also to be noted that microcloud stores data in the form of images, so when you bring up any operating system it is stored in the form of an image and this is stored on the disc of the head of the cluster, so it is suggested to use a disc with more storage for the head atleast.

Another point to note is that ipv6 has not been setup.

### setting up a storage pool:

The exact cause remains unknown, but we can make an assumption that the local storage pool is not configured because the distributed storage has not been setup:
Due to this reason we have to manually create a storage pool, this can be done as follows:
```
sudo lxc cluster list
sudo lxc storage create local dir --target <node>
(need to run this command for each node of the microcloud)
sudo lxc storage create local dir
sudo lxc profile device add default root disk path=/ pool=local
```
once setup you can inspect the setup of the cluster using the following commands:
```
Cluster Setup:
lxc cluster list
microcloud cluster list
microceph cluster list
microovn cluster list

Storage Setup:
lxc storage list
lxc storage info local

Network Setup:
lxc network list
lxc network show default
(you can ping the ip shown against volatile.network.ipv4.address to check if uplink is working)
```

You can launch an instance with the following command:
`lxc launch ubuntu:22.04 u1 --storage local`
`lxc launch <image> <image_name> --storage local`
Ensure you give the `--storage local` flag, because by default microcloud looks for the remote storage and this will not work since we dont have it setup.

You can inspect the instances using:
```
lxc storage volume list local
lxc list
```

Once a instance has been brought up you can enter the instance using:
`lxc exec <instance_name> -- bash`

You can check if the networks are connected by pinging the ip addresses of different instances from one instance using the ip addresses given at lxc list. You can also test internet connection from the instance using `ping www.google.com`

Note: you cannot ping the ip addresses of an instance from the external world.
