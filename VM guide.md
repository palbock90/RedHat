Two virtual machines with Red Hat Enterprise Linux (Developer edition of the server image) were set up in a local test environment 
using VirtualBox. It has later been discovered that virtual machines for development at Telenor uses CentOS, which would be more 
convenient because it does not require account creation. Test and production servers at Telenor uses Red Hat Enterprise Linux.

# Virtual machine setup
**1.** Download either Red Hat Enterprise Linux (RHEL) or CentOS. Create two virtual machines with VirtualBox and install the image on both. 
It is also possible to clone the first image and change MAC-addresses afterwards. 
Be sure to give your VM enough memory (>4GB) as it will kill some processes if it does not have enough.

*NOTE: If using RHEL, perform step 2 and 3 first.*  
Enable repos after installing the image. Only necessary for Red Hat Enterprise Linux.  
`subscription-manager register --username <username> --password <password> --auto-attach`

**2.** Enable two network interfaces in VirtualBox settings for each virtual machine, NAT and Host-only Adapter. This will provide internet access to the virtual machines via NAT and direct communication without port forwarding via Host-only Adapter from the host machine.

**3.** Use the following configurations for the network interfaces on each machine to enable internet access and to enable access from 
host machine to the virtual machines via static ip-addresses.

**/etc/sysconfig/network-scripts/ifcfg-enp0s3**
```
TYPE="Ethernet"
BOOTPROTO="dhcp"
DEFROUTE="yes"
NAME="enp0s3"
DEVICE="enp0s3"
ONBOOT="yes"
```

Increment the following IPADDR for the second virtual machine.  
**/etc/sysconfig/network-scripts/ifcfg-enp0s8**
```
DEVICE="enp0s8"
BOOTPROTO="static"
NM_CONTROLLER="yes"
ONBOOT="yes"
TYPE="Ethernet"
IPADDR="192.168.56.2"
NETMASK="255.255.255.0"
```

**4.** Install Java 8 and 11, git and Maven.

**5.** Test the installations by cloning the following git repository.

```
git clone https://github.com/adrianhj88/test-app
cd test-app
mvn spring-boot:run
```
