---
layout: post
title:  "Pwning Neutron Networks"
date:   2018-07-23 12:55:50 -0600
categories: openstack packstack
---

Those who know me, know that I am 100% an OpenStack Fangirl & will never stop
loving the project, but, like a lot of people in the community -- I have a
love/hate relationship with Neutron that can sometimes be heavy on
the hate. It's an awesome project created & maintained by some of the most brilliant people I've ever had the pleasure of speaking with but OMG Y ARE U SO DIFFICULT.

**ahem**

Anyhoo, while preparing architecture for my next course, I spent weeks digging
through [docs][rdodocs], [blogs][rdoblog], video demos & anything else I could find trying to figure
out a simple, straight-forward way to configure Neutron networks with [PackStack][packstack] without the need for fiddling with
network namespaces. Well, after trying out virtually every method I finally
got it working how I wanted, & this is how.

### Environment Setup

For my environment, I'm running [CentOS 7.5][centos] on an [Intel i5
NUC][nuc] with a [500GB Samsung SSD][ssd] & [16GBx2 RAM][ram]. My network name is `eno1`, & my host IP is `192.168.1.166`. Because I don't want to have to drop into a network namespace before accessing my instances I will need to configure my Neutron `public` network on this same subnet.

So, because I'm using [PackStack][packstack] as my deployment tool, I will
need to adjust Neutron settings in `answers.txt` with the following settings:

```
CONFIG_NEUTRON_ML2_TYPE_DRIVERS=vxlan,flat
CONFIG_NEUTRON_OVS_BRIDGE_MAPPINGS=extnet:br-ex
CONFIG_NEUTRON_OVS_BRIDGE_IFACES=br-ex:eno1
...
CONFIG_PROVISION_DEMO_FLOATRANGE=192.168.1.0/24
```

Let's take a closer look at what's going on:


- `CONFIG_NEUTRON_ML2_TYPE_DRIVERS` will ensure that, if needed, I can
  configure a "flat" network without tunnels, vlans, etc.
- `CONFIG_NEUTRON_OVS_BRIDGE_MAPPINGS` will create a physical network named
  `extnet` that will be associated with an OVS bridge named `br-ex` -- all
created by the `packstack` installer.
- `CONFIG_NEUTRON_OVS_BRIDGE_IFACES` will connect the OVS bridge created in
  the previous step - `br-ex` - with the host's public network, which in my
case is `eno1`
- Now, because I left `CONFIG_PROVISION_DEMO` set to the default `=y`, I will
  have to set `CONFIG_PROVISION_DEMO_FLOATRANGE` to match my home network
range, which is `192.168.1.0/24`. Adjust this to fit your own network range,
or if you're feeling fancy you can edit settings in your router to match the
default `172.24.4.0/24` range.

After making a few other customizations, such as changing the default
passwords for `admin` & `demo`, I'm ready to install!

```
$ packstack --answer-file=answers.txt
```

Once installation completes, I should be able to build a server connected to
the automatically created `Private` network, then after attaching a [floating
IP][float] from the `Public` network I should be able to access the server
without namespaces!

Let's see if it works:

```
$ openstack server create myserver --image cirros --flavor m1.tiny \
--key-name mykey --nic net-id=d21c5fde-0f9a-46af-9287-b554acca3538 --wait
```

```
$ openstack floating ip create public
```

```
$ openstack server floating ip associate myserver 192.168.1.10
```

Alright, moment of truth. Let's try to login to `myserver`.

````
# ssh cirros@192.168.1.10
$ hostname
myserver
$ exit
Connection to 192.168.1.10 closed.
```

SUCCESS! So after an embarrassingly long time spent troubleshooting, testing,
& re-OS-ing I've finally solved the problem of simple Provider networks.

[packstack]: https://www.rdoproject.org/install/packstack/
[rdoblog]: https://www.rdoproject.org/networking/neutron-with-existing-external-network/
[rdodocs]: https://docs.openstack.org/networking-ovn/queens/admin/refarch/provider-networks.html
[centos]: https://www.centos.org/download/
[nuc]: https://amzn.to/2LFNUIR
[ram]: https://amzn.to/2LJz8Bc
[ssd]: https://amzn.to/2LHjCWa
[float]: https://docs.openstack.org/python-openstackclient/pike/cli/command-objects/floating-ip.html
[la-profile]: https://linuxacademy.com/profile/show/user/name/trilliams
