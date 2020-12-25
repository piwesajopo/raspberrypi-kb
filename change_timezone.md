# Chaging Timezone

## Easy Way:

### Locatisation Options -> Change Timezone
```
$ sudo raspi-config
```

## Terminal Way:
(Taken From:)[https://linuxize.com/post/how-to-set-or-change-timezone-on-debian-10]

```
# Check Timezone
$ timedatectl

# List Timezones for America region
$ ls -l /usr/share/zoneinfo/America

# Simlink the file
sudo ln -sf /usr/share/zoneinfo/America/Monterrey /etc/localtime
```
