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
curl https://get.docker.com/ | 
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

## Custom Network in Docker

You can create a custom network in Docker to manage how containers communicate with each other.

### Example: Create a Custom Network
```
docker network create my_custom_network
```
Once the network is created, you can connect containers to it by specifying the network when running containers.

```
docker run --network my_custom_network <image-name>
```
---

## Docker Client and Daemon

- **Docker Client**: The Docker client (`docker`) sends commands to the Docker daemon. When you run commands like `docker run`, the client sends HTTP API calls to the Docker daemon.
- **Docker Daemon**: The Docker daemon (`dockerd`) is the background process that handles the creation, management, and execution of Docker containers.

### How the `docker run` Command Works

When you run the command:

```
docker run <image-name>
```
- An HTTP API call is sent to the Docker daemon.
- If the image is not already on the system, the daemon downloads the image from the Docker registry.
- Once the image is available, the daemon runs a container based on the image.

---

## Checking Running Processes

You can check running processes using the following command:

```
ps -ef
```
This will list all processes running on the system. The output will include the process IDs (PID) and the command being executed.

---

## Background Processes

To run a command in the background, use the `&` symbol at the end of the command. For example:
```
ping -n 100 www.google.com &
```
- `ping -n 100 www.google.com`: Pings `www.google.com` for 100 packets.
- `&`: Runs the process in the background.

The process runs as the current user, and the number `100` refers to the number of seconds the ping will run.

---

## Namespaces in Linux

Namespaces are a feature of the Linux kernel that isolates processes from each other. Each process runs in its own namespace, preventing interference between processes.

### Checking Process Namespaces

You can list namespaces with:
```
lsns -t pid
```
This shows the namespaces associated with processes. For example:

NS TYPE NPROCS PID USER COMMAND 4026531836 pid 105 1 root /sbin/init

- `pid`: Refers to the process ID (PID) namespace.
- `NPROCS`: Number of processes in this namespace.
- `PID`: Process ID of the command.
- `USER`: The user who is running the command.
- `COMMAND`: The command being executed.

You can monitor namespaces in real-time using the following command:
```
watch lsns -t pid
```
---

## Process Isolation with Namespaces

Each process runs in its own isolated namespace. For example:

- **Java processes**: Each Java process has its own namespace.
- **Containers**: Containers in Docker also have isolated namespaces to ensure they do not interfere with each other.

This isolation helps in security and resource management.

---

## Exiting a Process

To stop a running process in the terminal, you can press `Ctrl + C`, which sends a termination signal to the process.

---

## Docker Container Lifecycle

### Checking Container Status

You can list all containers (including stopped ones) using:
```
docker ps -a
```

Example Output:
```
CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES ffef94ff263a nginx:latest "/docker-entrypoint.…" 2 minutes ago Exited (0) 5 seconds ago app1
```

- `Exited (0)`: Indicates that the container stopped successfully without any errors.

---

## Running Containers in Background (Detached Mode)

To run a container in the background (detached mode), use the `-d` flag:

```
docker run -d --name app2 nginx:latest
```

- `-d`: Runs the container in detached mode, so it runs in the background.

To stop the containers:

```
docker stop app1 docker stop app2
```
---

## Running Multiple Containers in a Loop

To run multiple containers in a loop (e.g., running 10 containers):
```
for i in {1..10}; do docker run -d nginx:latest done
```

This will create 10 containers with auto-generated names.

---

## Stopping All Containers

To stop all containers without specifying each container name:
```
docker stop $(docker ps -aq)
```

- `$(docker ps -aq)`: This command fetches the IDs of all containers (including stopped ones), and `docker stop` stops them.

---

## Removing All Stopped Containers

To remove all stopped containers:
```
docker rm $(docker ps -aq)
```

---

## Automatically Removing Containers After Stopping

You can use the `--rm` flag to automatically remove containers after they are stopped:
```
docker run --rm -d --name app1 nginx:latest
```

This command will run the `app1` container in detached mode and automatically remove it once stopped.

---

## Viewing Running Containers

To list all running containers:
```
docker ps
```

Example output:
```
CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES 2b7130099d5b nginx:latest "/docker-entrypoint.…" 21 seconds ago Up 20 seconds 80/tcp database 6c9fff14d13b nginx:latest "/docker-entrypoint.…" 25 seconds ago Up 24 seconds 80/tcp backend 438348d94895 nginx:latest "/docker-entrypoint.…" 30 seconds ago Up 29 seconds 80/tcp frontend
```
---

## Port Forwarding (Mapping Container Ports to Host Ports)

To allow different containers to be accessed on specific ports, use the `-p` flag for port forwarding. For example, you can expose the frontend on port 8000 and backend on port 8001:
```
docker run --rm -d --name frontend -p 8000:80 nginx:latest docker run --rm -d --name backend -p 8001:80 nginx:latest
```

- `-p 8000:80`: Maps port `80` inside the container to port `8000` on the host.
- Similarly, `-p 8001:80` maps the backend container to port `8001`.

---

## Deleting Containers Automatically on Stop

To delete containers immediately after stopping them, use the `--rm` flag with the `docker run` command. For example:
```
docker run --rm -d --name frontend nginx:latest docker run --rm -d --name backend nginx:latest docker run --rm -d --name database nginx:latest
```
---

## Checking Logs of a Container

To view logs from a container, use `docker logs`:
```
docker logs -f backend
```

You can also navigate to the container logs directly in the Docker container storage directory:
```
cd /var/lib/docker/containers/ ls cd 8dca3f663019bda8ad378929ad2c0303987a4f7256bc847009f8d2375170c022 cat 8dca3f663019bda8ad378929ad2c0303987a4f7256bc847009f8d2375170c022-json.log
```

---

## Inspecting a Container's IP Address

To get the IP address of a running container, use:
```
docker inspect backend | grep -i IPAddress
```

Example Output:
```
"172.17.0.2"
```
---

## Custom Aliases for Docker Commands

You can create custom aliases for frequently used Docker commands. For example, to stop all containers:
```
dstop() { docker stop $(docker ps -aq) }
```

This function will stop all containers without needing to type their names.

---

## Summary of Key Commands

### Run a container in the background:
```
docker run -d --name app2 nginx:latest
```

### Stop all containers:
```
docker stop $(docker ps -aq)
```

### Remove all stopped containers:
```
docker rm $(docker ps -aq)
```

### Automatically remove container after stopping:
```
docker run --rm -d --name app1 nginx:latest
```

### Expose container ports to the host:
```
docker run --rm -d --name frontend -p 8000:80 nginx:latest
```

### View container logs:
```
docker logs -f backend
```
---
Docker Setup and Management Notes
1. Installing Docker
Update the system and install necessary tools:

```
sudo apt update && sudo apt install net-tools jq
Install Docker:

```
curl https://get.docker.com/ | 
2. Managing Docker Containers
Run a container:

```
docker run --rm -d --name app1 -p 8000:80 nginx:latest
List running containers:

```
docker ps
Stop a container:

```
docker stop app1
Remove a Docker image:

```
docker rmi nginx:latest
3. Configuring Docker Storage
Issue: Root Volume Gets Full
Docker by default stores container data in /var/lib/docker/containers/, which can fill up the root volume.
To avoid this, move Docker's storage to a separate disk (e.g., EBS volume).
Steps to change Docker's storage location:
Attach a new EBS volume (e.g., 50GB).
Format the new volume:

```
fdisk /dev/xvdf  # Follow prompts to create a new partition
mkfs.ext4 /dev/xvdf1  # Format it
Get the UUID of the new partition:

```
sudo blkid /dev/xvdf1
Edit /etc/fstab to mount the volume automatically:

```
sudo nano /etc/fstab
Add the following line (replace UUID with the actual UUID):


```
UUID=692d2f5c-4d9f-4b3f-affc-be50a1925082 /dockerdata ext4 defaults 0 0
Mount the volume:

```
sudo mount -a
Verify the mount:

```
df -h
ls -al /dockerdata
Change Docker’s default storage directory to /dockerdata by modifying Docker's config files.
4. Docker Networking
Docker containers can communicate via different network modes:

