### Create a virtual machine having the os centos.

* Install firewall in the vm(centos might have firewall installed in default).(firewalld or iptables)
* Block certain ip range/subnet using firewalld.
* Allow http, https and ssh connection using firewall.
* You can add other rules as well as you prefer.
* Note: The firewall rules should be saved permanently.

<br>

> Install firewall in the vm

firewalld was already installed in CentOS.

```console
[root@localhost ~]# yum install firewalld
Failed to set locale, defaulting to C.UTF-8
Last metadata expiration check: 6:23:42 ago on Tue Nov  2 03:31:44 2021.
Package firewalld-0.9.3-7.el8.noarch is already installed.
Dependencies resolved.
Nothing to do.
Complete!
```

> Block certain ip range/subnet using firewalld

> Allow http, https and ssh connection using firewall

> You can add other rules as well as you prefer

> Note: The firewall rules should be saved permanently

---

### Create one vm with 2 network interfaces one should behave as WAN and another as LAN. Create another vm attaching the previously created LAN interface to it. 
* Implement NAT in the first vm, so that the second vm can access the internet.
* Note: Configure the first vm as a router, so make the LAN interfaces in the first vm as gateway to the LAN network. And in the second vm configure the gateway to the ip of the first vm LAN ip.

> Implement NAT in the first vm, so that the second vm can access the internet

> Note: Configure the first vm as a router, so make the LAN interfaces in the first vm as gateway to the LAN network. And in the second vm configure the gateway to the ip of the first vm LAN ip.

---
