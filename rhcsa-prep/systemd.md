# Systemd
## Definition

- *Systemd* is a system and service manager, responsible for booting, starting and stopping services, managing processes and handling system shutdowns. 
- Most prevalent units:
    - **Service**
    - **Socket**
    - **Target**
    - **Mount**
- `systemctl -t help` - displays a full list of units.
- `systemctl -t <unit>` - displays all entries of a unit type.

## System unit locations
- **/usr/lib/systemd/system**: contains default unit files that have been installed from RPM packages. Never edit these files directly.
- **/etc/systemd/system**: contains custom unit files.
- **/run/systemd/system**: contains automatically generated unit files.
The last location has the highest importance, since it overwrites any setting defined elsewhere. 

## Systemd units

### Service units
- Used to start processes.
- Service unit files consist of:
    - [Unit]: describes the unit and defines dependencies. Also contains **After** and **Before** statements, which indicite if the unit should be started before specified units, or after.
    - [Service]: describes how to start and stop the service and request status installation. `man 5 systemd.service` for more details.
    - [Install] indicates in which target this unit has to be started. Units that don't have this section cannot be started automatically.

### Mount units
- Specifies how a file system can be mounted on a specific directory.
- Main configuration options:
    - [Unit]: **conflicts** statement is used to list units that cannot be used together with this unit.
    - [Mount]: defines where the mount has to be performed.

### Socket units
- A *socket* creates a method for applications to communicate with one another.
- A socket may be defined as a file but also as a port on which Systemd will be listening for incoming connections. Services will only start if a connection is coming in on the socket that is specified. Every socket needs a corresponding service file.
- Options *ListenStream* and *ListenDatagram* define TCP and UDP ports respectively.

### Target units
- A *target* unit is a group of units. Some targets define the state a server should be started in, others are just a group of services that make it easy to manage all the units required to get specific functionality.
- Targets can have dependencies on other targets.
- **basic.target** defines all the units that should always be started.
- `systemctl list-dependencies <unit>` to overview any existing dependencies.
- When enabling a unit, the [Install] section of that unit is considered to determine to which target the unit should be added. When you add a unit to a target, a symbolic link is created in the target directory in **/etc/systemd/system**.

## Managing units
- `systemctl status <unit>` - check unit status.
- `systemctl start <unit>` - start a unit.
- `systemctl stop <unit>` - stop a unit.
- `systemctl enable <unit>` - create a symlink in the wants directory for the multiuser target for the service to automatically start on boot.
- `systemctl disable <unit>` - unit won't be started on boot.
- `systemctl (un)mask <unit>` - creates a symlink from /etc/systemd/system to /dev/null, the the service can never be started.

Diffrent statuses:
    - **Loaded**: unit file has been processed and the unit is active.
    - **Active(running)**: unit is running with one or more active processes.
    - **Active(exited)**: unit has successfully completed a one-time run.
    - **Active(waiting)**: unit is running and waiting for an event.
    - **Inactive(dead)**: unit is not running.
    - **Enabled**: unit will be started at boot time.
    - **Disabled**: unit will not be started at boot time.
    - **Static**: unit cannot be enabled but may be started by another unit automatically.

## Unit overview commands
- `systemctl -t service` - shows only active service units.
- `systemctl list-units -t service` - same as previous command.
- `systemctl list-units -t service --all` - shows active and inactive service units.
- `systemctl --failed -t service` - shows all services that have failed.
- `systemctl status -l <service>` - shows detailed status info about services.

## Dependency management
- `systemctl list-dependencies [--reverse] <unit>` - shows unit dependencies \[dependendent units].
Unit dependency types:
    - **Requires**: if this unit loads, units listed in this \[unit] section will load also. If one of the other units is deactivated, this unit will also be deactivated.
    - **Requisite**: if the unit listed here is not already loaded, this unit will fail.
    - **Wants**: this unit wants to load the units that are listed here, but it will not fail if any of the listed units fail.
    - **Before**: this unit will start before units specified here.
    - **After**: this unit will start after the units specified here.

## Managing unit options
- `systemctl show <unit>` - show unit properties.
- `systemctl cat <unit>` - view the contents of a unit file.
- `systemctl edit <unit>` - edit unit properties.
- `systemctl daemon-reload` - reload the system manager configuration.