Bridge (default): Containers use IP addresses to communicate.
Custom Network: Allows communication between containers using names (easier for inter-container communication).
Host Network: Containers use the host's network interface.
None: No network access.
To create a custom network:


```
docker network create my_custom_network
docker run --rm -d --name app1 --network my_custom_network nginx:latest
Why can’t containers communicate by name?
Containers, by default, use IP addresses for communication. To enable name-based communication, use a custom network.

5. Monitoring Disk Usage
Check disk usage to avoid running out of space:


```
df -h
To see attached disks:


```
lsblk
Example output of lsblk:

```
NAME         MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
nvme1n1      259:4    0   50G  0 disk 
6. Example: Formatting and Mounting a New EBS Volume
Create a partition:

```
fdisk /dev/xvdf
# Follow the prompts: 'n' to create a new partition, 'w' to save
Format the partition:

```
mkfs.ext4 /dev/xvdf1
Get the UUID of the partition:

```
sudo blkid /dev/xvdf1
Modify /etc/fstab to mount the new partition automatically:

```
sudo nano /etc/fstab
Add the following line (replace UUID with the actual UUID):


```
UUID=692d2f5c-4d9f-4b3f-affc-be50a1925082 /dockerdata ext4 defaults 0 0
Mount the volume:

```
sudo mount -a
7. List of Commands
Here’s a summary of the commands used in the guide:


1  curl https://get.docker.com/ | 
2  sudo apt update && sudo apt install net-tools jq
3  df -h
4  docker ps
5  docker stop app1
6  docker rmi nginx:latest
7  cd /var/lib/docker/containers/
8  fdisk /dev/xvdf
9  mkfs.ext4 /dev/xvdf1
10 sudo blkid /dev/xvdf1
11 sudo nano /etc/fstab
12 sudo mount -a
13 lsblk
14 df -h
15 ls -al /dockerdata
Summary
This guide covers Docker installation, managing containers, configuring storage, and networking. It explains how to move Docker's data to a separate volume to prevent the root volume from getting full, and how to create custom networks for container communication.


1. Change Docker's Service Directory
To change Docker’s default storage location (e.g., /var/lib/docker) to a different directory (e.g., /dockerdata), follow these steps:

Step 1: Stop Docker Service

```
sudo systemctl stop docker.service
sudo systemctl stop docker.socket
Step 2: Modify Docker Service File
Edit the Docker service file to specify the new data directory.


```
sudo nano /lib/systemd/system/docker.service
Find the line starting with ExecStart and modify it to:


```
ExecStart=/usr/bin/dockerd --data-root /dockerdata -H fd:// --containerd=/run/containerd/containerd.sock
Step 3: Copy Data to New Directory
Use rsync to copy Docker’s existing data to the new directory:


```
sudo rsync -aqxp /var/lib/docker/ /dockerdata
Step 4: Reload Systemd and Restart Docker
Reload systemd and start Docker:


```
sudo systemctl daemon-reload
sudo systemctl start docker
Check Docker status:


```
sudo systemctl status docker --no-pager
Verify Docker processes:


```
ps aux | grep -i docker | grep -v grep
2. Troubleshooting Docker with Networking Tools
Install tools for troubleshooting:


```
sudo apt install net-tools jq
Run a container with troubleshooting tools:


```
docker run --rm -d --name app1 -p 8001:80 dockerid/troubleshootingtools:v1
Useful Network Commands for Troubleshooting:
netstat – Show active network connections.
ifconfig – Display network interfaces and their configurations.
docker network ls – List Docker networks.
3. Deleting Docker Containers
To remove a running Docker container:

Step 1: List Running Containers

```
docker ps
Step 2: Stop and Remove a Container

```
docker stop <container_id>
docker rm <container_id>
For example, to stop and remove a container with ID b5893e827aad:


```
docker stop b5893e827aad
docker rm b5893e827aad
4. Networking Modes in Docker
Host Network: The container uses the host’s IP and network stack, useful for monitoring tools (e.g., Prometheus Node Exporter).
Overlay Network: Used in Docker Swarm for container communication across nodes.
Host network usage example:

```
docker run --rm -d --name app1 --network host nginx:latest
This will make the container use the host's IP directly without -p (port binding).

Summary of Commands

```
1  sudo systemctl stop docker.service
2  sudo systemctl stop docker.socket
3  sudo nano /lib/systemd/system/docker.service
4  sudo rsync -aqxp /var/lib/docker/ /dockerdata
5  sudo systemctl daemon-reload && sudo systemctl start docker
6  sudo systemctl status docker --no-pager
7  ps aux | grep -i docker | grep -v grep
8  docker run --rm -d --name app1 -p 8001:80 dockerid/troubleshootingtools:v1
9  docker network ls
10  docker ps
11  docker stop <container_id>
12  docker rm <container_id>


1. Ping by IP vs. Ping by Container Name
Problem:
When running two containers (app1 and app2) on the default bridge network:


```
docker run --rm -d --name app1 -p 8000:80 dockerid/troubleshootingtools:v1
docker run --rm -d --name app2 -p 8001:80 dockerid/troubleshootingtools:v1
If you try to ping one container (e.g., app1) from the other using the container’s IP address:


```
ping 172.17.0.2  # app1's IP address
This won’t work, but you can ping using the container name (e.g., app1), if on the same network.

Why this happens:
Containers by default are stateless, and their IP addresses can change when the container restarts. This is why it's not recommended to use IP addresses to ping containers. Docker resolves container names to their respective IP addresses through DNS when they are in the same custom network.

In Kubernetes, the equivalent concept is handled by labels or service names that remain constant.

2. Docker Custom Network
To ensure that you can ping containers by their names, you need to create a custom network. Here’s how:

Step 1: Create Custom Network

```
docker network create custom-net
Step 2: Run Containers on Custom Network
Run the containers on the custom network so that they can communicate by name:


```
docker run --rm -d --name app3 -p 8003:80 --network custom-net dockerid/troubleshootingtools:v1
docker run --rm -d --name app4 -p 8004:80 --network custom-net dockerid/troubleshootingtools:v1
Step 3: Inspect Container IPs

```
docker inspect app3 | grep -i IPAddress
docker inspect app4 | grep -i IPAddress
You can now ping containers by their names, like this:


```
docker exec -it app3 
ping app1  # This should work since they are on the same network
Step 4: Connecting Containers to Custom Network
You can also connect containers to the custom network later:


```
docker network connect custom-net app1
3. Host Network Mode
In Docker, the host network mode allows a container to use the host machine's network directly, meaning it won't have its own IP address but will instead use the host’s IP. This is useful for running services like Prometheus Node Exporter.

Example: Run Prometheus Node Exporter on Host Network

```
docker run --rm -d --name nodeexporter --network host prom/node-exporter
The container will use the host machine’s IP address.
You can access the Prometheus metrics from the host using http://<host-ip>:9100/metrics.
The port 9100 is exposed in the image’s metadata:

```
docker image inspect prom/node-exporter:latest
This will show the exposed ports, including 9100/tcp, which Prometheus Node Exporter uses to expose metrics.

4. Port Forwarding in Docker
When using Docker's bridge network mode (the default mode), you can map container ports to the host machine's ports using -p:


```
docker run --rm -d --name app1 -p 8000:80 dockerid/troubleshootingtools:v1
Here, the container’s port 80 is forwarded to the host's port 8000.

5. Volumes and Bind Mounts
Difference Between Volumes and Bind Mounts:
Volumes: Managed by Docker and stored in Docker's own filesystem (/var/lib/docker/volumes/).

Bind Mounts: You mount a directory from the host filesystem directly into the container. You can specify the host directory for the mount, which means you control where the data is stored.

Creating and Using Volumes:

```
docker volume create myvolume
docker run -d --name app1 -v myvolume:/data dockerid/troubleshootingtools:v1
Bind Mount Example:

```
docker run -d --name app1 -v /host/path:/container/path dockerid/troubleshootingtools:v1
This binds the /host/path from the host machine to /container/path in the container.

Summary of Commands:

