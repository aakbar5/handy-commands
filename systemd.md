# Table of Contents
- [Systemd service](#service)
- [Systemd log](#log)

<a name="service"></a>
# Systemd service
- `systemctl status  foo.service` - Show status of the service
- `systemctl stop    foo.service` - Stop the service
- `systemctl disable foo.service` - Uninstall the service

<a name="log"></a>
# Systemd log
- `journalclt --no-pager` - Show log without page limit
- `journalctl -xb --no-pager` - Show log without page limit
