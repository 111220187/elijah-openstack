OpenStack extension for Cloudlet support
========================================================
A cloudlet is a new architectural element that arises from the convergence of
mobile computing and cloud computing. It represents the middle tier of a
3-tier hierarchy:  mobile device - cloudlet - cloud.   A cloudlet can be
viewed as a "data center in a box" whose  goal is to "bring the cloud closer".

Copyright (C) 2012-2013 Carnegie Mellon University This is a developing project
and some features might not be stable yet.  Please visit our website at [Elijah
page](http://elijah.cs.cmu.edu/).



License
----------

All source code, documentation, and related artifacts associated with the
cloudlet open source project are licensed under the [Apache License, Version
2.0](http://www.apache.org/licenses/LICENSE-2.0.html).



Prerequisites
-------------

1. OpenStack installation: This work assumes that you already have working
   OpenStack.  For installation of OpenStack, you can follow the [official
   documentation](http://docs.openstack.org/grizzly/openstack-compute/install/apt/openstack-install-guide-apt-grizzly.pdf).
   Since OpenStack is a collection of multiple semi-independent project, its
   installtion is not trivial if you don't have any experience. But once you
   have installed that, cloudlet patch would be simple. Please take a look at
   (my note) for the installation tip.


2. You need extra IP address to allocate IP to the synthesized VM. You can
   still execute VM synthesis using this extension, but can't use a mobile
   device to access to the synthesized VM.

3. The tested platform is Ubuntu 12.04 LTS 64 bits.



Installation
------------

Here we provide a script to apply this extension to the existing OpenStack.
It will install cloudlet library and apply pathches for the cloudlet extension.
This patch **does not change any existing source code**, but designed to be
purely pluggable extension, so you can revert back to original state by
reversing the installation steps.

1. Install libraries for the fabric script.

	> $ sudo apt-get install git openssh-server fabric

2.  You first need to specify IP addresses of your OpenStack machine at the
	installation script, **fabric.py** file.  We assume you run this script at
	control node, so you only need to list the ip address of compute machines.
	If you are testing with a single node (run both control and compute at one
	machine) you don't need this step.
	
		> (At fabric.py file)
		> compute_nodes = [
		> 		('ssh_username', 'ip address or domain name of node')
		> 		('krha', 'sleep.elijah.cs.cmu.edu')
		> 		..
		> 		]


3. Install cloudlet extension for both control node and compute node at localhost.

		> $ fab localhost install_control()


3. Install cloudlet extension at compte nodes.  Again, if you are testing with
   single node (all-in-one case), you don't need this step since
   install_control does already did path for computation.

		> $ fab remote install_compute()


How to use
-----------

First, if the installation is successful, you should be able to see update
Dashboard as below.  ![OpenStack cloudlet extension
dashboard](https://raw.github.com/cmusatyalab/elijah-openstack/tree/master/doc/screenshot/cloudlet_dashboard.png)
Or you can chech cloudlet extension by listing existing extension using
standard openstack API.



Known Issues
------------

1. Possible resource leak from unexpected OpenStack termination

2. __Early start optimization__ is turned off
	- Early start optimization splits VM overlay into multiple segments (files) 
	- Need better packaging for VM overlay to handle segments