```
1  docker network create custom-net
2  docker run --rm -d --name app3 -p 8003:80 --network custom-net dockerid/troubleshootingtools:v1
3  docker run --rm -d --name app4 -p 8004:80 --network custom-net dockerid/troubleshootingtools:v1
4  docker exec -it app3 
5  ping app1  # Ping container by name on custom network
6  docker network connect custom-net app1
7  docker run --rm -d --name nodeexporter --network host prom/node-exporter
8  docker image inspect prom/node-exporter:latest
9  docker run --rm -d --name app1 -p 8000:80 dockerid/troubleshootingtools:v1
10 docker volume create myvolume
11 docker run -d --name app1 -v myvolume:/data dockerid/troubleshootingtools:v1
12 docker run -d --name app1 -v /host/path:/container/path dockerid/troubleshootingtools:v1

setup with MongoDB using Docker, along with how to use Volumes and Bind Mounts in Docker, and a step-by-step guide to the MongoDB example:

Volumes vs Bind Mounts
Volumes:

Volumes are managed by Docker and are useful when you want to persist data even if the container is removed or stopped. Volumes are stored in Docker's default storage location, or you can specify a location (e.g., an EBS volume).
You can easily back up and restore volumes using Docker CLI.
Bind Mounts:

Bind mounts allow you to mount a specific directory from your host machine into a container. These can be used for configuration files, logs, etc., but they're tied directly to the host system.
When using bind mounts, if you stop or remove a container, the data is still accessible on the host system.
Setting Up MongoDB with Docker and Volumes
Step 1: Running MongoDB Without Volume (Stateless Container)
If you run MongoDB without a volume, the data will be lost when the container is stopped or removed.


```
docker run --rm -d --name mongodb -p 27017:27017 mongo:latest
What happens? The data is stored inside the container, and if the container stops, all data is lost.
Step 2: Creating a Docker Volume
To persist data even after the container is stopped or removed, you need to create a Docker volume.


```
docker volume create mongodb
This command creates a named volume called mongodb, where the container’s data will be stored.
Step 3: Running MongoDB with Volume
Now, you can run MongoDB and attach the volume to the container:


```
docker run --rm -d --name mongodb -v mongodb:/data/db -p 27017:27017 mongo:latest
What happens? The -v mongodb:/data/db flag mounts the Docker volume mongodb to the /data/db directory inside the container. This ensures that data is stored in the volume, and even if the container stops, the data will persist.
Step 4: Inserting Data into MongoDB
Now that MongoDB is running with persistent storage, you can connect to the container and insert data.

Connect to MongoDB:

```
docker exec -it mongodb mongosh
Insert Data:
javascript
```
db.helo.insertMany([
    { "_id": 1, "name": "Matt", "status": "active", "level": 12, "score": 202 },
    { "_id": 2, "name": "Frank", "status": "inactive", "level": 2, "score": 9 },
    { "_id": 3, "name": "Karen", "status": "active", "level": 7, "score": 87 },
    { "_id": 4, "name": "Katie", "status": "active", "level": 3, "score": 27, "status": "married", "emp": "yes", "kids": 3 },
    { "_id": 5, "name": "Matt1", "status": "active", "level": 12, "score": 202 },
    { "_id": 6, "name": "Frank2", "status": "inactive", "level": 2, "score": 9 },
    { "_id": 7, "name": "Karen3", "status": "active", "level": 7, "score": 87 },
    { "_id": 8, "name": "Katie4", "status": "active", "level": 3, "score": 27, "status": "married", "emp": "yes", "kids": 3 }
]);
Query the Data:
javascript
```
db.helo.find({name: "Katie"})
This will return the document(s) with the name "Katie" from the helo collection.
Step 5: Verify Persistence
If you stop the container and start it again, the data will be persistent because it's stored in the Docker volume.

Stop the MongoDB container:

```
docker stop mongodb
Start the MongoDB container again:

```
docker run --rm -d --name mongodb -v mongodb:/data/db -p 27017:27017 mongo:latest
Connect to MongoDB again and check if the data is still there:

```
docker exec -it mongodb mongosh
db.helo.find({name: "Katie"})
The data will be retained because it’s stored in the Docker volume and not inside the container’s filesystem.

Docker Architecture and Components:
Docker Client:

The Docker client is the interface where users interact with Docker (through commands like docker run, docker ps, etc.).
Docker Daemon:

The Docker daemon is a background process that manages Docker containers, images, and networks. The client communicates with the daemon via an HTTP API.
Docker Socket:

The Docker socket (/var/run/docker.sock) is the communication channel through which the Docker client talks to the Docker daemon. When you run a Docker command, it sends requests to this socket.
Bind Mounting the Docker Socket:

To allow a container to communicate with the Docker daemon, you need to bind mount the Docker socket into the container. This lets the container interact with Docker commands, such as listing containers (docker ps).
Example to mount the Docker socket in a container:


```
docker run -v /var/run/docker.sock:/var/run/docker.sock --rm -d --name portainer -p 9000:9000 portainer/portainer
This will run Portainer, a web UI for managing Docker, and it will have access to the Docker socket for container management.

Summary of Key Concepts:
Stateless Containers: Containers are stateless by default, meaning if a container is stopped or removed, its data is lost.
Volumes: Use Docker volumes to persist data outside the container’s lifecycle. Volumes can be stored on host systems like EBS and provide durability even if the container is stopped or removed.
Bind Mounts: Use bind mounts when you want to link specific files or directories from the host system to the container. Useful for configuration files and logs.
MongoDB with Docker: Store MongoDB data in a Docker volume to ensure data persists across container restarts.

Volumes and Bind Mounts in Docker:
In Docker, volumes and bind mounts are two primary methods used to store and persist data outside of containers. However, they serve slightly different purposes, and understanding their use cases is crucial when managing stateful applications in Docker.

1. Volumes:
Persistent Data Storage: Volumes are the preferred way to persist data generated by and used by Docker containers. Unlike bind mounts, volumes are managed by Docker, making it easier to back up, restore, and migrate data.

Advantages of Volumes:

Volumes are stored in Docker’s designated directory (/var/lib/docker/volumes/ by default), but you can configure it to store data in external storage such as AWS EBS (Elastic Block Store).
Volumes can be backed up, and Docker manages their lifecycle.
You can create snapshots of the volumes directly in AWS for backup or disaster recovery.
Example: MongoDB with Volume:

Create a volume for MongoDB:


```
docker volume create mongodb
Run the MongoDB container with the volume:


```
docker run --rm -d --name mongodb -v mongodb:/data/db -p 27017:27017 mongo:latest
This command mounts the volume (mongodb) to /data/db inside the MongoDB container to persist database data even if the container is stopped or removed.

Check the data in the volume (after inserting some data into MongoDB):

After inserting data into MongoDB, stop and restart the container. The data should still be accessible, as it is stored in the volume.
Navigate to the volume’s data directory:

```
cd /dockerdata/volumes/mongodb/_data
Snapshot in AWS:

You can take a snapshot of the EBS volume where Docker stores the volume data. This is useful for backup or disaster recovery.
2. Bind Mounts:
Direct Mapping to Host Files: Bind mounts are used to mount specific directories or files from the host system into a container. This means any changes made to the mounted directory in the container are reflected on the host system and vice versa.

When to Use Bind Mounts:

Bind mounts are useful when you need to access specific files on the host system, such as configuration files, logs, or for development environments where you want changes to the code to immediately reflect inside the container.
Example: Portainer with Bind Mounts:

Portainer Setup: Portainer is a popular Docker management UI tool. You can run Portainer in a container, and use bind mounts to interact with the Docker daemon and track the container status.

Run Portainer with Bind Mount:


```
docker run -d -p 8000:8000 -p 9443:9443 --name portainer \
--restart=always \
-v /var/run/docker.sock:/var/run/docker.sock \
-v portainerdata:/data \
portainer/portainer-ce:2.11.1
This will mount the Docker socket (/var/run/docker.sock) from the host into the container. This allows Portainer to communicate with the Docker daemon on the host machine, giving it full access to manage Docker containers.

Use Case of Bind Mounts: You would typically use a bind mount for monitoring and management tools like Portainer. The bind mount enables the Portainer container to interact with Docker, view running containers, and show logs and statuses.

3. Docker Architecture & Docker Daemon:
Docker Client-Server Architecture: The Docker client communicates with the Docker daemon, which runs as a background process on the host. The client sends commands (like docker ps, docker run, etc.) to the daemon via HTTP API calls.

Docker Socket: The Docker socket (/var/run/docker.sock) is how the Docker client communicates with the Docker daemon. When you mount this socket inside a container (using bind mounts), the container can access the Docker daemon and issue commands as if it were the Docker client.

Example: If you want to run docker ps from inside a container, you need to mount the Docker socket:


```
docker run --rm -d --name app2 -v /var/run/docker.sock:/var/run/docker.sock --network none dockerid/troubleshootingtools:v1
This allows the container to use the Docker client commands and interact with the host’s Docker daemon.

