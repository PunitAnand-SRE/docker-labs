# Lab 01 – Docker Installation on Azure Ubuntu VM 🐳

## 🎯 Objective
Set up Docker Engine on an Ubuntu virtual machine running in Microsoft Azure and verify the installation using official Docker validation commands.

This lab focuses on installing Docker the **recommended way** using Docker’s official convenience script and validating the setup end-to-end.

---

## 🧰 Environment Details

- **Cloud Provider:** Microsoft Azure  
- **VM OS:** Ubuntu (Azure image)  
- **VM Access:** SSH via Public IP from local machine  
- **Docker Install Method:** Official Docker convenience script  
- **Reference:** https://docs.docker.com/engine/install/ubuntu/

---

## 🛠️ Steps Performed

### 1️⃣ Azure VM Creation & SSH Access
- Created an Ubuntu VM in Azure
- Enabled SSH (port 22) via Network Security Group
- Connected from local machine using VM public IP

---

### 2️⃣ Remove Any Existing / Conflicting Container Packages
To avoid conflicts with older or distro-provided container runtimes, attempted removal of existing packages:

```bash
sudo apt remove $(dpkg --get-selections docker.io docker-compose docker-compose-v2 docker-doc podman-docker containerd runc | cut -f1)
````

> No existing Docker or container runtime packages were found, confirming a clean environment.

---

### 3️⃣ Download Docker Installation Script

Downloaded Docker’s official installation script:

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
```

---

### 4️⃣ Install Docker Engine

Executed the script to install Docker Engine, CLI, containerd, and required plugins:

```bash
sudo sh get-docker.sh
```

This automatically:

* Added Docker’s official GPG key
* Configured Docker APT repository
* Installed Docker Engine and dependencies
* Enabled and started the Docker service using `systemd`

📸 **Docker installation and service startup output:**

![Docker installation using convenience script](https://raw.githubusercontent.com/PunitAnand-SRE/docker-labs/main/lab-01-docker-installation-on-azure-vm/screenshots/01-docker-installation-using-convenience-script.jpg)

---

### 5️⃣ Verify Docker Installation

Checked Docker client and server versions:

```bash
sudo docker version
```

---

### 6️⃣ Validate Docker Functionality

Ran Docker’s official test container to confirm the installation:

```bash
sudo docker run hello-world
```

This verifies:

* Docker daemon is running
* Client can communicate with daemon
* Images can be pulled from Docker Hub
* Containers can be created and executed

📸 **Docker version output and successful hello-world container execution:**

![Docker version and hello-world verification](https://raw.githubusercontent.com/PunitAnand-SRE/docker-labs/main/lab-01-docker-installation-on-azure-vm/screenshots/02-docker-version-and-hello-world-verification.png)

---

## ✅ Outcome

* Docker Engine installed successfully on Azure Ubuntu VM
* Docker service running and managed by systemd
* Container runtime (`containerd`, `runc`) working correctly
* Docker verified using `hello-world` image

---

## 📘 Key Learnings

* Importance of removing conflicting container packages before installation
* Using Docker’s official convenience script for clean, supported installs
* Understanding Docker client ↔ daemon interaction
* Verifying container runtime health using `hello-world`

---

## 👤 Author

**Punit Anand**
SRE Aspirant | Monitoring & Observability → SRE
GitHub: [https://github.com/PunitAnand-SRE](https://github.com/PunitAnand-SRE)

```
```

