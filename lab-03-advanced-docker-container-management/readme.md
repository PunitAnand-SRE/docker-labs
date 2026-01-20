# 🧪 Lab 03: Advanced Docker Container Management

This lab focuses on **advanced Docker runtime concepts** required to run real-world applications inside containers.
Instead of basic image execution, this lab explains **how and why containers are run in a certain way**.

---

## 🎯 What This Lab Covers

* Running containers with **specific image tags**
* Understanding **attached vs detached mode**
* Running long-lived services in **detached mode**
* Exposing container applications using **port mapping**
* Difference between **internal container access** and **host access**
* Persisting data using **volume mapping**
* Verifying data persistence across container restarts

---

## 🏷️ Running Containers Using Image Tags

Docker images are versioned using **tags**.

If no tag is provided, Docker defaults to `latest`, which may change over time.

### Example

```bash
docker run ubuntu:17.10 cat /etc/*release*
```

### Why this is important

* Ensures the **exact OS version** is used
* Avoids unexpected behavior from newer images
* Critical for debugging and production consistency

---

## 🔗 Attached Mode vs Detached Mode

### 🔹 Attached Mode (Foreground)

```bash
docker run ubuntu sleep 15
```

**What happens**

* Container runs in the foreground
* Terminal is blocked for 15 seconds
* You cannot run other commands

**When to use**

* Short tasks
* Debugging
* Learning container behavior

---

### 🔹 Detached Mode (Background)

```bash
docker run -d ubuntu sleep 1500
```

**What happens**

* Container runs in the background
* Terminal is immediately available
* Container continues running independently

You can verify:

```bash
docker ps
```

**Why detached mode is needed**

* Required for services like Jenkins, databases, APIs
* Prevents terminal blocking
* Enables multiple containers to run simultaneously

---

## 🌐 Port Mapping – Accessing Jenkins from Browser

Containers have their **own internal network**.
Applications running inside containers **cannot be accessed externally by default**.

### ❌ Without Port Mapping

* Jenkins runs inside container
* Browser cannot access it
* Application is isolated

---

### ✅ With Port Mapping

#### Command Used

```bash
docker run -p 8080:8080 jenkins/jenkins
```

### Command Breakdown

* `-p` → Enables port mapping
* `8080` (left) → Host machine port
* `8080` (right) → Container port where Jenkins runs
* `jenkins/jenkins` → Jenkins Docker image

This means:

> Accessing `http://<host-ip>:8080` → forwards traffic to Jenkins inside the container

---

### 📸 Jenkins UI Accessible via Port Mapping

![Jenkins Port Mapping](https://raw.githubusercontent.com/PunitAnand-SRE/docker-labs/main/lab-03-advanced-docker-container-management/screenshots/01-jenkins-port-mapping-and-ui-access.png)

---

## 🌍 Container Access: Internal IP vs Port Mapping

### Internal IP Access

* Works only inside Docker network
* IP changes after container restart
* Not suitable for browser access or production

### Port Mapping (Recommended)

* Stable access via host IP
* Works after restarts
* Industry best practice

✅ This lab uses **port mapping**, not internal IP.

---

## 💾 Volume Mapping – Persisting Jenkins Data

By default, **container data is temporary**.

If the container stops or is removed:

* Jenkins jobs
* Plugins
* Configuration
  ❌ **All data is lost**

---

### ✅ Volume Mapping Solution

#### Command Used

```bash
docker run -p 8080:8080 \
-v /root/my-jenkins-data:/var/jenkins_home \
-u root \
jenkins/jenkins
```

---

### Command Breakdown

* `-p 8080:8080` → Exposes Jenkins UI
* `-v /root/my-jenkins-data:/var/jenkins_home`

  * `/root/my-jenkins-data` → Host directory
  * `/var/jenkins_home` → Jenkins data directory inside container
* `-u root` → Runs container as root (for volume permission handling)
* `jenkins/jenkins` → Jenkins image

---

### Why Volume Mapping Is Needed

* Data survives container stop/start
* Jenkins setup is preserved
* Enables backup and migration
* Follows stateless container design

---

### 📸 Volume Mapping for Jenkins Data

![Jenkins Volume Mapping](https://raw.githubusercontent.com/PunitAnand-SRE/docker-labs/main/lab-03-advanced-docker-container-management/screenshots/02-jenkins-volume-mapping-for-data-persistence.png)

---

## ✅ Volume Mapping Verification

After:

1. Stopping the container
2. Restarting Jenkins
3. Opening Jenkins UI again

All Jenkins data was still present.

This confirms:

* Data is stored on host
* Container lifecycle does not affect data
* Volume mapping is working correctly

---

### 📸 Volume Mapping Verification

![Volume Mapping Verification](https://raw.githubusercontent.com/PunitAnand-SRE/docker-labs/main/lab-03-advanced-docker-container-management/screenshots/03-jenkins-volume-mapping-verification.png)

---

## 🧠 Key Learnings from This Lab

* Use **detached mode** for services
* Always prefer **specific image tags**
* Use **port mapping** for external access
* Use **volume mapping** for persistent data
* Containers should be stateless, **data should not**




