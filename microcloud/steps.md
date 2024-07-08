# Pre-requisites and understanding the setup

## Setup

first let us get familiar with the setup and what we are using.

Hardware:
1. raspberry pi 5 (4x)
2. switch 16 (1x)

Software:
1. Each raspberry pi has ubuntu 24.04 LTS version installed. 

# What are the requirements?

Hardware requirements:
1. Microcloud needs atleast 3 machines. (can support max 50 machines)
2. Each machine must have atleast 8 gigs of ram. 
3. Each Machine needs to have a local storage disk. Also additionally,
   each cluster should have atleast 3 additional disks.
   (the three additional disks can be attached to any machine in the cluster and will be used 
    for distributed storage by ceph).

Networking requirements:
1. EACH machine needs to have TWO network interfaces mounted.
   one for intra-cluster communication and the other for external connectivity.
   additionally the network interface needs to support broadcast and multicast.
2. The IP addresses of the machines must not change after installation.

Steps To install Microcloud:
On each system install the following:
```
snap install lxd --channel=5.21/stable --cohort="+"
snap install microceph --channel=quincy/stable --cohort="+"
snap install microovn --channel=22.03/stable --cohort="+"
snap install microcloud --channel=latest/stable --cohort="+"
```


```














