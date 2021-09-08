# About Host Keys

One of the best things about the Raspberry Pi is that you can have different set of Boot Cards and easily swap them. For example:
* Raspberry Pi OS Desktop
* Raspberry Pi Lite
* LibreELEC
* RetroPie Emulator

However, when you connect to your raspberry Pi you usually do it using the IP. This may pose a problem when using different OS installations. To work arround this, you have several optiions:
- **(a)** Configure different static IPs for each installation.
- **(b)** Maintain a single set of host keys on your installations.
- **(c)** Use different hostnames for each installation but point them to the same IP.

The most safe of these options is **(a)** since you will be sure you are login to the correct device each time since you will use a unique IP. This is the same for **(c)** but you would need to configure the hostname tables on each client machine.

I'm kind of lazy and decided to use **(b)** for my installations with SSH service enabled. So that's what we'll be discussing here.


## Sharing the Host Keys

Your host keys are located at ***/etc/ssh***. So you could just copy them from another system. 

**If you are using Raspberry Pi OS Desktop, first insert the card and it should mount automatically as /media/pi/rootfs**
```
# Backup Your hostkeys (you never know)
[[ ! -d ~/hostkeys_bk/myraspberry_hostkeys ]] && mkdir -p ~/hostkeys_bk/myraspberry_hostkeys
[[ ! -d ~/hostkeys_bk/myotherpi_hostkeys ]] && mkdir ~/hostkeys_bk/myotherpi_hostkeys
sudo cp /etc/ssh/ssh_host_*key* ~/hostkeys_bk/myraspberry_hostkeys/
sudo cp /media/pi/rootfs/etc/ssh/ssh_host_*key* ~/hostkeys_bk/myotherpi_hostkeys

# Replace Hostkeys on target MicroSD Card
sudo cp /etc/ssh/ssh_host_*key* /media/pi/rootfs/etc/ssh/
```

**If you are using Raspberry Pi Lite or other OS, first insert the card and run the following commands:**
```
# Mount the OS Filesystem
[[ ! -d /mnt/transfer ]] && mkdir /mnt/transfer
sudo chown root:root /mnt/transfer
sudo mount /dev/sda2 /mnt/transfer

# Backup Your hostkeys (you never know)
[[ ! -d ~/hostkeys_bk/myraspberry_hostkeys ]] && mkdir -p ~/hostkeys_bk/myraspberry_hostkeys
[[ ! -d ~/hostkeys_bk/myotherpi_hostkeys ]] && mkdir ~/hostkeys_bk/myotherpi_hostkeys
sudo cp /etc/ssh/ssh_host_*key* ~/hostkeys_bk/myraspberry_hostkeys/
sudo cp /mnt/transfer/etc/ssh/ssh_host_*key* ~/hostkeys_bk/myotherpi_hostkeys

# Replace Hostkeys on target MicroSD Card
sudo cp /etc/ssh/ssh_host_*key* /mnt/transfer/etc/ssh/
```
