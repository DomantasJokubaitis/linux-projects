# Managing Containers
## Understanding containers
- A **container** is a fancy way to run an application based on a container image that contains all required dependencies. 
- A container *image* is an executable package of software that includes code, runtime, system tools, system libraries and settings.
- A container *engine* is software that allows the running of multiple containers on the same OS kernel. **CRI-o** is the container engine used in Red Hat.
- A container *registry* can be compared to an app store, where different container images can be found. They are specified in /etc/containers/registries.conf
---

### Containers rely on these features:
- Namespaces for isolation between processes;
- Cgroups for resorce management;
- SELinux for security.

**Namespaces**:
- Mount namespace - directory contents are presented in such a way where no other directories can be accessed.
- Process namespace - processes running in the namespace cannot reach or connect to processes in other namespaces.
- Network namespace - similar to VLAN, contact to other namespaces only possible through routers.
- User namespace - user IDs and group IDs are seperated between namespaces.
- Interprocess communication - is what processes use to connect to one another.
The network namespace is not enabled by default.

**Cgroups** are a kernel feature that enables resource access limitation.
**SELinux** secures access by using resource labels.

## Containers on RHEL9
Red Hat uses the CRI-o container runtime and podman as the main tool to run containers.
Contrary to Docker, podman uses rootless containers, they run in a user namespace and don't need a daemon to work. Rootless containers do not have IP addresses.

## Container orchestration
*Kubernetes* is the most popular container orchestration tool. Red Hat uses *OpenShift*, a Kubernetes distribution.

Features of container orchestration:
- Easy connection to a wide range of external storage types.
- Secure access to sensitive data.
- Decoupling, such that site-specific data is strictly separated from the code inside the container environment.
- Scalability, such that additional instances can be easily data when the workload increases.
- Availability, ensuring that the outage of a container host doesn't result in container unavailability.

## Podman commands

