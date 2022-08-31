# Lesson 1 - Intro and virtualization 


**Virtualization** - In computing, virtualization is the act of creating a virtual (rather than actual) version of something at the same abstraction level, including virtual computer hardware platforms, storage devices, and computer network resources.



![title](images/Capture1.PNG)
![title](images/Capture2.PNG)
![title](images/Capture3.PNG)
![title](images/Capture4.PNG)
![title](images/Capture5.PNG)
![title](images/Capture6.PNG)
![title](images/Capture7.PNG)
![title](images/Capture8.PNG)
![title](images/Capture9.PNG)
![title](images/Capture10.PNG)
![title](images/Capture11.PNG)
![title](images/Capture12.PNG)
![title](images/Capture13.PNG)
![title](images/Capture14.PNG)
![title](images/Capture15.PNG)
![title](images/Capture16.PNG)
![title](images/Capture17.PNG)
![title](images/Capture18.PNG)
![title](images/Capture19.PNG)
![title](images/Capture20.PNG)
![title](images/Capture21.PNG)
![title](images/Capture22.PNG)
![title](images/Capture23.PNG)
![title](images/Capture24.PNG)
![title](images/Capture25.PNG)
![title](images/Capture26.PNG)
![title](images/Capture27.PNG)
![title](images/Capture28.PNG)
![title](images/Capture29.PNG)
![title](images/Capture30.PNG)
![title](images/Capture31.PNG)
![title](images/Capture32.PNG)
![title](images/Capture33.PNG)
![title](images/Capture34.PNG)



# Demo
1. Download Rocky from https://rockylinux.org/download/
2. Go to Proxmox and upload it

![title](images/Capture35.PNG)

3. Right click in the node -> create VM

![title](images/Capture36.PNG)
![title](images/Capture37.PNG)
![title](images/Capture38.PNG)


Storage dropdown will have either: local-zfs or local-lvm options. Looks like it depends on how the proxmox was installed. On *HardDisc Options Screen* we use ext4 for lvm or we used zfs (RAID0) - for zfs.

![title](images/Capture39.PNG)
![title](images/Capture40.PNG)
![title](images/Capture41.PNG)
![title](images/Capture42.PNG)
![title](images/Capture43.PNG)


After that do Rocky installation
1. Set English and time and date for Chisinau
2. Software Selection -> Minimal Install
3. Installation Destination: Select "Custom" -> Done
	Select "LVM thin Provisioning" -> "Click here to create them automatically"
4. Delete "swap", extend root with some big number and click "Update Settings" 
5. For "root" - modify volume group to be alphabetically after boot, update "rl" for example to "wrl" -> Save
6. For "boot" - select ext4 and click Update; for "root" - select "LVM thin Provisioning" and ext4 - and click Update -> Click Done
7. Network & Host name -> set custom name like "rocky" and enable Ethernet
8. Create root password
9. Create user and check "Make this user admin" 
10. Wait to finish installation



# Steps inside Rocky

```sudo vi /etc/sudoers``` - and uncomment the following line (it will allow users with wheel group to use sudo without entering password each time)

![title](images/Capture44.PNG)


```groups andrey``` - to check user groups
```usermod -aG wheel andrey``` - can run from root user to add **wheel** group to **andrey**


**Install QEMU and enable it**
```sudo dnf install -y qemu-guest-agent```

``` sudo systemctl enable --now qemu-guest-agent```