4. Three-Tier Architecture and Containerization:
In a three-tier architecture, you typically have:

Frontend: The web application user interface.
Backend: The application layer where the logic resides.
Database: Stores the data used by the application.
In a containerized environment:

Frontend: Can be a web server or an app running in a container.
Backend: An application container communicating with the database container.
Database: A persistent data store running in a container with volumes to persist data (e.g., MongoDB, PostgreSQL, etc.).
Containers are stateless by default, meaning that if a container is stopped or removed, its data is lost. This is why volumes are critical for stateful services like databases.

Best Practices:

Use volumes for databases to persist data even if containers are restarted.
Use bind mounts for configuration files or logs that you want to persist outside the container and to allow real-time updates.
5. Handling Data Loss in Containers:
Developers often report that when they stop and restart a container, they lose all the data. This can be avoided by:

Mounting volumes for persistent data storage.
If the developer doesn’t know how to handle it, the DevOps engineer can set up volumes or bind mounts to persist data.
Steps to Handle This Request:

Create a Volume:

```
docker volume create mongodb
```
Run MongoDB with Volume:

```
docker run --rm -d --name mongodb -v mongodb:/data/db -p 27017:27017 mongo:latest
```
Data Persistence: Insert data, stop the container, and start it again. The data should still be accessible because it's stored in the volume.
You can then instruct the developer to check the data inside the volume directory and even take snapshots in AWS to back it up.
Conclusion:
Volumes are used to persist data in Docker containers, especially for stateful services like databases.
Bind mounts are useful for mapping host directories into containers for development, configuration management, and monitoring purposes.
Using AWS EBS with Docker volumes can help persist data even after container restarts, and snapshots can be taken for backup and disaster recovery.
Docker Socket Mounting enables containers to interact with the Docker daemon and access information about the host system, useful for tools like Portainer.

setting up Docker containers and managing them using various tools and configurations, such as volumes, bind mounts, Portainer, Dockerfile instructions, and build arguments. Let me walk you through the key points you're covering:

1. Docker and Socket Binding for Inter-Container Communication:
You're trying to use Docker socket mounting to allow one container (e.g., app2) to communicate with the Docker daemon on the host system. This allows the container to query Docker information, like inspecting containers, viewing logs, etc.

Problem: When you ran the docker ps inside a container (app1), it failed to connect to the Docker daemon.

Solution: Mount the Docker socket into the container to allow it to communicate with Docker:


```
docker run --rm -d --name app2 -v /var/run/docker.sock:/var/run/docker.sock --network none dockerid/troubleshootingtools:v1
```
This command mounts the Docker socket (/var/run/docker.sock) from the host into the container, allowing the container to interact with Docker commands like docker ps, docker inspect, etc.

2. Portainer for Container Management:
You're setting up Portainer, a management UI for Docker, and mounting the Docker socket so Portainer can communicate with the Docker daemon and monitor containers.

Step 1: Create a volume to store Portainer’s data:


```
docker volume create portainerdata
```
Step 2: Run Portainer container, mounting the Docker socket for communication:


```
docker run -d -p 8000:8000 -p 9443:9443 --name portainer \
  --restart=always \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v portainerdata:/data \
  portainer/portainer-ce:2.11.1
```
This setup allows you to monitor Docker containers through the Portainer UI by accessing http://<host-ip>:9443.

3. Dockerfile Basics and Build Arguments:
You seem to be working with a Dockerfile to create custom Docker images. Here’s a breakdown of some Dockerfile instructions you're using:

FROM: Specifies the base image.
LABEL: Adds metadata to the image (e.g., maintainer).
ENV: Defines environment variables (e.g., AWS credentials).
ARG: Defines build-time arguments. These can be passed during the docker build process.
RUN: Executes commands during image build (e.g., installing software, downloading files).
CMD: Specifies the default command to run when the container starts (e.g., nginx -g "daemon off;" to run Nginx in the foreground).

Example Dockerfile:
Dockerfile
```
FROM ubuntu:latest
LABEL name="saikiran"

# Set environment variables
ENV AWS_ACCESS_KEY_ID=SDFSDFSDFSDFSDFSDFSDFSDF \
    AWS_SECRET_KEY_ID=KYRjcJYtJ/F8MRrlBkq1iq3pUFPm3+XdYwAi9+9v \
    AWS_DEFAULT_REGION=EU-NORTH-1

# Define build-time arguments
ARG T_VERSION='1.6.6' \
    P_VERSION='1.8.0'

# Install dependencies and software
RUN apt update && apt install -y jq net-tools curl wget unzip \
    && apt install -y nginx iputils-ping

# Download and set up Terraform and Packer
RUN wget https://releases.hashicorp.com/terraform/${T_VERSION}/terraform_${T_VERSION}_linux_amd64.zip \
    && wget https://releases.hashicorp.com/packer/${P_VERSION}/packer_${P_VERSION}_linux_amd64.zip \
    && unzip terraform_${T_VERSION}_linux_amd64.zip \
    && unzip packer_${P_VERSION}_linux_amd64.zip \
    && chmod 777 terraform \
    && chmod 777 packer \
    && ./terraform version \
    && ./packer version

# Start Nginx in the foreground
CMD ["nginx", "-g", "daemon off;"]
```

This Dockerfile installs tools like Terraform, Packer, and Nginx. During the build, it uses build arguments for the versions of Terraform and Packer. It also sets up Nginx as the default service for the container.

4. Build Arguments and Passing Them During Build:
To pass arguments when building a Docker image, you can use the --build-arg flag:


```
docker build --build-arg T_VERSION=1.6.6 --build-arg P_VERSION=1.8.0 -t my-custom-image .
```
This passes the T_VERSION and P_VERSION arguments to the Dockerfile during build.

5. Docker Image Not Tagged Correctly (Dangling Images):
If you don't tag an image when building, Docker will create a dangling image (an image with no tag). To avoid this:

Tag the image when building:


```
docker build -t my-image-name .
```
If you forget to tag it, Docker will display it as <none> in the image list (docker images).

6. Understanding Docker Architecture and Interactions:
Docker Client: Sends API requests to the Docker Daemon (e.g., docker ps, docker run).
Docker Daemon: Executes the commands received from the client and interacts with the system to create containers, images, volumes, etc.
Docker Socket: The socket file (/var/run/docker.sock) facilitates communication between the Docker client and daemon. When mounted inside a container, it allows that container to send Docker commands to the host system.
Summary:
Use Docker volumes for persistent data storage (e.g., MongoDB).
Bind mounts are used to mount host directories into containers, especially for things like configuration or monitoring (e.g., Portainer).
Dockerfile is used to automate the process of building custom images, and you can pass build arguments using ARG and --build-arg.
Make sure to tag your images during the build process to avoid dangling images.

key concepts in Docker, including Docker socket mounting, Portainer for container management, and Dockerfile instructions for building custom images. Let me break down the key parts and provide clarifications:

1. Docker Socket Mounting for Container Communication:
When running a Docker container and trying to interact with the Docker daemon inside that container (e.g., by running docker ps inside the container), you need to mount the Docker socket into the container. Without this, the container cannot communicate with the Docker daemon on the host system.

Problem:
When you ran docker exec -it app1  and tried to run docker ps, it couldn't connect to the Docker daemon because the Docker socket wasn't mounted.

Solution:
You mounted the Docker socket to allow the container to interact with the Docker daemon:


```
docker run --rm -d --name app2 -v /var/run/docker.sock:/var/run/docker.sock --network none dockerid/troubleshootingtools:v1
```
This allows app2 to execute Docker commands like docker ps, docker inspect, etc.

