# Solution to Assignment 3 `Firewall and NAT`


### <a href="/Solution.md#1-create-a-virtual-machine-having-the-os-centos"> 1. Create a virtual machine having the os centos. </a>

* [Install firewall in the vm(centos might have firewall installed in default).(firewalld or iptables)](/Solution.md#firewalld-was-already-installed-in-centos)
* [Block certain ip range/subnet using firewalld.](/Solution.md#firewalld-was-already-installed-in-centos)
* [Allow http, https and ssh connection using firewall.](/Solution.md#we-need-to-add-ports-80---http-443---https-and-22---ssh-we-can-directly-use-service-names-to-use-default-ports-instead-of-writing-them-manually)
* [You can add other rules as well as you prefer.](/Solution.md#blocking-tcp-port-9090)
* Note: The firewall rules should be saved permanent

### <a href="/Solution.md#2-create-one-vm-with-2-network-interfaces-one-should-behave-as-wan-and-another-as-lan-create-another-vm-attaching-the-previously-created-lan-interface-to-it"> 2. Create one vm with 2 network interfaces one should behave as WAN and another as LAN. Create another vm attaching the previously created LAN interface to it. </a> 
* [Implement NAT in the first vm, so that the second vm can access the internet.](/Solution.md#2-create-one-vm-with-2-network-interfaces-one-should-behave-as-wan-and-another-as-lan-create-another-vm-attaching-the-previously-created-lan-interface-to-i)
* Note: Configure the first vm as a router, so make the LAN interfaces in the first vm as gateway to the LAN network. And in the second vm configure the gateway to the ip of the first vm LAN ip.

---

By: Pranav Pudasaini
