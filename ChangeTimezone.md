# Changing the Timezone

## The Easy Way:

### Run Raspberry Pi Configuration Tool
```
# After executing the following command, go to Locatisation Options -> Change Timezone
sudo raspi-config
```

## Using Terminal:
Taken From: https://linuxize.com/post/how-to-set-or-change-timezone-on-debian-10

### If your OS is recent and has timedatectl
```
# Check Timezone
timedatectl

# List Timezones
timedatectl list-timezones

# Set New Timezone using timedatectl
sudo timedatectl set-timezone America/Santo_Domingo
```

### If your OS is too old
```
# Check Timezone
ls -l /etc/localtime

# List Timezones for America region for example
ls -l /usr/share/zoneinfo/America

# Simlink the zoneinfo file manually
sudo ln -sf /usr/share/zoneinfo/America/New_York /etc/localtime
```
