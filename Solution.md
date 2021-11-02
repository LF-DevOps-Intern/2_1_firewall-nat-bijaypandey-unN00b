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

> Note: Configure the first vm as a router, so make the LAN interfaces in the first vm as gateway to the LAN network. And in the second vm configure the gateway to the ip of the first vm LAN ip.

---
