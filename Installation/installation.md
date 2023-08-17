# Table of Contents
- [OVM Installation](#oracle-virtual-machine-installation)
- [Native Installation](#native-installation)
- [Enable Desktop GUI](#enable-desktop-gui)


# Oracle Virtual Machine Installation
## 1. Download Solaris 11 ISO image
- Go to [Solaris 11 Downloads](https://www.oracle.com/solaris/solaris11/downloads/solaris11-install-downloads.html#license-lightbox) and select **x86 Text Installer**
- Note: You need to have an account into Oracle to be download ISO
## 2. Create a new VM 
Into VirtualBox Manager select `New` and:
### 2.1 VM Name and Operating System
- Set virtual machine name for example **Solaris_11_4**
- Select the installation directory for the virtual machine
- Go to path of the downloaded an select the Solaris ISO Image
- Select the type and version OS, **Solaris** and **Oracle Solaris 11 (64bit)** in this case
![](/images/installation01.png)
- Click `Next`
### 2.2 VM Hardware
- Assign the virtual machine RAM and CPU, the minimal requieremnt for Solaris 11.4 are **1 Proccesor and 1.5 GB**
![](/images/installation02.png)
- Click `Next`
### 2.3 Vitual Hard Disk
- Create a virtual hard disk the size you want but the minimal disk size for solaris 11.4 are **32 GB**
![](/images/installation03.png)
- Click `Next`
### 2.4 VM Summary
- Check that the virtual machine configuration is correct
![](/images/installation04.png)
- Click `Finish`
### 2.5 Recommended network and boot settings
- On the virtual machine go to the `Settings`
- Select `System`, on the Boot Order disable `Floppy` and move `Hard Disk` as first option
- Select `Network`, enabled the `Adapter 2` and select **Bridged Addapter**, if you want it to communicate with other vms or your host
- Click `Ok`
## 3. Start installation Solaris 11.4
Start the machine to begin the installation.
### 3.1 Set Keyboard Layout and Installation Language
- Use the **1-9** keys to select option and press **Enter** to confirm
![](/images/installation05.png)
### 3.2 Installation menu
- Press **1** and **Enter** keys to begin installation
![](/images/installation06.png)
- Hints on how to navigate the installer will be displayed, then press **F2** key
### 3.3 Discovery Selection
- Select **Local Disk** and press **F2** key to continue
![](/images/installation07.png)
### 3.4 Disks
- Choose your installation disk and press **F2** key to continue
![](/images/installation08.png)
### 3.5 Partition
- Select how to the disk partition will be, I will use **entire disk**
![](/images/installation09.png)
### 3.6 System Identity
- Set the hostname that you prefer and press **F2** key to continue
![](/images/installation10.png)
### 3.7 Network Configuration
- Select a network interface to configure, I will choose the first interface, which was NAT type.
![](/images/installation11.png)
- Select **DHCP** for automatic IP assignment or if you want a static IP, select **Static**
![](/images/installation12.png)
### 3.8 Regions, Language, Territory, Date and Time, Keyboard
- According to your local territory choose the region, language and territory, for example I will set **UTC/CMT, English and UTF-8**
- If the date and time are outdated, you will need to set them manually
- Also, choose your Keyboard layout.
### 3.9 Users
- Set the root password and create a user account, it is recommended that the password have uppercase letters, lowercase letters and digits
![](/images/installation13.png)
- Press **F2** key to continue and optionally you can register for support.
### 3.10 Installation Summary
- Check and confirm the installation summary and press **F2** key to proceed with installation
![](/images/installation14.png)
### 3.11 Installation Complete
- if all is correct, a progress bar will be displayed to monitor the progress
![](/images/installation15.png)
- If all is good, then it should be successfully finish and finally press **F8** key to reebot the system
- Note: after that the system shutdown, be sure to remove boot ISO.

# Native Installation
## 1. Download Solaris 11 ISO image
Go to [Solaris 11 Downloads](https://www.oracle.com/solaris/solaris11/downloads/solaris11-install-downloads.html#license-lightbox) and select **x86 Text Installer**

# Enable Desktop GUI

## Título
### Subtítulo
Este es un ejemplo de texto que da entrada a una lista genérica de elementos:
- Elemento 1
- Elemento 2
- Elemento 3
Este es un ejemplo de texto que da entrada a una lista numerada:
1. Elemento 1
2. Elemento 2
3. Elemento 3
Al texto en Markdown puedes añadirle formato como **negrita** o *cursiva* de una manera muy sencilla.

# Encabezado 1
## Encabezado 2
### Encabezado 3
#### Encabezado 4
##### Encabezado 5
###### Encabezado 6

# Table of Contents
1. [Example](#example)
2. [Example2](#example2)
3. [Third Example](#third-example)
4. [Fourth Example](#fourth-examplehttpwwwfourthexamplecom)


## Example
## Example2
## Third Example
## [Fourth Example](http://www.fourthexample.com)