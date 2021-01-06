# How to use fdisk to manage partitions

## Note: Use this tool with caution it may damage your OS disk

### Listing your disks and partitions:

To list your disks and partitions use this command:
```
$ sudo fdisk -l
```

Your non-os drives will appear with the sda designation. 

Like this:
```
Disk /dev/sda: 29.8 GiB, 32010928128 bytes, 62521344 sectors
Disk model: uSD UHS2 RDR    
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 09F7E3CF-DE92-4225-AB7D-6895CDB1CF64
```

Your partitions will be listed as follows:
```
Device      Start      End  Sectors  Size Type
/dev/sda1      40   409639   409600  200M EFI System
/dev/sda2  409640 62259159 61849520 29.5G Apple HFS/HFS+
```

Let's see if any of the partitions is mounted:
```
$ df -k | grep sda
/dev/sda2       30924760   113368  30811392   1% /media/pi/Untitled
```

As we can see /dev/sda2 is mounted. Let's unmount this partition:
```
$ umount /dev/sda2
```

### Erasing existing partitions 

This card was formatted on a Mac, it's read-only, we need to erase it.

For this we launch an interactive fdisk session, you need to run fdisk and specify the disk you will work with:
```
$ sudo fdisk /dev/sda
```

You can use the ***p*** command to check the current disk information:
```
Command (m for help): p
Disk /dev/sda: 29.8 GiB, 32010928128 bytes, 62521344 sectors
Disk model: uSD UHS2 RDR    
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 09F7E3CF-DE92-4225-AB7D-6895CDB1CF64

Device      Start      End  Sectors  Size Type
/dev/sda1      40   409639   409600  200M EFI System
/dev/sda2  409640 62259159 61849520 29.5G Apple HFS/HFS+
```

Now delete the sda2 partition:
```
Command (m for help): d
Partition number (1,2, default 2): 2

Partition 2 has been deleted.
```

And then delete sda1 (the only one left so, you won't be asked which one):
```
Command (m for help): d
Selected partition 1
Partition 1 has been deleted.
```

Now, let's check the disk current disk state:
```
Command (m for help): p
Disk /dev/sda: 29.8 GiB, 32010928128 bytes, 62521344 sectors
Disk model: uSD UHS2 RDR    
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 09F7E3CF-DE92-4225-AB7D-6895CDB1CF64
```

Now, let's make the changes permannent (make sure you are not accessing any folder inside the disk or you may see some errors):
```
Command (m for help): w
The partition table has been altered.
Failed to remove partition 2 from system: Device or resource busy

The kernel still uses the old partitions. The new table will be used at the next reboot. 
Syncing disks.
```

*Don't worry if you see an error like the one in this example. Eject the Drive and re-insert to make sure your kernel releases the old partition. The reason for the last error was that the filesystem was still mounted (I intentionally didn't run the umount /dev/sda2 command).*


### Creating a New Partition:

After inserting your disk again, let's make sure everything is as expected:
```
$ sudo fdisk /dev/sda

Welcome to fdisk (util-linux 2.33.1).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.


Command (m for help): p
Disk /dev/sda: 29.8 GiB, 32010928128 bytes, 62521344 sectors
Disk model: uSD UHS2 RDR    
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 09F7E3CF-DE92-4225-AB7D-6895CDB1CF64
```

We have a gpt partition table but it's best to re-create it. And maybe your disk has another type like dos. So let's make sure it's now gpt:
```
Command (m for help): g
Created a new GPT disklabel (GUID: 96CEFF88-7D30-F149-BEF6-EB35551DA66D).
```

In this case the only thing that changed was the disk identifier:
```
Command (m for help): p
Disk /dev/sda: 29.8 GiB, 32010928128 bytes, 62521344 sectors
Disk model: uSD UHS2 RDR    
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 96CEFF88-7D30-F149-BEF6-EB35551DA66D
```

Now let's make a new partition and use the default values so we use the entire disk:
```
Command (m for help): n
Partition number (1-128, default 1): 
First sector (2048-62521310, default 2048): 
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-62521310, default 62521310): 

Created a new partition 1 of type 'Linux filesystem' and of size 29.8 GiB.
```

The new partition looks as follows:
```
Command (m for help): p
Disk /dev/sda: 29.8 GiB, 32010928128 bytes, 62521344 sectors
Disk model: uSD UHS2 RDR    
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 96CEFF88-7D30-F149-BEF6-EB35551DA66D

Device     Start      End  Sectors  Size Type
/dev/sda1   2048 62521310 62519263 29.8G Linux filesystem
```

Write and exit:
```
Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.
```

Now we can format the disk and specify the label *NASHDD* should be used for it:
```
$ sudo mkfs.ext4 /dev/sda1 -L NASHDD
mke2fs 1.44.5 (15-Dec-2018)
Creating filesystem with 7814907 4k blocks and 1954064 inodes
Filesystem UUID: d8a489ee-daef-43d0-83b0-7a9ad9f2a877
Superblock backups stored on blocks: 
	32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208, 
	4096000

Allocating group tables: done                            
Writing inode tables: done                            
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done
```

Disconnect the disk and reconnect it again then check that the filesystem was automatically mounted by the OS:
```
$ df -k | grep sda
/dev/sda1       30637908    45080  29013464   1% /media/pi/NASHDD
```
