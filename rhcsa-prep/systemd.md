# Systemd
## Definition

Systemd is a system and service manager, responsible for booting, starting and stopping services, managing processes and handling system shutdowns. 
Most prevalent units:
- Service
- Socket
- Target
- Mount
`systemctl -t help` displays a full list of units.

## System unit locations
- **/usr/lib/systemd/system**: contains default unit files that have been installed from RPM packages. Never edit these files directly.
- **/etc/systemd/system**: contains custom unit files.
- **/run/systemd/system**: contains automatically generated unit files.
The last location has the highest importance, since it overwrites any setting defined elsewhere. 

## Systemd units

### Service units
- Used to start processes.
- Service unit files consist of:
    - [Unit]: describes the unit and defines dependencies. Also contains *After* and *Before* statements, which indicite if the unit should be started before specified units, or after.
    - [Service]: describes how to start and stop the service and request status installation. `man 5 systemd.service` for more details.
    - [Install] indicates in which target this unit has to be started. Units that don't have this section cannot be started automatically.
### Mount units


### Socket units


### Target units


## Systemd status


## Unit overview commands



