---
title: Project Springfield
layout: default
---

What is Project Springfield?
============================

Springfield is a collection of projects unifying multiple levels of the storage
stack and providing a general API for automation, health and status monitoring,
as well as sane and easy configuration across multiple levels of the storage
stack. It is a scalable solution, working from a single node to large
deployments with a mix of base metal, containers and VMs. For the first time,
it was presented on Vault 2017 in a birds of a feather session.

Springfield builds upon and enhances the existing work of [Udisks](https://github.com/storaged-project/udisks),
[libblockdev](https://github.com/storaged-project/libblockdev),
[Blivet](https://github.com/storaged-project/blivet) and others in order to
overcome many of the shortcomings of currently available filesystem tools and
provide a unified approach to storage management. There is nothing like a
binary or a library with the name "Springfield," it is a name for a coordinated
efford across the various tools.


OK, but what does it do?
========================

Let’s back up for a minute and define storage management.  Storage management
refers to a fairly large set of functions -- all of which are, of course,
related to storage. Here’s a high-level list of what it entails:

  * enumerate block devices and network filesystems
  * provide properties for each device, including relations with other devices
  * drives, partitions, lvm, md
  * drives includes: spinning rust, SSD, flash, NVMe, including those behind storage appliances
  * not included or planned: management of storage network switches
  * manage block devices and their formatting
  * device create, destroy, resize, rename, reshape
  * this includes volumes on a storage appliance
  * file system create, destroy, resize, modify (UUID, label), mount, unmount
  * monitor health/status and configuration changes, provide a notification framework
  * device failures and warnings
  * file system full (or nearly full)
  * device created, destroyed, resized, renamed, or reshaped
  * file system created, destroyed, resized, modified (UUID, label)
  * file system mounted, unmounted

Project Comparisons
===================

Language Interfaces
-------

|                | C | Python | DBus | gobject |
| :---           |:---:|:---:|:---:|:---:|
| udisks         | ✓ | ✓ | ✓ | ✓ |
| blivet         |   | ✓ |   |   |
| libblockdev    | ✓ | ✓ |   | ✓ |
| libstoragemgmt | ✓ | ✓ |   |   |


Functionality
------

|   | external storage | iscsi initiator | local storage | noti-fication | plug-ins | model-ing | high-level API |
| :--- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| udisks        |   | ✓ | ✓ | ✓ | ✓ |   |   |
| blivet        |   |   | ✓ | ✓ |   | ✓ | ✓ |
| libblockdev   |   |   | ✓ |   | ✓ |   |   |
| libstoragmgmt | ✓ |   | **?** |   | ✓ |   |   |


Interactions
----

![A diagram of interactions between the projects.]({{ site.url }}/assets/images/interactions.svg)
*A visual representation of the interactions between the projects.*

So what project should I use?
=============================

Which Project Fits My Requirements?
Following are some examples of potential scenarios and guidance as to which of the Springfield projects is best suited for the application.

1. I have a python script that does some lvm management including queries and
   stack modifications. What should I use?
   1. libblockdev or blivet. libblockdev is more direct, almost like directly
      replacing the lvm commands 1:1, but saving you the code for executing
      programs and parsing output. blivet provides a more complete view of the
      system’s storage and provides a bit more leverage, but with more overhead.
      Lastly, lvm provides its own DBus interface for management.
2. I have a C app that does some lvm management including queries and stack
   modifications and must run from the initramfs. What should I use?
   1. libblockdev. blivet is a python module and there is no python runtime in
      the initramfs.
3. I have a storage management application that needs to manage local storage
   and receive notifications when things change on the stack. What should I
   use?
   1. udisks. udisks send signals via DBus whenever a new device is added or
      removed and can also handle most local storage management tasks.
4. I have a storage management application that needs to manage external
   storage arrays or local RAID HBA or SCSI enclosure. What should I use?
   1. libstoragemgmt.

