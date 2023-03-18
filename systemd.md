# systemd

- [Service](#service)
- [Target](#target)
- [System](#system)
- [Sample service](#sample_service)
- [Sample target](#sample_target)
- [Sample socket](#sample_socket)

<a name="service"></a>
## Service
- `systemctl stop    foo.service` - Stop the service
- `systemctl disable foo.service` - Uninstall the service
- `systemctl status  foo.service` - Show status of the service
- `systemctl --failed` -- Show any failing services
- `systemctl restart foo.service` - Restart a service
- `systemctl enable foo.service` - Enable service to auto-run on system boot
- `systemctl -l --no-pager status foo.service` - Show status of the service
- `systemctl -l --no-pager list-unit-files --state=enabled` - Show the services which are in enabled state.
- `systemctl -l --no-pager list-unit-files --state=enabled,running` - Show the services which are in enabled or running state.
- `systemctl list-dependencies --type service` - Show services w.r.t dependencies
- To stop service from booting
	```
	systemctl stop foo.service
	systemctl disable foo.service
	```
- `systemctl daemon-reload` - Reload systemd
- `systemctl reset-failed` - Reset the failed state of all units

<a name="target"></a>
## Target
- `systemctl --no-pager list-units --type target` - Show all targets
- `ls -al /lib/systemd/system/runlevel*.target` - See mapping of runlevels to targets
- `systemctl get-default` - Show default target

<a name="system"></a>
## System
- `journalclt --no-pager` - Show log without page limit
- `journalctl -xb --no-pager` - Show log without page limit
- `systemctl -l --no-pager list-units --type service --all` - Show services which are loaded on system boot
- `systemd-analyze blame` - Show time consumed by each service
- `systemd-analyze plot > services_on_boot.svg` - Plot graph of the services loaded during system boot
- `systemd-analyze dot | dot -Tsvg > services_on_boot.svg` - Plot graph of the services loaded during system boot
- `systemd-analyze critical-chain` - Show services which are blocking boot process
- `systemd-analyze` - Show summary of the system boot
- `systemd-analyze time` - Show summary of the system boot

<a name="sample_service"></a>
## Sample service
- A unit configuration file contains information about a process.
- systemd will use to manage this process.
- This config file will be saved in `.service` file.
- A sample `foo.service`
```
[Unit]
Description=Hello world Application
Before=
After=multi-user.target
Before=something.service

[Service]
Type=idle
Environment=APP_OPTION=0
WorkingDirectory=/home/root
ExecStartPre=/bin/cp -f /home/root/resouce /home/root
ExecStart=/home/root/app

[Install]
WantedBy=multi-user.target
```
- In case you want to run your application after multiple target or service you can have

```
After=multi-user.target network.target xyz.service
```

<a name="sample_target"></a>
## Sample target
- A unit configuration file contains information about a group of processes.
- This config file will be saved in `.target` file.
- A sample `foo.target`
```
#  This file is part of systemd.
#
#  systemd is free software; you can redistribute it and/or modify it
#  under the terms of the GNU Lesser General Public License as published by
#  the Free Software Foundation; either version 2.1 of the License, or
#  (at your option) any later version.

[Unit]
Description=Graphical Interface
Documentation=man:systemd.special(7)
Requires=multi-user.target
After=multi-user.target
Conflicts=rescue.target
Wants=display-manager.service
AllowIsolate=yes

[Install]
Alias=default.target
```

<a name="sample_socket"></a>
## Sample socket
- A unit configuration file contains information about socket based activation. 
- This config file will be saved in `.socket` file.
