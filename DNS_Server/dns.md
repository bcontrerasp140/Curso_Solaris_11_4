# TABLE CONTENT
- [Install and Configure DNS Server](#install-and-configure-dns-server)
- [Configure DNS Client](#configure-dns-client)
# Install and Configure DNS Server
## 1. Set a statict IP
- With `ipadm create-addr -T static -a 192.168.1.70/24 net0/addr` command you will can assign a static IP, it must be an available address on the network
- If the network interface has an assigned IP, with `ipadm delete-addr net0/addr` you can delete it
## 2. Install package
- Run the command `pkg install service/network/dns/bind` for install DNS package
![](/images/dns01.png)
## 3. Create configuration file
- Create file with `touch /etc/named.conf`
- The file should look like this, you can use the `vi` editor
```
options {
        directory       "/etc/namedb/working";
        pid-file        "/var/run/named/pid";
        dump-file       "/var/dump/named_dump.db";
        statistics-file "/var/stats/named.stats";
        forwarders { 208.67.222.222; 208.67.220.220; };
};
zone "solariscourse" {
        type master;
        file "/etc/namedb/master/solariscourse.db";
};
zone "1.168.192.in-addr.arpa" {
        type master;
        file "/etc/namedb/master/1.168.192.db";
};
```
- Note: I will be use the domain name **solariscourse** and network **192.168.1.0**
## 4. Create directories for DNS
- This directories will be necessary to start the server
```
mkdir /var/dump
mkdir /var/stats
mkdir -p /var/run/namedb
mkdir -p /etc/namedb/master
mkdir -p /etc/namedb/working
```
## 5. Create forward lookup file
- Create file with `touch /etc/namedb/master/solariscourse.db`
- The file should look like this, you can use the `vi` editor
```
$TTL 3h
@       IN      SOA     solarisserver.solariscourse. root.solarisserver.solariscourse. (
        2013022744 ;serial (change after every update)
        3600 ;refresh (1 hour)
        3600 ;retry (1 hour)
        604800 ;expire (1 week)
        38400 ;minimum (1 day)
)
solariscourse.  IN      NS      solarisserver.solariscourse.
solarisserver   IN      A       192.168.1.70 ; 
manjaroclient   IN      A       192.168.1.69 ;
solarisclient   IN      A       192.168.1.67 ;
```
- Note: Remember that I will use the domain name **solariscourse**, **solarisserver** is the DNS server, **solarisclient** and **manjaroclient** are nodes in the network **192.168.1.0** with their IP
## 6. Create reverse forward lookup file
- Create file with `touch /etc/namedb/master/1.168.192.db`
- The file should look like this, you can use the `vi` editor
```
$TTL 3h
@       IN      SOA     solarisserver.solariscourse. root.solarisserver.solariscourse. (
        2013022744 ;
        3600 ;
        3600 ;
        604800 ;
        38400 ;)
)
        IN      NS      solarisserver.solariscourse.
70      IN      PTR     solarisserver.solariscourse. ;
69      IN      PTR     manjaroclient.solariscourse. ;
67      IN      PTR     solarisclient.solariscourse. ;
```
## 7. Verify DNS server settings
- To confirm that all is rigth run command `named-checkconf -z /etc/named.conf`, then this menssage will be displayed
![](/images/dns02.png)
otherwise check that the files from steps 2, 4 and 5 have the correct syntax
## 8. Star DNS server service
- Run `svcadm enable dns/server` command to start DNS server service
- Run `svcs dns/server` command to confirm that the service started correctly
![](/images/dns03.png)
# Configure DNS Client
At this point if you try to ping the node hostnames, you will not get a response, because it is necessary to specify a dns server that can resolve them.
![](/images/dns04.png)
## 1. Set IP for DNS server 
- Run the command `svccfg -s network/dns/client setprop config/nameserver = net_address: 192.168.1.70` to set the ip address of the DNS server
- Note: this is the IP you set in this [step](#1-set-a-statict-ip)
## 2. Specify the domain name
Run this commands to set the domain name
- `svcfg -s network/dns/client/ setprop config/domain = astring: solariscourse`
- `svccfg -s network/dns/client setprop config/search = astring: solariscourse`
- `svccfg -s network/dns/client refresh`
- Note: this is the domain name that you used in the server [configuration files](#3-create-configuration-file)
## 3. Update the name switch service 
The next commands will update the name service switch information in the SMF repository to use DNS.
- `svccfg -s name-service/switch setprop config/ipnodes = astring: '"files dns"'`
- `svccfg -s name-service/switch setprop config/host = astring: '"files dns"'`
- `svccfg -s name-service/switch refresh`
## 4. Check settings
- Run `svccfg -s network/dns/client listprop config` command that check that the DNS server information is correct
## 5. Start DNS client service
- Run `svcadm enable dns/client` command to start DNS server service
- Run `svcs dns/client` command to confirm that the service started correctly
![](/images/dns05.png)

Now if you try to ping the hostnames of the nodes in the network they will already be available
![](/images/dns06.png)
Using the `nslookup` command you can confirm that the reverse forward lookup also works
![](/images/dns07.png)