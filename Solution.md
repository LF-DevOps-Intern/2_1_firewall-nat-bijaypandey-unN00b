### Create a virtual machine having the os centos.

* Install firewall in the vm(centos might have firewall installed in default).(firewalld or iptables)
* Block certain ip range/subnet using firewalld.
* Allow http, https and ssh connection using firewall.
* You can add other rules as well as you prefer.
* Note: The firewall rules should be saved permanently.

<br>

> Install firewall in the vm

##### firewalld was already installed in CentOS.

```console
sudo yum install firewalld
```

![1a-install](https://user-images.githubusercontent.com/23631617/139876925-95af09bc-9e2c-46ee-9fb2-7aad6ae156c4.png)

> Block certain ip range/subnet using firewalld

##### We are rejecting traffic from the subnet 192.168.1.1/24 using firewalld.

```console
# To create a rule
sudo firewall-cmd --permanent --add-rich-rule="rule family='ipv4' source address='192.168.1.0/24' reject"
```

![Listing Rules](https://user-images.githubusercontent.com/23631617/139878063-c754659c-33cc-43b7-90de-6edfe93c6190.png)

> Allow http, https and ssh connection using firewall

We need to add ports `80 - http`, `443 - https` and `22 - ssh`. We can directly use service names to use default ports instead of writing them manually.

```console
firewall-cmd --zone=publicweb --add-service=ssh --permanent

firewall-cmd --zone=publicweb --add-service=http --permanent

firewall-cmd --zone=publicweb --add-service=https --permanent
```

![Ports](https://user-images.githubusercontent.com/23631617/139878525-5336b213-bb97-4689-b0e8-73510f695a56.png)

> You can add other rules as well as you prefer

##### Blocking TCP port 9090

```console
sudo firewall-cmd --zone=public --permanent --add-port=9090/tcp
```

> Note: The firewall rules should be saved permanently

##### The `permanent` flag was used to create a permanent rule so that the config survies reload/reboot. This can be checked by reloading the firewall.

```console
sudo firewall-cmd --reload
```

##### The following screenshot confirms that our firewall config survived reloading of the daemon.

![permanent](https://user-images.githubusercontent.com/23631617/139879084-78eadb67-6b1c-46ee-bd68-5b96d40be1c4.png)

---

### Create one vm with 2 network interfaces one should behave as WAN and another as LAN. Create another vm attaching the previously created LAN interface to it. 
* Implement NAT in the first vm, so that the second vm can access the internet.
* Note: Configure the first vm as a router, so make the LAN interfaces in the first vm as gateway to the LAN network. And in the second vm configure the gateway to the ip of the first vm LAN ip.

> Implement NAT in the first vm, so that the second vm can access the internet

Since I'm using QEMU for virtualization, I had no readily-available host-only interface. So, first step was to create a virtual adapter to libvirt using virsh with this config file. This had to be done in the host OS.

```console
# To define new interface from config file
virsh net-define router-lan-config.xml

# To start the interface
virsh net-start router-lan
```

```xml
# Referenced from: https://kevrocks67.github.io/blog/qemu-host-only-networking.html

# router-lan-config.xml

<network>
  <name>router-lan</name>
  <uuid>8f49de66-0947-4271-85a4-2bbe88913555</uuid>
  <bridge name='router-lan' stp='on' delay='0'/>
  <mac address='52:54:00:95:26:26'/>
</network>
```

Then, the router VM was assigned 2 network interfaces - one with internet access and another with host-only interface, whereas the client VM was assigned only host-only interface. The host-only interface is to be set up as a LAN and the client should be able to get internet access using NAT.

> Note: Configure the first vm as a router, so make the LAN interfaces in the first vm as gateway to the LAN network. And in the second vm configure the gateway to the ip of the first vm LAN ip.

---
