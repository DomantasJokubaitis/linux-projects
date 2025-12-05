# Managing software
---
## DNF commands
- `dnf install [argument]` - installs a package.
- `dnf up [argument]` - updates a package.
- `dnf remove [argument]` - removes a package.
- `dnf search [argument]` - searches for a package.
- `dnf info [argument]` - shows package info.
- `dnf repolist` - shows which repositories are in use.
- `dnf provides [file]` - shows the package providing a specific file.
- `dnf group {list (hidden)} {install | info [argument]` - allows the installation of multiple packages
for a specific use case, for example: *c-development*, *kde-desktop*.
- `dnf list {installed}` - prints all installed packages.
- `dnf repoquery --list [argument]` - lists all files included in package.
- `dnf repoquery --requires [argument]` - lists dependencies of a package.
- `dnf history undo [argument]` - undoes an action.
- `dnf autoremove` - removes packages that are not needed.
- `dnf clean all` - cleans cached files and metadata.
---
A **module** describes a set of RPM packages that belong together. \
A **stream** contains one specific module version.
By using streams different versions of packages can be offered through the same repositories.\
A **profile** is a list of packages that are installed together for a particular use case.
For example: minimal, server, default profiles.

# RPM
## RPM vs DNF
RPM doesn't automatically install dependencies like DNF.
It also doesn't update the DNF database when installing software using the
RPM command.



