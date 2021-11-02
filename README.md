# Solution to Assignment 3 `Firewall and NAT`


### <a href="/Solution.md#1-create-a-virtual-machine-having-the-os-centos"> 1. Create a virtual machine having the os centos. </a>

* Install firewall in the vm(centos might have firewall installed in default).(firewalld or iptables)
* Block certain ip range/subnet using firewalld.
* Allow http, https and ssh connection using firewall.
* You can add other rules as well as you prefer.
* Note: The firewall rules should be saved permanent
<a></a>

### <a href="/Solution.md#2-create-one-vm-with-2-network-interfaces-one-should-behave-as-wan-and-another-as-lan-create-another-vm-attaching-the-previously-created-lan-interface-to-it"> 2. Create one vm with 2 network interfaces one should behave as WAN and another as LAN. Create another vm attaching the previously created LAN interface to it. </a> 
* Implement NAT in the first vm, so that the second vm can access the internet.
* Note: Configure the first vm as a router, so make the LAN interfaces in the first vm as gateway to the LAN network. And in the second vm configure the gateway to the ip of the first vm LAN ip.

---

By: Pranav Pudasaini
