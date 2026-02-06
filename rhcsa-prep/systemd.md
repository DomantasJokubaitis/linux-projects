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
- `systemctl enable <unit>` - create a symlink in the wants directory for the multiuser target for the service to automatically start on boot.
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



