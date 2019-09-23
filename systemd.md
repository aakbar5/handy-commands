# Table of Contents
- [Systemd service](#service)
- [Systemd log](#log)

<a name="service"></a>
## Systemd service
- `systemctl status  foo.service` - Show status of the service
- `systemctl stop    foo.service` - Stop the service
- `systemctl disable foo.service` - Uninstall the service
- `systemctl -l --no-pager status foo.service` - Show log of the service
- `systemctl -l --no-pager list-unit-files --state=enabled` - Show the services which are in enabled state.
- `systemctl -l --no-pager list-unit-files --state=enabled,running` - Show the services which are in enabled or running state.

<a name="log"></a>
## Systemd log
- `journalclt --no-pager` - Show log without page limit
- `journalctl -xb --no-pager` - Show log without page limit
