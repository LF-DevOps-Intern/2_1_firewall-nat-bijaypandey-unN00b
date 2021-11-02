### 1. Create a virtual machine having the os centos.

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

### 2. Create one vm with 2 network interfaces one should behave as WAN and another as LAN. Create another vm attaching the previously created LAN interface to it. 
* Implement NAT in the first vm, so that the second vm can access the internet.
* Note: Configure the first vm as a router, so make the LAN interfaces in the first vm as gateway to the LAN network. And in the second vm configure the gateway to the ip of the first vm LAN ip.

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

##### Router's Network Interfaces

###### Router Host-only Adapter
![router-hostonly](https://user-images.githubusercontent.com/23631617/139914358-a7f01b9d-84f2-43e8-92a2-9b2f1171d558.png)

###### Router's NAT adapter (with internet access)
![router-nat](https://user-images.githubusercontent.com/23631617/139914412-f2d5a655-24bb-4504-99e7-aaca97cdf375.png)


##### Client's Network Interface

![client-hostonly](https://user-images.githubusercontent.com/23631617/139914701-cac33641-61ea-4261-8337-10436fa6d698.png)

> Implement NAT in the first vm, so that the second vm can access the internet

On router VM, the host-only interface should be configured

```console
ifconfig ens8 192.168.0.1 netmask 255.255.255.0
```

Similarly on client VM,

```console
ifconfig ens3 192.168.0.2 netmask 255.255.255.0
```

Attempting to ping router from client and vice-versa.

![host-guest-comms](https://user-images.githubusercontent.com/23631617/139915434-9778ff44-6ec4-442d-a771-cc59d1ebab40.png)

To enable NAT, first we need to load `iptable_nat` kernel module, then turn on the `ip_forward` flag. Then we configure iptables to route traffic coming from host-only interface to another interface with internet access.

```console
modprobe iptable_nat
echo 1 > /proc/sys/net/ipv4/ip_forward
iptables -t nat -A POSTROUTING -o ens3 -j MASQUERADE
iptables -A FORWARD -i ens8 -j ACCEPT
```

Finally, we need to edit `/etc/resolv.conf` in order to use DNS. We can add any nameserver in this file.

```console
nameserver 8.8.8.8
```

Finally, client VM is able to connect to the internet via NAT.

![internet-working](https://user-images.githubusercontent.com/23631617/139917603-cb0f57c7-d9a0-4f52-86cb-6383a7ce8ee8.png)



###### Referenced from: https://how-to.fandom.com/wiki/How_to_set_up_a_NAT_router_on_a_Linux-based_computer

---
