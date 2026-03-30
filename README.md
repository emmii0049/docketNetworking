
# Container Networking and Security Lab (Codespaces Edition)

## Overview
In this lab, you will:
- Run Docker containers
- Explore container networking
- Inspect container IP addresses
- Test connectivity between containers
- Deploy a web server (nginx)
- Validate your work using an automated check script

---

## Step 0: Open in Codespaces

1. Fork this [repository](https://github.com/dipaish/docketNetworking) to your GitHub account.
2. Navigate to your forked repository and select:
   **Code → Codespaces → Create codespace**
3. Wait for the environment to start.

> **Note:** Docker is not pre-installed in this Codespace. If you need Docker, install it manually using the official instructions for Ubuntu 22.04, or use Codespaces' built-in Docker support if available. The automated check script will skip Docker checks if Docker is not installed.

---

## Step 1: Verify Docker

```bash
docker --version
docker ps
```

---

## Step 2: Pull Ubuntu Image

```bash
docker pull ubuntu:focal
docker images
```

---

## Step 3: Create a Custom Bridge Network

```bash
docker network create --driver bridge csf-net
docker network ls
```

**Why?**
> A user-defined bridge network (csf-net) allows containers to communicate securely and predictably. Unlike the default bridge, it provides better network isolation and lets you control which containers can talk to each other. This is important for container security, as it limits exposure and enables network segmentation—key principles in secure container deployments.

---

## Step 4: Run the First Container (on the custom network)

```bash
docker run -it -d --name csf-ubuntu1 --network csf-net ubuntu:focal
```

---

## Step 5: Run the Second Container (on the custom network)

```bash
docker run -it -d --name csf-ubuntu2 --network csf-net ubuntu:focal
```

---

## Step 6: Get Container IP Address

```bash
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' csf-ubuntu1
```
Save this IP address for later use.

---

## Step 7: Access the Second Container’s Shell

```bash
docker exec -it csf-ubuntu2 /bin/bash
```

---

## Step 8: Update Packages

```bash
apt-get update
```

---

## Step 9: Install Ping

```bash
apt-get install iputils-ping -y
```

---

## Step 10: Test Connectivity

```bash
ping <IP_ADDRESS>
```
Replace `<IP_ADDRESS>` with the value from Step 5. You should see successful replies.

Exit the container shell with:
```bash
exit
```

---

## Step 11: Run the Nginx Web Server

```bash
docker run -d -p 8080:80 --name csf-nginx nginx:latest
```

---

## Step 12: Access the Web Server

- Go to the Ports tab in Codespaces.
- Open port 8080.
- You should see: "Welcome to nginx!"

---

## Step 13: Check Your Work and Get Your Marksheet

Run the check script:
```bash
bash check.sh
```
This will generate a `marksheet.md` file with your GitHub username and a summary of your lab checks. Your instructor will use this for grading.