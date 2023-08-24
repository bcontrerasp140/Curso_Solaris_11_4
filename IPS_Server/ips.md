# Image packaging system(IPS)
# Table Content
- [Install and Configure IPS Server](#install-and-configure-ips-server)
- [Configure IPS Client](#configure-ips-client)

# Install and Configure IPS Server
## 1. Set a statict IP
- With `ipadm create-addr -T static -a [IP/[mask] [interface]/addr` command you will can assign a static IP, it must be an available address on the network
- If the network interface has an assigned IP, with `ipadm delete-addr [interface]/addr` you can delete it
## 2. Download the repository files
- Go to [Solaris 11 Downloads Create a Local Repository](https://www.oracle.com/solaris/solaris11/downloads/local-repository-downloads.html) and download all the repository files, you must have an oracle account
![](/images/ips01.png)
Note: It is important that **Repository Assembly Script** be saved as **install-repo.ksh** and **SHA256 Digest** as **sol-11_4-repo_digest.txt**.
- Create the directory **/var/repository/s11u4** and move the downloads, with commands `mkdir -p /var/repository/s11u4` and `mv [downloads_path]/* /var/repository/s11u4`
## 3. Prepare the repository files
- Use `ls -lrt` to check file permissions to root user and root group, the output should look like this
```
-rw-r--r-- 1 root root 1814619737 Jan 11 10:03 sol-11_4-repo_2of5.zip
-rw-r--r-- 1 root root 1968246581 Jan 11 10:13 sol-11_4-repo_1of5.zip
-rw-r--r-- 1 root root 2132702935 Jan 11 10:38 sol-11_4-repo_4of5.zip
-rw-r--r-- 1 root root 1772147401 Jan 11 10:52 sol-11_4-repo_3of5.zip
-rw-r--r-- 1 root root 1939943920 Jan 11 11:01 sol-11_4-repo_5of5.zip
-rw-r--r-- 1 root root 12262 Jan 11 11:13 install-repo.ksh
-rw-r--r-- 1 root root 495 Jan 11 11:13 sol-11_4-repo_digest.txt
```
- If necessary to get the adove output use the commands `chown root *`, `chgrp root *` and `chmod -R 644 *`
- With `chmod 755 install-repo.ksh` give permission to execute the script **install-repo.ksh**
## 3. Setting up repository
- With `mkdir` create the destination directory to hold repository, I will use the directoy **/repo** 
- To start installation run the script `./install-repo.ksh -d /repo`
![](/images/ips02.png)
- Use command `ls -lrt` to check the destination directory, if it completed successfully the output should look like this:
```
-rw-r--r--   1 root     root         347 Jun 25 05:25 pkg5.repository
-rwxr-xr-x   1 root     root        5970 Jun 25 10:15 README-repo-iso.txt
-rw-r--r--   1 root     root        1625 Jun 25 10:15 NOTICES
-rw-r--r--   1 root     root        3246 Jun 25 10:15 COPYRIGHT
drwxr-xr-x   3 root     root           3 Aug 10 20:41 publisher

```
## 4. Set the repository locally
If you run the command `pkg publisher` you can see that **pkg.oracle.com** is currently used as the repository, this will not be necessary because it already have the local repository
![](/images/ips03.png)
- Use the command `pkg set-publisher -G '*' -M '*' -g /repo solaris` to publish the repository locally, **/repo** is my destination directory
![](/images/ips04.png)
## 5. Publish Solaris 11 IPS repository
- `svccfg -s application/pkg/server setprop pkg/inst_root=/repo` make the system repository service is pointing to the local repo and use `svcprop -p pkg/inst_root application/pkg/server` to confirm result 
- `svccfg -s application/pkg/server setprop pkg/readonly=true` enable read-only for requests to the server and use `svcprop -p pkg/readonly application/pkg/server` to confirm result 
- `svccfg -s application/pkg/server setprop pkg/port=8080` set the server port as 8080 and `svcprop -p pkg/port application/pkg/server` to confirm result
## 6. Start PKG Server Service
- Run `svcadm enable pkg/server` command to start DNS server service
- Run `svcs application/pkg/server` command to verify the service is online
![](/images/ips05.png)

# Configure IPS Client
If you run the command `pkg publisher` you can see that **pkg.oracle.com** is currently used as the repository, this will not be necessary because the IPS server already exists on the network
![](/images/ips06.png)
- Use the command `pkg set-publisher -G '*' -M '*' -g http://[SERVER-IP]:8080/ solaris` to have access to the IPS Server
![](/images/ips07.png), if you followed the dns server tutorial you can use the hostname instead of the **SERVER-IP**