2. Portainer Setup:
You’re using Portainer, a web-based UI for Docker, to manage and monitor containers. You mounted the Docker socket in the Portainer container to enable it to monitor the Docker daemon.

Steps:
Create a Docker volume for Portainer's data:

```
docker volume create portainerdata
```
Run the Portainer container:

```
docker run -d -p 8000:8000 -p 9443:9443 --name portainer \
  --restart=always \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v portainerdata:/data \
  portainer/portainer-ce:2.11.1
```
By mounting /var/run/docker.sock, Portainer can now interact with the Docker daemon and manage containers. The logs and configurations are stored in the portainerdata volume.

3. Dockerfile and Build Arguments:
You are using a Dockerfile to create a custom Docker image. Dockerfiles define the instructions needed to build an image (e.g., setting up dependencies, downloading files, configuring software).

Example Dockerfile:
Here's a basic Dockerfile that sets up Terraform, Packer, and Nginx on an Ubuntu image:

Dockerfile
```
FROM ubuntu:latest
LABEL name="saikiran"

# Set environment variables
ENV AWS_ACCESS_KEY_ID=SDFSDFSDFSDFSDFSDFSDFSDF \
    AWS_SECRET_KEY_ID=KYRjcJYtJ/F8MRrlBkq1iq3pUFPm3+XdYwAi9+9v \
    AWS_DEFAULT_REGION=EU-NORTH-1

# Define build-time arguments
ARG T_VERSION='1.6.6' \
    P_VERSION='1.8.0'

# Install dependencies and software
RUN apt update && apt install -y jq net-tools curl wget unzip \
    && apt install -y nginx iputils-ping

# Download and set up Terraform and Packer
RUN wget https://releases.hashicorp.com/terraform/${T_VERSION}/terraform_${T_VERSION}_linux_amd64.zip \
    && wget https://releases.hashicorp.com/packer/${P_VERSION}/packer_${P_VERSION}_linux_amd64.zip \
    && unzip terraform_${T_VERSION}_linux_amd64.zip \
    && unzip packer_${P_VERSION}_linux_amd64.zip \
    && chmod 777 terraform \
    && chmod 777 packer \
    && ./terraform version \
    && ./packer version
```
# Start Nginx in the foreground
CMD ["nginx", "-g", "daemon off;"]
Key Dockerfile Instructions:
FROM: Specifies the base image (e.g., ubuntu:latest).
LABEL: Adds metadata to the image (e.g., maintainer name).
ENV: Defines environment variables (e.g., AWS keys).
ARG: Defines build-time arguments (e.g., versions of Terraform and Packer).
RUN: Executes commands to install dependencies and configure software.
CMD: Defines the default command to run when the container starts (e.g., starting nginx).
4. Build Arguments:
To pass arguments during the build process, use the --build-arg flag with docker build:


```
docker build --build-arg T_VERSION=1.6.6 --build-arg P_VERSION=1.8.0 -t my-custom-image .
```
This allows you to pass values to the ARG instructions in the Dockerfile.

5. Docker Image Tagging and Dangling Images:
When you build an image without tagging it (docker build . without -t), Docker will create a dangling image, which is an image without a name or tag. You can avoid this by specifying a tag when building the image:


```
docker build -t my-image-name .
```
This way, your image will have a proper tag instead of <none>.

Summary of Solutions:
Docker Socket Mounting: Ensure you mount /var/run/docker.sock to allow containers to interact with the Docker daemon.
Portainer Setup: Mount the Docker socket in Portainer to monitor and manage containers through the UI.
Dockerfile: Use Dockerfile instructions like RUN, ARG, ENV, and CMD to automate image creation.
Build Arguments: Pass build-time arguments using --build-arg when building the Docker image.
Tagging Images: Always tag your images to avoid dangling images (<none>).

Docker tasks, from building images with Dockerfile instructions to running containers, configuring environment variables, and performing cleanup tasks. Let's go over a few important points based on what you've described:

1. No Cache Build:
You were trying to build a Docker image without using the cache to ensure that all steps in the Dockerfile are executed from scratch. This is done with the --no-cache option:


```
docker build -t dockerid/custom:v1 -f dockerfile.dev . --no-cache
```
This command prevents Docker from using the cache from previous builds and forces it to rebuild each layer, starting from the top of the Dockerfile.

2. Running Containers and Smoke Testing:
After building the image, you ran it with the following command:


```
docker run --rm -d --name app9 -p 8007:80 dockerid/custom:v1
```
You then entered the container and performed a smoke test by checking the installed versions of terraform and packer:


```
docker exec -it app9 
./terraform version
./packer version
```
Smoke testing ensures that the container is working as expected, and in this case, you confirmed that the installed versions of terraform and packer were correct.

3. Environment Variables (-e Flag):
You passed environment variables to the container using the -e flag, like so:


```
docker run --rm -d --name aap3 -p 8002:80 \
  -e AWS_ACCESS_KEY_ID=SDFSDFSDFSDFSDFSDFSDFS \
  -e AWS_SECRET_KEY_ID=KYRjcJYtJ/F8MRrlBkq1iq3pUFP \
  -e AWS_DEFAULT_REGION=hidden \
  dockerid/custom:v1
```
This ensures that the environment variables are available inside the container. You can verify them by executing the env command inside the running container:


```
docker exec -it aap3 env
```
This will show the environment variables you set using -e, such as AWS_ACCESS_KEY_ID, AWS_SECRET_KEY_ID, and AWS_DEFAULT_REGION.

4. Pruning Docker System:
You used various commands to clean up Docker resources, including stopping and removing containers, pruning unused images, and clearing build cache:

Stop all containers:

```
docker stop $(docker ps -aq)
```
Remove all images:

```
docker rmi $(docker images -a -q)
```
Prune unused Docker resources:

```
docker system prune -a -f
```
The docker system prune command will remove stopped containers, unused networks, dangling images, and build cache, helping free up space on your system.

5. Copy vs. Add in Dockerfile:
You mentioned COPY and ADD, which are both used to copy files or directories into the image, but there are key differences:

COPY: Copies files from the host into the container.
ADD: Similar to COPY, but it also has the ability to handle URLs and automatically extract tar files.
For most cases, COPY is recommended because it's more predictable, while ADD should be used when you need its extra features.

6. Overriding CMD and ENTRYPOINT:
In your Dockerfile, you're using CMD to specify the default command when the container starts. For example:

Dockerfile
```
CMD ["/usr/bin/ping", "-c", "4", "www.google.com"]
```
This command can be overridden when running the container with docker run:


```
docker run --rm dockerid/custom:v1 /bin/
```
In contrast, ENTRYPOINT defines the command that cannot be easily overridden by the user. It's often used for containers that need to execute a single main task.

7. Expose (Documentation) in Dockerfile:
You also mentioned the EXPOSE directive in Dockerfile. It's not a command for configuring port forwarding but rather a documentation feature to tell developers which ports the container is expected to listen on. It doesn't affect the container's behavior unless you explicitly map the container ports to host ports using the -p option when running the container.

Example:

Dockerfile
```
EXPOSE 80
```
This is mainly for informational purposes; the actual port mapping happens at runtime with -p:


```
docker run -p 8000:80 dockerid/custom:v1
```
8. Build Arguments in Dockerfile (--build-arg):
You used the --build-arg flag to pass build-time arguments like version numbers for terraform and packer:


```
docker build -t dockerid/custom:v1 --build-arg T_VERSION='1.5.0' --build-arg P_VERSION='1.4.0' -f dockerfile.dev . --no-cache
```
This allows you to customize the image during the build process by passing dynamic values for these arguments.

Summary:
--no-cache: Forces Docker to rebuild everything from scratch without using cache.
Smoke Testing: Ensures that the container works as expected after build.
Environment Variables: Use -e to pass variables to a container at runtime.
Docker Cleanup: Use commands like docker system prune to clean up unused resources.
CMD vs ENTRYPOINT: CMD can be overridden; ENTRYPOINT is typically not overridden.
EXPOSE: Is for documentation purposes and does not affect container functionality unless combined with -p during docker run.
Build Arguments: Use --build-arg to pass arguments during image build time.


