---
title: systemd
tags:
  - Linux
date: 2025-06-23 22:13
---

A good [blog](https://wiki.archlinux.org/index.php/Systemd) for systemd.

Some notes:

* You can use all of the following systemctl commands with the `-H user@host` switch to control a systemd instance on a remote machine. This will use SSH to connect to the remote systemd instance.
* `systemctl status` show status
* `systemctl` or `systemctl list-units` to list running units.
* `systemctl --failed` to show failed.
* The available unit files can be seen in `/usr/lib/systemd/system/` and `/etc/systemd/system/` (the latter takes precedence). List installed unit files with: `systemctl list-unit-files`
* `systemctl status pid` Show the cgroup slice, memory and parent for a PID.
* `systemctl start unit` Start a unit immediately
* `systemctl stop unit` stop a unit
* `systemctl restart unit` restart a unit
* `systemctl reload unit` Ask a unit to reload its configuration
* `systemctl status unit` Show the status of a unit, including whether it is running or not

Add `--user` to `systemctl` command will use user path ` ~/.config/systemd/user`
## write unit files
The syntax for unit file is similar to windows ini file. There are several sections for a unit file. we can name it `somename.service`. an example look like:

```
[Unit]
Description=Shadowsocks R Server Service
After=docker.service
Requires=docker.service

[Service]
ExecStart=/usr/bin/python server.py -c ../user-config.json
```
 To notice, we need to use the absolute path in `ExecStart`. Some other in `Service` section:

 | Name    | Description    |
 | --- | --- |
 |   ExecStartPre  |   Commands that will run before ExecStart.  |
 |   ExecStart  |  Main commands to run for this unit.   |
 |   ExecStartPost  | Commands that will run after all ExecStart commands have completed.    |
 |   ExecReload  |   Commands that will run when this unit is reloaded via systemctl reload foo.service  |
 |  ExecStop   |  Commands that will run when this unit is considered failed or if it is stopped via systemctl stop foo.service   |
 |  ExecStopPost   |   Commands that will run after ExecStop has completed.  |
 |   RestartSec  |   The amount of time to sleep before restarting a service. Useful to prevent your failed service from attempting to restart itself every 100ms.  |

`After=docker.service` and `Requires=docker.service` means this unit will only start after `docker.service` is active. You can define as many of these as you want.

we can also add a `Install` section, `WantedBy=` is the target that this unit is a part of.

There is a `Timer` section to do some timer work. Some settings can be used.

| Setting             | Meaning                                                                                                                                                                                                                                                                                                                                                                                         |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| OnActiveSec=        | Defines a timer relative to the moment the timer unit itself is activated.                                                                                                                                                                                                                                                                                                                      |
| OnBootSec=          | Defines a timer relative to when the machine was booted up. In containers, for the system manager instance, this is mapped to OnStartupSec=, making both equivalent.                                                                                                                                                                                                                            |
| OnStartupSec=       | Defines a timer relative to when the service manager was first started. For system timer units this is very similar to OnBootSec= as the system service manager is generally started very early at boot. It's primarily useful when configured in units running in the per-user service manager, as the user service manager is generally started on first login only, not already during boot. |
| OnUnitActiveSec=    | Defines a timer relative to when the unit the timer unit is activating was last activated.                                                                                                                                                                                                                                                                                                      |
| OnUnitInactiveSec=  | Defines a timer relative to when the unit the timer unit is activating was last deactivated.                                                                                                                                                                                                                                                                                                    |
| OnCalendar=         | Defines realtime (i.e. wallclock) timers with calendar event expressions.                                                                                                                                                                                                                                                                                                                       |
| AccuracySec=        | Specify the accuracy the timer shall elapse with. Defaults to 1min.                                                                                                                                                                                                                                                                                                                             |
| RandomizedDelaySec= | Delay the timer by a randomly selected, evenly distributed amount of time between 0 and the specified time value. Defaults to 0, indicating that no randomized delay shall be applied.                                                                                                                                                                                                          |
| Unit=               | The unit to activate when this timer elapses. The argument is a unit name, whose suffix is not ".timer".                                                                                                                                                                                                                                                                                        |
| Persistent=         | Takes a boolean argument. If true, the time when the service unit was last triggered is stored on disk.                                                                                                                                                                                                                                                                                         |

