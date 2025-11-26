- `dnf repolist` - shows which repos the system is using.
- `dnf provides [file]` - shows which package provides this file.
- `dnf group {list (hidden)} {install | info [argument]` - allows the installation
of multiple packages for a specific use case, for example: c-development, kde-desktop.
- `dnf info [argument]` - show package info.
- `dnf history undo [argument]` - undoes an action.
- `dnf list [argument]` - checks if the package is installed.
--
A **module** describes a set of RPM packages that belong together.
A **stream** contains one specific module version. By using streams different
versions of packages can be offered through the same repositories. **profile** is a list of packages that are installed together for a particular use case.
For example: minimal, server, default profiles.
