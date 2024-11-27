# Docker Notes

## Components in Hardware
- **CPU**: Processes instructions.
- **HDD (Hard Disk Drive)**: Permanent data storage.
- **RAM (Random Access Memory)**: Temporary storage for running processes.
- **Graphics Card**: Handles rendering of images, videos, and animations.

## Components in Software
- **OS (Operating System)**: Manages hardware and provides a platform for other software to run.

---

## How to Access Data in Notepad
1. You have a Notepad file containing user details.
2. To view the information:
   - Double-click the file.
   - The data is stored in the HDD.
   - The OS interacts with the HDD to retrieve and display the data.

---

## How Hardware Understands Software Requests
- **Kernel**: The kernel converts software requests into hardware instructions.
  - It acts as a bridge between hardware and software.

---

## Difference Between Virtualization and Containers

### Virtualization
- **Infrastructure**: Managed by VMware or data centers.
- **Hypervisor**:  
  - Think of it like two kids asking their mother for chocolates. The mother (hypervisor) gives each child the resources they request (CPU, RAM, storage).
  - The hypervisor ensures the guest OS gets the necessary resources from the infrastructure.
- **Guest OS**: Each virtual machine (VM) has its own OS and kernel.
  - For example, you install Jenkins on top of the guest OS.  
  - Before installing Jenkins, you need to install Java libraries.  
  - If you're installing Node.js, you need to install dependencies like Node 14.

#### Disadvantages of Virtualization
1. **Multiple Environments**:  
   - In real-time, there are different environments: development (dev), QA, client, etc.  
   - For example:
     - In the **dev environment**, Jenkins runs fine on Linux.
     - In the **QA environment**, Jenkins doesn't work because it’s running on Windows.
     - On the **client's machine**, the application might run on macOS.
   - This can cause compatibility issues and confusion between environments.

2. **Resource Consumption**: Each guest OS consumes significant resources from the infrastructure or data center. Guest OS is heavy because it includes its own OS and kernel.

---

### Containers
- **Solution**: Containers were introduced as a lightweight alternative to VMs.
- **Infrastructure**: Managed by cloud providers like AWS.
- **OS**: Containers share the host OS.
- **Container Engine**: Docker, containerd, CRI-O.
  - Containers do not include a kernel. They only include the application and its necessary libraries.
  - Containers get their resources from the base OS.

#### Example: Django Application
- A Django application inside a container would include the app and required Python libraries but would rely on the base OS for resources.

---

## Key Differences: Virtualization vs Containers

| Feature                 | Virtual Machines (VMs)     | Containers               |
|-------------------------|----------------------------|--------------------------|
| **Kernel**              | Each VM has its own kernel | Shared kernel with base OS|
| **Resource Usage**      | Heavy                      | Lightweight              |
| **Environment Issues**  | Common                     | Minimized                |
| **Use Case**            | Full OS for each guest     | Lightweight apps and microservices |

---

## Why Containers Are Better
- **Efficiency**: Containers eliminate the need for heavy guest OS installations.
- **Consistency**: Containers ensure that applications run the same across different environments (Dev, QA, Production).
- **Lightweight**: Containers are smaller and consume fewer resources than VMs.

---

## Conclusion
- **Virtualization**: Uses hypervisors to manage virtual machines with their own OS and kernel, consuming a lot of resources.
- **Containers**: Use a shared OS and container engine to run lightweight, isolated applications, making them more efficient.
- **Docker**: One of the most popular container engines used for building and managing containers.

---

# Docker Notes

## Docker in Development vs Production

- **Docker Engine in Development**: 
  - Developers use Docker Engine in development environments to build, test, and manage containers locally.
  - **Kubernetes in Docker**: Kubernetes can run on a Docker environment, typically in a single-node cluster for local testing and development.
  - **KIND**: Kubernetes IN Docker (KIND) is used for running Kubernetes clusters on Docker itself. It is a useful tool for testing Kubernetes locally.

- **Containerd in Production**:
  - In production environments, **Containerd** is more commonly used instead of Docker Engine. 
  - Containerd is a container runtime that is lighter and better suited for production environments.
  - **Why not Docker Engine in Production**: Docker Engine is primarily designed for development and testing. In production, you need a more streamlined, efficient container runtime like Containerd.

## Networking and Ifconfig Command

### Configuring Network Interface with `ifconfig`
The `ifconfig` command is used to configure network interfaces and display network configuration details on Linux systems. 

- **Use Case**: 
  - You can use `ifconfig` to assign an IP address to a network interface and to configure or display the current network interface configuration.

#### Example Command: 

```
ifconfig
```
---
This command will show the network configuration for all network interfaces on your system.

Example Output:

```
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST> mtu 9001
        inet 172.31.92.186  netmask 255.255.240.0  broadcast 172.31.95.255
        inet6 fe80::10a4:42ff:fe07:e1d3  prefixlen 64  scopeid 0x20<link>
        ether 12:a4:42:07:e1:d3  txqueuelen 1000  (Ethernet)
        RX packets 26156  bytes 36803903 (36.8 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 4435  bytes 438413 (438.4 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING> mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 168  bytes 19234 (19.2 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 168  bytes 19234 (19.2 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

Installing ifconfig and jq on Ubuntu
To install ifconfig and jq on an Ubuntu EC2 instance, run the following commands:

```
sudo apt update && sudo apt install net-tools
sudo apt install jq
```

Installing Docker
To install Docker on your system, run this command:

```
curl https://get.docker.com/ | bash
```
This command will download and install Docker using an automatic script.

Once Docker is installed, verify the installation by checking the Docker version:

```
docker --version
```

Docker Networking: docker0 Bridge Network
After installing Docker, run ifconfig again to see a new network interface called docker0. This is Docker's default bridge network for containers.

Example Output for docker0:
```
docker0: flags=4099<UP,BROADCAST,MULTICAST> mtu 1500
        inet 172.17.0.1  netmask 255.255.0.0  broadcast 172.17.255.255
        ether 02:42:e5:5c:18:cf  txqueuelen 0  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

# Explanation:
  - docker0: This is a virtual Ethernet bridge created by Docker to allow containers to communicate with each other and with the host system.
  - eth0: Represents the primary network interface on the host system (usually the physical network interface).
  - docker0 and eth0: docker0 connects the Docker containers to the host network via eth0.

# Docker Networking Between Containers and Host

- Communication Flow
  - Containers communicate with each other and the host system through the docker0 bridge network. If a container needs to access the internet, the traffic flows as follows:
  - Container → docker0 → eth0 → Internet.
  - docker0 acts as a bridge network between containers and the host system's network.
    
# Example: Communication Between Containers

- Let's say you have three containers running on Docker:
  - Container 1 (C1), Container 2 (C2), and Container 3 (C3).
    
- If C1 needs to access the internet, it will route traffic through the following path:
  - C1 → docker0 → eth0 → Internet

# Conclusion
  - Docker Engine is widely used in development environments to build and manage containers locally.
  - Containerd is a lighter and more efficient container runtime, making it suitable for production environments.
  - Docker uses the docker0 bridge network to allow communication between containers and between containers and the host system.