Docker workflow and facing a few challenges, including issues with image caching, environment variables, and overwriting images. Let's break down your steps and address the key points:

1. Building Without Cache
You're trying to build the image from scratch without using any cached layers. To do this, you correctly used the --no-cache flag:


```
docker build -t dockerid/custom:v1 -f dockerfile.dev . --no-cache
```
This command ensures that no previous layers are cached, and everything is rebuilt. If Docker is still using cache, you can ensure that you are cleaning up before rebuilding by running the following to remove unused images and cached layers:


```
docker builder prune -a -f
docker build -t dockerid/custom:v1 -f dockerfile.dev . --no-cache
```
2. Running the Image
Once the image is built, you run it with:


```
docker run --rm -d --name app9 -p 8007:80 dockerid/custom:v1
```
This starts the container in detached mode, mapping port 8007 on your host to port 80 inside the container.

3. Smoketesting the Container
You then execute a command inside the container to verify that it is working correctly:


```
docker exec -it app9 
./terraform version
```
This confirms that the Terraform version is correctly installed, and this process of checking key functionalities is what you referred to as smoke testing.

4. Overwriting Environment Variables
You're overriding environment variables at runtime using the -e flag:


```
docker run --rm -d --name aap3 -p 8002:80 \
  -e AWS_ACCESS_KEY_ID=SDFSDFSDFSDFSDFSDFSDFS \
  -e AWS_SECRET_KEY_ID=KYRjcJYtJ/F8MRrlBkq1iq3pUFP \
  -e AWS_DEFAULT_REGION=hidden \
  dockerid/custom:v1
```
You confirmed the new environment variables were correctly applied by checking inside the container:


```
docker exec -it aap3 env
```
This outputs the environment variables you passed with the docker run command.

5. Removing Unused Images and Containers
You're using various commands to clean up unused images and containers:

Stop all running containers:


```
docker stop $(docker ps -aq)
```
Remove all containers:


```
docker rm $(docker ps -aq)
```
Prune unused images and build cache:


```
docker system prune -a -f
```
This cleans up unnecessary images and containers, which helps you manage disk space effectively.

6. Best Practices for Dockerfile
Based on your Dockerfile examples, here are a few recommendations:

Combining RUN Commands: Instead of having multiple RUN commands for installing packages, you can combine them into one to reduce image layers:

dockerfile
```
RUN apt update && apt install -y jq net-tools curl wget unzip nginx iputils-ping
```
This reduces unnecessary layers in the image.

ENTRYPOINT vs CMD: The CMD instruction can be overridden by passing a command when running the container. If you want to ensure that a specific command is always run, use ENTRYPOINT. For example:

dockerfile
```
ENTRYPOINT ["/usr/bin/ping", "-c", "4", "www.google.com"]
```
This guarantees that when the container is run, the ping command will be executed. You can still override it with the docker run command, but it's the default behavior.

Efficient Layering: Always ensure to clean up after installing packages or doing any heavy work to keep the image size small:

dockerfile
```
RUN apt update && apt install -y jq net-tools curl wget unzip nginx iputils-ping \
    && rm -rf /var/lib/apt/lists/*
```

7. Debugging Build Process with Verbosity
To get a more verbose output of the Docker build process, you are using the --progress=plain flag:


```
docker build -t dockerid/custom:v1 --progress=plain --build-arg T_VERSION='1.5.0' --build-arg P_VERSION='1.4.0' -f dockerfile.dev . --no-cache
```
This will show the output of each step in the Dockerfile and help you identify where the build might be failing or behaving unexpectedly.


1. Why CMD Doesn't Run During Build
You’re correct in noting that the CMD instruction does not execute during the build process. It only runs when you start a container with docker run. So, the ping command in the CMD will only execute when the container is run:

dockerfile
```
CMD ["/usr/bin/ping", "-c4", "www.google.com"]
```
However, during the build process, you can use the RUN instruction to execute commands, such as testing ping:

