# MicroCloudRPI
This is a start-to-finish guide and project to setup Microcloud (ubuntu) using raspberry pi's as nodes.

## What is [Microcloud](https://canonical-microcloud.readthedocs-hosted.com/en/latest/)?

Microcloud is a low resource cloud infra that provides the scalability and reliability of a cloud
for small-scale private sectors or it can act as an edge computing solution.

## What is an edge computer?

Cloud computing is powerful but has bottlenecks, such as latency due to distance. Accessing distant servers can be slow, especially for time-critical processes. Edge computing solves this by performing compute-intensive processes close to the user's location.

In simple terms, an edge computer is a device located near the data sources or users, enabling faster data processing and reduced latency.

Let's take a large scale example:
Company X has locations worldwide and a single data center. Accessing this data center from each location 24/7 causes high latency and server load. This can be solved by deploying edge computers at each location for local processing, with only critical data transferred to the central data center.

How about a small scale one:
Smartwatches use edge computing. Heart rate scanners can't process data, so a processor (edge computer) in the watch processes raw data, converts it to useful information, and:

1. Sends it to the phone if connected.
2. Stores it locally until it can connect to the phone and offload the data.

## How Can MicroCloud Act as an Edge Computer?

MicroCloud provides the scalability and reliability of a cloud for small-scale private sectors or as an edge computing solution. It performs compute-intensive tasks locally, reducing latency and server load. Using a low-maintenance cloud infrastructure like MicroCloud offers the benefits of cloud computing within a local network.

## How does microcloud work?

Microcloud is a mixture of 3 main (OSS) software:

1. [lxd](https://documentation.ubuntu.com/lxd/en/latest/explanation/lxd_lxc/): it is used to bring up virtual machines or containers
2. [Ceph](https://docs.ceph.com/en/reef/start/intro/) it is used to create distributed storage
3. [OVN](https://docs.ovn.org/en/latest/) it is used to manage the networks for uplink

canonical has their own versions of ceph and OVN ie : [microceph](https://canonical-microceph.readthedocs-hosted.com/en/reef-stable/) and [microovn](https://canonical-microovn.readthedocs-hosted.com/en/latest/)

