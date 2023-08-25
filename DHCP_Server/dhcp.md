# Table Content
- [DHCP server](#install-and-configure-dhcp-server) 
- [DHCP client](#configure-dhcp-client)
# Install and configure DHCP server
## 1. Set a statict IP
- With `ipadm create-addr -T static -a 192.168.1.70/24 net0/addr` command you will can assign a static IP, it must be an available address on the network
- If the network interface has an assigned IP, with `ipadm delete-addr net0/addr` you can delete it
## 2. Install package
- Run the command `pkg install pkg:/service/network/dhcp/isc-dhcp` for install DHCP package
## 3. Create configuration file
- Create file with `touch /etc/inet/dhcpd4.conf`
- The file should look like this, you can use the `vi` editor
```
subnet 192.168.1.0 netmask 255.255.255.0 {
	interface net1;
	range 192.168.1.100 192.168.1.120;
	option routers 192.168.1.0;
	option broadcast-address 192.168.1.255;
}
```
## 4. Star DHCP server service
- Run `svcadm enable /network/dhcp/server:ipv4` command to start DNS server service
- Run `svcs -a | grep dhcp` command to confirm that the service started correctly

# Configure DHCP client
## 1. Check the network interfaces
- Use the commands `dladm show-phys` or `dladm show-phys` to check network interfaces an their status
![](/images/dhcp01.png)
- Use the command `ipadm show-addr` to check the assigned addresses
![](/images/dhcp02.png)
- Note: In this case I have 4 network interfaces (net0-3), net0 is the only one configured
## 2. Manage network interfaces
According to the state of the interface you want to configure you must:
- 2.1 Create the IP interface with the command `ipadm create -ip [interface]`
- 2.2 Register a DHCP address for the network interface using the command `ipadm create-addr -T dhcp [interface]/v4`
- Note: if the IP interface already exist with a IP address you should delete the existing IP address with `ipadm delete-addr [interface]/v4`

![](/images/dhcp03.png)
I am using the **net3** interface, as you can see after configuring it the **STATE** change to **up** and the DHCP server assigns the address **192.168.1.100**, which is in the range of the configuration file made in this [step](#3-create-configuration-file)