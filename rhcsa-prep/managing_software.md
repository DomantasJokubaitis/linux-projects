# Managing software
---
## DNF commands
- `dnf (re)install [package]` - (re)installs a package.
- `dnf upgrade | downgrade [package]` - upgrades | downgrades a package.
- `dnf remove [package]` - removes a package.
- `dnf swap [package A package B]` - swaps package A for B.
- `dnf search [package]` - searches for a package.
- `dnf info [package]` - shows package info.
- `dnf list --installed` - prints all installed packages.
- `dnf provides [file]` - shows the package providing a specific file.
- `dnf repolist` - shows which repositories are in use.
- `dnf repoquery --list [package]` - lists all files included in package.
- `dnf repoquery --requires [package]` - lists dependencies of a package.
- `dnf repoquery --installed-from-repo=<repoID>` - filters installed packages by the ID of the repository they were installed from.
- `dnf config-manager` - manages main configuration, repositories configuration and variables.
- `dnf group {list (hidden)} {install | info [package]` - allows the installation \
of multiple packages for a specific use case, for example: *c-development*.
- `dnf history {list | info}` - lists | shows info about transactions.
- `dnf history undo [id]` - undoes a transaction.
- `dnf history rollback [id]` - undoes all transactions performed after a specified transaction.
- `dnf autoremove` - removes packages that are not needed.
- `dnf clean all` - cleans cached files and metadata.
- `apropos dnf` - find man pages related to dnf.

## DNF module terminology
- A **module** describes a set of RPM packages that belong together.
- A **stream** contains one specific module version.
By using streams different versions of packages can be offered through the same repositories.
- A **profile** is a list of packages that are installed together for a particular use case.
For example: minimal, server, default profiles.
- Repositority configuration is stored in /etc/yum.repos.d/

## Creating your own repository
1. `dnf install createrepo_c`
2. Possess a directory with .rpm files
3. `dnf config-manager --add-repo=file:///path/to/directory`
4. Edit `/etc/yum.repos.d/directory.repo` and set `gpgcheck=0`
5. `createrepo path/to/directory`
6. `dnf repolist`

## RPM vs DNF
- **RPM** installs a single package and does not resolve dependencies.
- **DNF** resolves dependencies, keeps transaction history and manages repos.
## RPM flags
- `RPM -qa` - lists installed packages.
- `RPM -qi` - shows package description.
- `RPM -ql` - lists all files in a package.
- `RPM -qd` - shows all documentation for a package.
- `RPM -qc` - shows all configuration files in the package.
- `RPM -qf` - shows the package a file belongs to.
- `RPM -qp --scripts` - queries the package file, not the RPM database,
and looks what scripts it has, if any.
- `RPM -qR` - lists package dependencies.
- `RPM -Va` - shows which parts of the package have been changed since installation and verifies all installed packages.

## Containerized application packaging formats

Containerized application packaging formats pack required dependencies
together with the application in the same package to ensure that there
are **no dependency conflicts** between different applications.
Most popular formats:
1. Snap:
    - Developed by Canonical.
    - Packages are listed on Snap Store.
    - Has a daemon, snapd, which can be interected using `snap`.
    - It automatically checks for application updates in the background.
2. AppImage:
    - Developed by Simon Peter.
    - Packages are listed on AppImageHub.
    - Does not support automatic updates.
    - Package is executable on it's own.
    - By default is not sandboxed.
3. Flatpak:
    - Developed by Alexander Larsson.
    - Packages are listed on FlatHub.
    - Does not support automatic updates.
    - Decompressed on the client side.
    - Basic commands:
        1. `flatpak (un)install app_id`.
        2. `flatpak run app_id`.
        3. `flatpak search app`.
        4. `flatpak update`.
        5. `flatpak list`.
