# How to upgrade your current release

### Update your package list

Run the apt update command to update the package list:
```shell
$ sudo apt udpdate
```

### Upgrade your upgradable packages:

This will upgrade packages that have newer version(s) to the latest version:
```shell
$ sudo apt upgrade
```

### Do a Full Upgrade

Some packages that can only be upgraded by removing some other package(s) will not be upgraded by a normal upgrade command.
You can use full-upgrade to install those if needed:
```shell
$ sudo apt full-upgrade
```

### Remove any package that is no longer needed.

If the upgrade / full-upgrade commands suggested there are packages that are no longer needed you can remove them with autoremove:
```shell
$ sudo apt autoremove
```

### Clean downloaded packages (optional)

If you want to free some space used by apt you can use the clean option:
```shell
$ sudo apt clean
```

**Note:** This procedure will not upgrade your OS to a newer release. There are ways to do it, by changing the source repositories to the most recent release. That procedure is not recommended by the Raspberry Pi Foundation, so it's better if you do a clean install instead.
