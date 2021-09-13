# Adding a *Lock* menu entry

By default the Raspberry Pi `Applications Menu` has a Logout Menu with 3 options: `Shudown`, `Reboot`, `Logout`.
This menu is seldomly used, and the option that you would use the most in a desktop environment is missing: A `Lock Screen` menu option.

Luckily adding this option to the menu is really easy. Add these lines to the file `/etc/xdg/lxpanel/LXDE-pi/panels/panel`:

```
item {
      name=Lock…
      image=gnome-lockscreen
      action=/usr/bin/dm-tool lock
    }
```

The previous `item { }` section would be located at the end of the `Plugin { type=menu Config { } }` as follows:
```
Plugin {
  type=menu
  Config {
    padding=4
    image=start-here
    system {
    }
    separator {
    }
    item {
      image=system-run
      command=run
    }
    separator {
    }
    item {
      image=system-shutdown
      command=logout
    }
    item {
      name=Lock\u2026
      image=gnome-lockscreen
      action=/usr/bin/dm-tool lock
    }
  }
}

```
