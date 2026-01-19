# Lab 02 – Basic Docker Commands & Container Lifecycle 🐳

## 🎯 Objective
Understand and practice **core Docker commands** related to:
- Running containers
- Managing container lifecycle
- Listing, stopping, and removing containers
- Pulling and removing images
- Executing commands inside running containers
- Understanding common Docker errors

This lab focuses on **how Docker actually behaves**, not just ideal scenarios.

---

## 🧰 Environment Details

- **Cloud Provider:** Microsoft Azure  
- **VM OS:** Ubuntu 24.04 LTS  
- **Docker Version:** Latest (installed via official Docker script)
- **Shell:** Bash

---

## 🧠 Core Docker Concepts Covered

| Concept | Explanation |
|------|------------|
| Image | Read-only template used to create containers |
| Container | Running (or stopped) instance of an image |
| Registry | Central place to store images (Docker Hub) |
| Detached Mode | Run container in background |
| Foreground Mode | Run container attached to terminal |

---

## 🚀 Docker Commands Practiced

---

## 1️⃣ Running a Container (`docker run`)

### Example: Run NGINX container
```bash
docker run nginx
````

What happens internally:

1. Docker checks if `nginx:latest` exists locally
2. If not found → pulls from Docker Hub
3. Creates a container
4. Starts the container

---

### ❌ Error: Image Not Found (CentOS)

```bash
docker run centos
```

**Error Reason:**

* `centos:latest` is no longer available on Docker Hub

✅ **Correct alternatives:**

```bash
docker run quay.io/centos/centos:stream9
docker run rockylinux:9
docker run almalinux:9
```

---

## 2️⃣ Detached Mode (`-d`)

Run container in background:

```bash
docker run -d nginx
```

✔ Container runs without blocking terminal
✔ Returns container ID

---

## 3️⃣ List Containers

### Running containers only

```bash
docker ps
```

### All containers (running + stopped)

```bash
docker ps -a
```

Key learning:

> Docker keeps stopped containers unless explicitly removed.

---

## 4️⃣ Stop a Container (`docker stop`)

```bash
docker stop <container_name_or_id>
```

Example:

```bash
docker stop fervent_bohr
```

✔ Gracefully stops the container
✔ Container still exists (stopped state)

---

## 5️⃣ Remove Containers (`docker rm`)

```bash
docker rm <container_id_or_name>
```

Examples:

```bash
docker rm b0064
docker rm upbeat_hofstadter
docker rm 01f 6471
```

📌 **Important Rule**

> A container must be **stopped** before it can be removed.

---

## 6️⃣ List Images (`docker images`)

```bash
docker images
```

Shows:

* Repository name
* Tag
* Image ID
* Disk usage

---

## 7️⃣ Remove Images (`docker rmi`)

```bash
docker rmi nginx
```

### ❌ Error: Image in Use

```text
conflict: unable to delete image (must be forced)
```

📌 **Key Rule**

> ❗ You **cannot delete an image** if **any container (running or stopped)** is using it.

### ✅ Correct Workflow

```bash
docker ps -a        # Find containers using image
docker rm <id>      # Remove containers
docker rmi nginx    # Remove image
```

---

## 8️⃣ Pull Images Explicitly (`docker pull`)

```bash
docker pull nginx
docker pull ubuntu
```

Difference:

* `docker pull` → only downloads image
* `docker run` → pull + create + start container

---

## 9️⃣ Running Ubuntu Containers

### ❌ Incorrect Command

```bash
docker run -d ubuntu 100
```

**Why it failed:**

* Docker tried to execute `100` as a command
* No such executable exists

### ✅ Correct Command

```bash
docker run -d ubuntu sleep 100
```

✔ Runs container for 100 seconds
✔ Keeps container alive

---

## 🔍 Executing Commands Inside Containers (`docker exec`)

```bash
docker exec <container_id> <command>
```

Example:

```bash
docker exec fb71f0b6fef7 cat /etc/*release*
```

✔ Confirms OS inside container
✔ Shows containers are isolated environments

---

## 🧪 Foreground vs Background Containers

### Foreground (attached)

```bash
docker run nginx
```

* Logs visible
* CTRL+C stops container

### Background (detached)

```bash
docker run -d nginx
```

* Runs silently
* Managed using `docker ps`, `stop`, `rm`

---

## 🏷️ Common Docker Flags

| Flag     | Meaning                       |
| -------- | ----------------------------- |
| `-d`     | Detached mode                 |
| `-p`     | Port mapping (host:container) |
| `--name` | Custom container name         |
| `-it`    | Interactive terminal          |
| `--rm`   | Auto-remove container on exit |

### Example:

```bash
docker run -it --rm ubuntu bash
```

---

## ❌ Common Mistakes Observed (Good Learning!)

* Typo in command (`docler` instead of `docker`)
* Trying to delete images before containers
* Running containers without valid commands
* Assuming `latest` tag always exists

---

## ✅ Outcome

✔ Understood Docker image vs container relationship
✔ Practiced full container lifecycle
✔ Learned why Docker throws specific errors
✔ Gained confidence with real-world Docker usage

---

## 📘 Key Learnings (SRE Perspective)

* Containers are disposable; images are reusable
* Docker errors are informative — read them carefully
* Cleanup (`docker rm`, `docker rmi`) is essential in production
* Detached mode is preferred for services
* Always validate running containers

---

## 🚀 Next Labs

* Port mapping & accessing containers via browser
* Docker logs & inspection
* Dockerfile basics
* Volumes & persistent storage
* Docker networking

---

## 👤 Author

**Punit Anand**
SRE Aspirant | Docker | Linux | Cloud
GitHub: [https://github.com/PunitAnand-SRE](https://github.com/PunitAnand-SRE)

```

