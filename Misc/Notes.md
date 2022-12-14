## How to add space to Proxmox in Hyper-V

Prerequisite:
	First go Hyper-V -> Settings -> SCSI controller -> Hard Drive and expand hard drive


1. Show data related to disks (you will see some new unallocated space here after prerequisite)

```fdisk -l /dev/sda```


2. Extend sda3 disk partition

```growpart /dev/sda 3```

3. Extend sda3 physical volume
```pvresize /dev/sda3``` 



4. Add free space to pools - root and data

```lvextend -l +10%FREE /dev/pve/root```

```lvextend -l +100%FREE /dev/pve/data```



5. Resize root filesystem  

```resize2fs /dev/pve/root```

