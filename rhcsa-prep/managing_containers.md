# Managing Containers
---
## Understanding containers
- A **container** is a fancy way to run an application based on a container \
image that contains all required dependencies. 
- A container *image* is an executable package of software that includes \ code, runtime, system tools, system libraries and settings.
- A container *engine* is software that allows the running of multiple containers on the same OS kernel. **CRI-o** is the container engine used in Red Hat.
- A container *registry* can be compared to an app store, where different \
container images can be found. They are specified in /etc/containers/registries.conf
---

Containers rely on these features:
- Namespaces for isolation between processes;
- Cgroups for resorce management;
- SELinux for security.

Namespaces:
- Mount namespace - directory contents are presented in such a way where \
no other directories can be accessed.
- Process namespace - processes running in the namespace cannot reach \
or connect to processes in other namespaces.
- Network namespace - similar to VLAN, contact to other namespaces only \
possible through routers.
- User namespace - user IDs and group IDs are seperated between namespaces.
- Interprocess communication - is what processes use to connect to one \
another.
The network namespace is not enabled by default.

- **Cgroups** are a kernel feature that enables resource access limitation.
- **SELinux** secures access by using resource labels.
---
