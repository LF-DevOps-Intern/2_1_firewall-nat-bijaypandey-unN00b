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
sudo yum install firewalld
```

> Block certain ip range/subnet using firewalld

We are rejecting traffic from 192.168.1.1-254 using firewalld.

```console
# To create a rule
sudo firewall-cmd --permanent --add-rich-rule="rule family='ipv4' source address='192.168.1.1/24' reject"
```

> Allow http, https and ssh connection using firewall

We need to add ports `80 - http`, `443 - https` and `22 - ssh`

```console

firewall-cmd --zone=publicweb --add-service=ssh --permanent

firewall-cmd --zone=publicweb --add-service=http --permanent

firewall-cmd --zone=publicweb --add-service=https --permanent
```

> You can add other rules as well as you prefer

Blocking TCP port 9090

```console
sudo firewall-cmd --zone=public --permanent --add-port=9090/tcp
```

> Note: The firewall rules should be saved permanently

The `permanent` flag was used to create a permanent rule so that the config survies reload/reboot. This can be checked by reloading the firewall.

```console
sudo firewall-cmd --reload
```

---

### Create one vm with 2 network interfaces one should behave as WAN and another as LAN. Create another vm attaching the previously created LAN interface to it. 
* Implement NAT in the first vm, so that the second vm can access the internet.
* Note: Configure the first vm as a router, so make the LAN interfaces in the first vm as gateway to the LAN network. And in the second vm configure the gateway to the ip of the first vm LAN ip.

> Implement NAT in the first vm, so that the second vm can access the internet

> Note: Configure the first vm as a router, so make the LAN interfaces in the first vm as gateway to the LAN network. And in the second vm configure the gateway to the ip of the first vm LAN ip.

---