dockerfile
```
RUN /usr/bin/ping -c 4 www.google.com
```
This will test the ping during the image build phase. If the command succeeds, the build will continue. If it fails (for example, if the image doesn't have network access), the build will stop with an error.

2. Overwriting CMD with CLI
As you've pointed out, CMD can be overwritten when running the container via the CLI. This allows flexibility if you want to override the default command behavior defined in the Dockerfile:


```
docker run hemaboth www.overwrite.com
```
The above command would override the CMD from the Dockerfile, and instead of running the default command (ping www.google.com), it will run ping www.overwrite.com.

3. ENTRYPOINT Can't Be Overwritten (Easily)
The ENTRYPOINT instruction, on the other hand, cannot be easily overwritten when running the container. It specifies the command that is always executed when the container starts. This is useful if you want to ensure a certain command is always run. For example:

dockerfile
```
ENTRYPOINT ["/usr/bin/ping", "-c4", "www.google.com"]
```
Even if you try to override it using the docker run command, the ping command will still be executed, but the argument (www.google.com) can be replaced:


```
docker run hemaboth www.youtube.com
```
This will run the ping command with www.youtube.com as the argument, instead of www.google.com.

4. Best Practices for CMD and ENTRYPOINT
You seem to be experimenting with two strategies for how to structure the Dockerfile. Here's a breakdown of both approaches:

Strategy 1: Combining ENTRYPOINT and CMD
In this approach, you set the ENTRYPOINT to a fixed command (which can't be easily overridden) and use CMD to specify the default arguments for that command. For example:

dockerfile
```
ENTRYPOINT ["/usr/bin/ping"]
CMD ["-c4", "www.google.com"]
```
This ensures that the container always runs ping, but you can override the CMD arguments by passing different ones when you run the container:


```
docker run hemaboth www.youtube.com
```
This flexibility allows you to use default values while still allowing customization at runtime.

Strategy 2: Using CMD to Start Services (e.g., Nginx)
In this case, you want the container to run continuously, serving a service like Nginx. You can use the CMD instruction to specify the command that keeps the process running:

dockerfile
```
CMD ["nginx", "-g", "daemon off;"]
```
This ensures Nginx runs in the foreground, which is necessary for Docker containers to keep running. If Nginx were run in the background (using &), the container would stop immediately after starting because there would be no foreground process keeping it alive.

5. EXPOSE Instruction
The EXPOSE instruction in a Dockerfile is primarily a documentation feature. It tells Docker which ports the container is expected to listen on, but it doesn't actually publish or bind the ports. For example:

dockerfile
```
EXPOSE 80
```
This simply indicates that the container will listen on port 80. When running the container, you will need to bind the port to the host machine explicitly using the -p flag:


```
docker run -p 8000:80 my-container
```

This binds port 8000 on the host to port 80 inside the container. While EXPOSE is helpful for documentation purposes, the actual mapping happens during the docker run step.

6. Using COPY vs. ADD
The COPY and ADD instructions are similar, but they have different behaviors:

COPY: Copies files from your local machine to the image without any additional functionality.
ADD: Has the same behavior as COPY but also supports extracting tar files and fetching files from remote URLs (though it's generally recommended to use COPY unless you specifically need the extra functionality of ADD).
dockerfile
```
COPY ./local_file /container_path/
```

If you want to fetch files from a remote source, use ADD:

dockerfile
```
ADD http://example.com/file.tar.gz /container_path/
```

7. Distroless Images and Multi-Stage Builds
Distroless images: These are images that contain only the application and its runtime dependencies, but not the full operating system. They are more lightweight and secure because they have fewer components that could potentially be exploited.
Multi-stage builds: You can use multiple FROM statements in a Dockerfile to create a build image and a final runtime image, minimizing the size of your final image by excluding unnecessary build tools.
Example:

dockerfile
```
# Build Stage
FROM golang:1.20 as builder
WORKDIR /app
COPY . .
RUN go build -o myapp .

# Runtime Stage
FROM gcr.io/distroless/base
COPY --from=builder /app/myapp /usr/local/bin/myapp
CMD ["/usr/local/bin/myapp"]
```

This way, the final image only contains the built app and necessary runtime libraries, reducing its size and attack surface.

8. Security and User Management
Avoid running containers as the root user. Instead, create a non-root user for better security. You can do this in the Dockerfile:

dockerfile
```
RUN useradd -ms /bin/ myuser
USER myuser
```

This ensures that the container doesn't run as root, which reduces the risk of privilege escalation attacks.

Conclusion
You are making great progress in understanding Docker's behavior and best practices. A few takeaways for building better Dockerfiles:

Use ENTRYPOINT for fixed commands and CMD for default arguments.
For long-running processes like Nginx, use CMD to keep the container running.
Use COPY instead of ADD unless you need specific functionality like fetching files from URLs.
Consider using multi-stage builds and distroless images to reduce the size and increase the security of your images.


1. Development Process
SAST (Static Application Security Testing): Tools like Checkmarx and SonarQube check the code for security vulnerabilities. If any issues are found, the developer fixes them.
Build: After testing, the code is built into artifacts (e.g., WAR or JAR files) using build tools like Maven.
Dockerization: A Dockerfile is created to package the built artifact into a Docker image. This image is pushed to a container registry (like Docker Hub or AWS ECR).
2. Docker Image Concepts
Image Size: Docker images can vary in size, and images without unnecessary tools (like just the runtime) are smaller.
Multi-Stage Docker Build: This optimizes Docker images by separating the build environment from the runtime environment, resulting in smaller and more efficient images.
3. Multi-Stage Docker Build Explanation
Stage 1: Build Stage (Using Maven)
Base Image: You start with a Maven image to build your Spring Boot application.
Steps:
Copy the project files into the container.
Use Maven to build the application (mvn clean package), generating a .jar file.
Output: The built artifact is stored in the /target directory inside the container.
Stage 2: Runtime Stage (Using OpenJDK)
Runtime Image: After building the app, you use a smaller OpenJDK runtime image to run the application.
Steps:
Only the necessary .jar file (produced in Stage 1) is copied from the build container to the runtime container.
Expose port 8080 for external access.
Use the CMD command to run the application using java -jar.
4. Why Multi-Stage Build?
Reduces Image Size: By separating the build and runtime environments, only the necessary files (the .jar file) are included in the final image. The build tools (like Maven) are not included in the runtime image, making the image smaller.
Improves Performance: The final image is lightweight and optimized for deployment, improving container startup time and reducing security risks.
5. Simplified Example Dockerfile

dockerfile
```
# Stage 1: Build stage
FROM maven:3.8.1-openjdk-11-slim AS build
WORKDIR /app
COPY . /app
RUN mvn clean package -DskipTests

# Stage 2: Runtime stage
FROM openjdk:11-jre-slim
WORKDIR /app
COPY --from=build /app/target/my-app.jar /app/my-app.jar
EXPOSE 8080
CMD ["java", "-jar", "/app/my-app.jar"]
```

Explanation:
Stage 1: Uses the Maven image to build the application and produce the .jar file.
Stage 2: Uses the OpenJDK image to run the application. It copies only the necessary .jar file from Stage 1, resulting in a smaller runtime image.
Expose Port 8080: The app is accessible via port 8080.
CMD: Starts the application with java -jar.

6. Docker Commands
Build the Docker Image:

```
docker build -t my-app .
```

Run the Docker Image:

```
docker run -p 8080:8080 my-app
```

7. Benefits of This Approach
Optimized for Production: By using multi-stage builds, your final Docker image is smaller and more secure since it only includes what is necessary to run the app.
Efficient Deployment: With only the runtime dependencies included, the container starts faster and uses fewer resources.

Simplified Overview of the Docker Process with Distroless, Multi-Stage Builds, and Docker Compose
1. Using Distroless Images:
Pulling Distroless Image:

Distroless images are minimal images that do not contain a shell, reducing the image size.
Example: You can use gcr.io/distroless/java for a Java application, which is lightweight but doesn’t allow interactive logging into the container.
Image Size: Distroless images are usually much smaller than full-fledged images with shells.
Issues: Since they lack a shell, you can’t log into the container directly to inspect it or install additional software, making them ideal for production but less flexible during development.

2. Multi-Stage Docker Build:
Multi-stage builds are used to separate the build environment from the runtime environment. This helps to:

Reduce image size: By copying only the necessary artifacts (e.g., .jar file) from the build stage into the runtime image.
Speed up build: You can use more powerful base images in the build stage (e.g., Maven) and then move to a lean runtime image.
Steps to Containerize a Spring Boot Application Using Multi-Stage Builds:
Clone the Project:


```
git clone https://github.com/spring-projects/spring-petclinic.git
cd spring-petclinic
```

Create Dockerfile:

Write a Dockerfile to build the application and then run it in a minimal environment.
dockerfile
```
# Stage 1: Build Stage
FROM maven:3.8.1-openjdk-11-slim AS build
WORKDIR /app
COPY . /app
RUN mvn clean package -DskipTests

# Stage 2: Runtime Stage (Using Distroless or Minimal Runtime Image)
FROM gcr.io/distroless/java:11
WORKDIR /app
COPY --from=build /app/target/my-app.jar /app/my-app.jar
EXPOSE 8080
CMD ["java", "-jar", "/app/my-app.jar"]
```

First Stage: The app is built using Maven, and the artifact (e.g., my-app.jar) is stored in the /target folder inside the container.
Second Stage: A distroless image is used to run the app with the .jar file, keeping the image size small.
Build the Docker Image:


```
docker build -t spring-boot-app .
```
Image Size: After building, the image size will be larger if build tools are included. If using multi-stage builds, the final image will be smaller.
Example Image Size: A typical Spring Boot app with Maven in the build stage may result in an image of around 682 MB.
Run the Image:


```
docker run -p 8080:8080 spring-boot-app
```
Optimize Build with Single Command: Instead of writing multiple commands in the Dockerfile, use && to combine them:

dockerfile
```
RUN apt-get update && apt-get install -y <package>
```

6. Docker Push & Repository Management:
Login to Docker Hub:


```
docker login
```
Enter your username and token (can be created under Docker Hub settings).
Push Image to Repository:


```
docker push spring-boot-app
```
7. Docker Compose:
When you have multiple containers (like frontend, backend, Redis, databases, etc.), Docker Compose helps manage and run them together with a single configuration.

Define Services in docker-compose.yml:

A simple example for a Spring Boot backend with PostgreSQL:

```
version: '3'
services:
  backend:
    image: spring-boot-app
    ports:
      - "8080:8080"
  db:
    image: postgres:13
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: mydb
```

Run All Containers:

To start all services defined in the docker-compose.yml file:

```
docker-compose up
```
Benefits of Docker Compose:

Simplifies Development: Run multiple services with one command.
Consistent Environments: Ensures the same setup for development and production.
Service Dependencies: Docker Compose manages service startup order (e.g., backend first, then database).
Summary:
Distroless Images reduce size and improve security but make debugging difficult.
Multi-Stage Docker Builds separate the build and runtime environments, leading to smaller and more efficient images.
Docker Compose helps manage multi-container applications with a single configuration file, simplifying development and deployment.


Docker Compose with Real-Time Scenario & Kubernetes vs Docker Swarm
1. Docker Compose Example:
Docker Compose simplifies running multi-container applications, such as the voting app scenario you described, by allowing you to define and run all the necessary containers (frontend, backend, Redis, databases) with a single command.

Here's a simple docker-compose.yml example for a voting application that includes frontend, backend, Redis, and two databases:


```
version: '3'
services:
  frontend:
    image: my-frontend-image:v1
    ports:
      - "8001:80"
    depends_on:
      - backend

  backend:
    image: my-backend-image:v1
    ports:
      - "8002:80"
    depends_on:
      - redis
      - db
      - postgresql

  redis:
    image: redis:latest
    ports:
      - "6379:6379"

  db:
    image: my-mem-db-image:v1
    ports:
      - "2801:80"

  postgresql:
    image: postgres:latest
    environment:
      POSTGRES_USER: myuser
      POSTGRES_PASSWORD: mypassword
      POSTGRES_DB: mydb
    ports:
      - "5432:5432"
```

Explanation of Docker Compose File:
version: Defines the version of Docker Compose configuration (v3 in this case).
services: Defines the individual services or containers that make up the application.
frontend, backend, redis, db, postgresql: These are the containers or services defined for the application.
image: Specifies the image to use for each container. You can pull pre-built images from Docker Hub or use custom ones.
ports: Maps ports from the container to the host machine, so you can access services on specific ports.
depends_on: Specifies the dependencies between containers (e.g., frontend depends on backend).
environment: Defines environment variables for services like PostgreSQL (user, password, database name).
Steps to Run the Application Using Docker Compose:
Create the docker-compose.yml file: Save the above configuration in a docker-compose.yml file in your project directory.

Start All Containers: To start all services defined in the docker-compose.yml file, run:


```
docker-compose up
```
This will start all the services (frontend, backend, Redis, and both databases) and link them together based on the dependencies.

Scaling Services: If you want to scale a particular service (e.g., run 3 instances of the backend), you can use:


```
docker-compose up --scale backend=3
```
Stop the Services: To stop and remove all containers, networks, and volumes defined in the Compose file:


```
docker-compose down
```
Docker Compose Benefits:
Single Command: Developers only need to run a single command (docker-compose up) to start all containers.
Environment Variables: Easily pass environment variables like database credentials.
Networking: Docker Compose automatically manages networking, so services can communicate using their container names (e.g., redis, db).
Custom Containers: Define a custom build context for your services if you need custom images.
2. Docker Compose in a Real-Time Scenario:
In real-time environments, developers or testing teams often work with multiple containers running on local machines. Docker Compose simplifies the process of managing multiple services (like Jenkins, PostgreSQL, Redis, etc.) that need to run together daily for testing or development purposes.

Real-World Use Case:
Scenario: You have 6 containers running locally on a developer’s machine. Instead of running each container individually with different commands, Docker Compose can be used to start all containers with a single command.

How It Helps: Docker Compose allows you to define and run multi-container applications, such as:

Jenkins for CI/CD pipelines.
Databases like PostgreSQL and Redis.
Other services like frontend/backend applications.
With a single docker-compose.yml file, a developer can start all required containers for local testing with just one command, making development and testing more efficient.

3. Docker Orchestration with Kubernetes vs Docker Swarm:
While Docker Compose is great for local development and managing multi-container applications on a single machine, in production environments, you might need to use orchestration tools like Kubernetes or Docker Swarm to manage containers across multiple nodes.

Docker Swarm:
What it is: A native Docker orchestration tool.
How it works: Allows you to manage a cluster of Docker nodes, with features like:
Service discovery: Automatically discovers containers in the cluster.
Load balancing: Distributes traffic across containers.
Scaling: You can scale up/down the number of replicas of a container.
Limitations: Docker Swarm lacks some advanced features found in Kubernetes, such as:
Namespaces: Needed for managing multiple applications in a single cluster.
RBAC: Role-Based Access Control for security.
Kubernetes:
What it is: An advanced container orchestration platform.

How it works: Kubernetes provides powerful features for managing containers at scale, such as:

Pods: Grouped containers that share networking and storage resources.
Services: Expose container applications to the network.
Scaling & Auto-scaling: Scale your applications up/down based on load.
Fault Tolerance: Kubernetes ensures that containers are always running and can recover from failures.
Why use Kubernetes?:

Advanced features: Namespaces, RBAC, and fault tolerance.
Production-grade: Widely used in production environments due to its robustness.
Scalability: Efficiently handles large-scale applications.
Conclusion:
Docker Compose is perfect for local development and testing, where you need to run multiple containers with a single command.
Docker Swarm and Kubernetes are used in production environments for container orchestration across multiple nodes, with Kubernetes being the preferred choice for large-scale applications.

This guide will walk you through setting up Docker Swarm and deploying services in a real-world scenario.

Docker Swarm Overview
Docker Swarm is a native clustering and orchestration tool for Docker. It allows you to manage a group of Docker engines (multiple machines or VMs) as one virtual system. With Docker Swarm, you can scale your applications, balance load, and handle container orchestration with minimal effort.

Setup and Management Workflow
Initial Setup (Preparing Nodes)

You need multiple instances (either machines or virtual machines) to set up Docker Swarm. In this case, we'll create 6 instances.
3 nodes will act as manager nodes and 3 nodes will act as worker nodes.
Installing Docker on All Nodes: First, ensure that Docker is installed on all your nodes. If Docker isn’t installed, you can follow these steps on each node:


```
sudo apt-get update
sudo apt-get install docker.io
```

Set Up Docker Swarm Cluster:

Initialize Docker Swarm on the First Node: On the first node (we'll call this the master node), you run the following command:


```
docker swarm init
```
This command makes the first node the manager node in your Docker Swarm cluster.

Join Worker Nodes: After initializing the master node, you will get a join token that allows other nodes to join as worker nodes. The command will look like this:


```
docker swarm join --token <worker-token> <master-ip>:<master-port>
```
Run this on the 3 worker nodes, replacing <worker-token> with the actual token and <master-ip> with the IP address of the master node.

Join Additional Manager Nodes: If you want to add more manager nodes (you typically want 3 or 5 managers for fault tolerance), use the following command on the other two manager nodes:


```
docker swarm join --token <manager-token> <master-ip>:<master-port>
```
This will make them manager nodes in the swarm.

Verify the Setup:

After all nodes are joined, run the following command on the master node to verify the status of the Swarm:


```
docker info
```
This will display information about the Swarm and the nodes that are part of it.

To list all nodes in your Swarm, run:


```
docker node ls
```
To view the networks available in the Swarm:


```
docker network ls
```
Deploying Services in Docker Swarm
Create a Service: Now that the Swarm is set up, you can deploy services across the cluster.

Deploy a Sample Service: You can create a service that will run on multiple nodes in the Swarm. For example, to deploy a service called app1 with 3 replicas:


```
docker service create --name app1 --replicas 3 --publish 8000:80 dockerid/rollingupdate:v10
```
--replicas 3: This means that Docker will run 3 copies (replicas) of this service.
--publish 8000:80: This publishes the container’s port 80 to the host’s port 8000.
dockerid/rollingupdate:v10: The image to run.
Scaling Services:

Scale up: If you want to increase the number of replicas (i.e., run more instances of the service):


```
docker service scale app1=6
```
Scale down: If you want to reduce the number of replicas:


```
docker service scale app1=1
```
Deploy Services Only on Worker Nodes: If you want a service to run only on worker nodes, use the --constraint option to specify the node role (worker):


```
docker service create --name app1 --constraint node.role==worker --replicas 6 --publish 8000:80 dockerid/rollingupdate:v10
```
Global Services: To deploy a service on every node in the Swarm (including all worker and manager nodes), use the --mode global option:


```
docker service create --name monitor --publish 9100:9100 --mode global prom/node-exporter
docker service create --name cadvisor --publish 8888:8080 --mode global google/cadvisor:latest
```
Managing Services and Nodes
Check Service Tasks: You can check the status of tasks running for a service with:


```
docker service ps app1
```
Put a Node in Maintenance Mode: If you want to drain a node (so it stops running tasks), use:


```
docker node update <node-id> --availability drain
```
Reactivate a Node: If you want to bring a node back to active status, use:


```
docker node update <node-id> --availability active
```
Load Balancing in Docker Swarm
Docker Swarm automatically handles load balancing across services. If you run multiple replicas of a service, the traffic is distributed among the available replicas.

To see load balancing in action, you can run a curl command to test how traffic is distributed across the nodes:


```
while true; do curl -sL http://<ip>:8000/ | grep -I 'IP-A'; sleep 1; done
```
This will show which IP addresses are handling the requests.

Visualizing the Cluster
To visualize the Swarm cluster, you can run a Swarm Visualizer Docker container. It shows your entire Docker Swarm architecture in a graphical interface:


```
docker run -it -d -p 8080:8080 --name swarm-visualizer -e HOST=<manager-ip> -e PORT=<manager-port> dockersamples/visualizer
```
After running this, you can access the visualizer at http://<manager-ip>:8080.

Conclusion
With Docker Swarm, you can easily manage a cluster of Docker containers, scale services, and ensure fault tolerance. By using Swarm's built-in tools and commands, you can deploy and manage applications in a distributed environment efficiently.

Docker Swarm is a great tool for container orchestration, especially in smaller to medium-sized environments.
It allows you to manage multiple containers across multiple hosts with ease, ensuring scalability, availability, and fault tolerance.
