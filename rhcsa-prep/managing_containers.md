# Managing Containers
## Understanding containers
- A **container** is a fancy way to run an application based on a container image that contains all required dependencies. 
- A container *image* is an executable package of software that includes code, runtime, system tools, system libraries and settings.
- A container *engine* is software that allows the running of multiple containers on the same OS kernel. **CRI-o** is the container engine used in Red Hat.
- A container *registry* can be compared to an app store, where different container images can be found. They are specified in /etc/containers/registries.conf
- A container *file* is a text file containing commands to build an image.
- Non root container files are stored in ~/.local/share/containers/storage directory.
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

**Cgroups** are a kernel feature that enables resource access limitation for process groups.
**SELinux** secures access by using resource labels.

## Containers on RHEL9
- Red Hat uses the CRI-o container runtime and podman as the main tool to run containers.
- Contrary to Docker, podman uses rootless containers, they run in a user namespace and don't need a daemon to work. Rootless containers do not have IP addresses.
- Both Docker and podman follow the Open Container Initiative (OCI). It's primary goal is to ensure the compatibility and interoperability of container environments through defined technical specifications.
## Container orchestration
*Kubernetes* is the most popular container orchestration tool. Red Hat uses *OpenShift*, a Kubernetes distribution.

Features of container orchestration:
- Easy connection to a wide range of external storage types.
- Secure access to sensitive data.
- Decoupling, such that site-specific data is strictly separated from the code inside the container environment.
- Scalability, such that additional instances can be easily data when the workload increases.
- Availability, ensuring that the outage of a container host doesn't result in container unavailability.

## Running containers
- Install the appropriate software using `sudo dnf install container-tools`
- `podman run -it [image] /bin/sh` - run a container image with shell access.
- `podman attach <name>` - attach to a running container.
- `Ctrl-P Ctrl-Q` to detach an image.
- `podman ps [-a] | podman container ls [-a]` to see an overview of running containers.
- `podman-compose up -d` to build and start a container in the background.

## Working with container images
- `podman info` - see which registries are used.
- `podman login` - get access to subscriber-only Red Hat registries.
- `podman search [--filter]` - find and filter available container images.
`podman search` output fields:
    - INDEX
    - NAME
    - DESCRIPTION
    - STARS
    - OFFICIAL
    - AUTOMATED
- `skopeo inspect` - inspect an image before pulling.
- `podman inspect` - inspect a local image.
- `podman images | podman image ls` - list locally available images.
- `podman pull` - pull an image from a registry.
- `podman rmi | podman image rm` - remove an image.
- `podman build [-t <iamgename:tag .>]` - build a custom image using different instructions in a Containerfile.
Containerfile example:
```
FROM registry.access.redhat.com/ubi8/ubi:latest
RUN dnf install nmap
CMD ["/usr/sbin/nmap", "-sn", "192.168.29.0/24"]
```

## Managing containers
- `podman stop` - send a SIGTERM signal to a container.
- `podman kill` - send a SIGKILL signal to a container.
- `podman restart` - restart a currently running container.
- `podman rm | podman container rm` - remove container files.
- `podman logs` - see container logs.
- `podman exec` - run a second command (after container entrypoint command) inside a container.
`podman run -e <env>` - run a container with environment variables.

## Managing container ports
Rootless containers don't have network addresses because they have insufficient privileges to allocate one. To make the container accessible from the outside, port forwarding must be setup. Only ports 1025-65535 can be assigned to containers, because ports 1-1024 are accessible by root only.
You can use `sudo podman run` to run a root container with an IP address and a privileged port.
`podman run -p <hostport:containerport>` - run nginx container which is exposed on hostport. Don't forget to add a firewall rule for the hostport and reload the firewall.

## Managing container storage
- `podman run -v <host_dir:container_dir:Z` - attach storage to a container. The user who runs the container has to have ownership of the host_dir, write permission is not enough.
- `podman unshare` - run commands inside the container namespace. For example, `podman unshare chown 27:27 <db>` 
## Running containers as systemd services

Containers can be autostarted by generating systemd files with `podman generate systemd --name <containername> --files`. After that, the new unit files need to be enabled using `systemctl --user enable container.mycontainer.service`. By default, they would autostart when the user (container owner) start a session. It's possible to change this behaviour by using `loginctl enable-linger <user>`. When linger is enabled, systemd services that are enabled for that user will be started on system start, not on user session start.
