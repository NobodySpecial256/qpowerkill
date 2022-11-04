# qpowerkill

A script to shutdown a system on power events. This is useful, for example, to tether a system to a specific physical location while in public. This way, any attempt to move the system elsewhere (for example, for forensic analysis at a separate location) will result in immediate poweroff

**WARNING:** This script has no way to differentiate being unplugged from an innocent loss of power. An unexpected temporary loss of power can result in data loss. Therefore, it's recommended to avoid using destructive panic actions. The script syncs data to disk by default, but cannot save your documents for you. Unsaved data *will* be lost

Installation is as follows:

- Copy `qpowerkill`, `qpowerkill.service`, and `pk` into dom0
- Copy/move `qpowerkill` into `/usr/bin` and mark it as executable
- Copy/move `qpowerkill.service` into `/etc/systemd/system`
- Copy/move `pk` into `/usr/bin` and mark it executable
  - `pk` is a helper script to disable/re-enable qpowerkill without typing a long systemctl command
- Run `systemctl enable qpowerkill` to make qpowerkill start at boot
- After qpowerkill is set to start at boot, run `systemctl start qpowerkill` to start qpowerkill immediately

This will enable the service at boot, and start the service
To attach or detatch your charger/power adapter without causing dom0 to power off, temporarily stop the service with `systemctl stop qpowerkill` or use the provided helper script `pk`, then restart it after the power adapter is attached or removed

### Using the `pk` helper script

`pk` is effectively just an alias for `systemctl <start/stop/restart/status> qpowerkill`. The major benefit is it's shorter

- To start qpowerkill, run `pk start`
- To stop qpowerkill, run `pk stop`
- To check if qpowerkill is running, run `pk status`
- To view qpowerkill's logs, run `pk log`
- To delete qpowerkill's logs, run `pk rm-log`
- You can also queue multiple commands. For example, `pk start status` will start qpowerkill, then show the status to make sure it's actually running

### How to copy files to dom0

The Qubes official documentation has information about copying files to dom0: https://www.qubes-os.org/doc/how-to-copy-from-dom0/#copying-to-dom0

For the best security, you should download this into a disposable VM, to prevent a compromised qube from tampering with the data locally
