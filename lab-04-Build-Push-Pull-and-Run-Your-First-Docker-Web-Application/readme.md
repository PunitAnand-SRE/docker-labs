# 🐳 Lab 04: Build, Push, Pull, and Run Your First Docker Web Application

## 📌 Lab Objective

In this lab, we will build a **simple web application using Docker**, push the Docker image to **Docker Hub**, and then **pull and run it again** to verify that it works end-to-end.

By completing this lab, you will clearly understand:

* How to create a Docker image using **Dockerfile**
* How **port mapping** works in Docker
* How to **push images to Docker Hub**
* How to **pull and run images from Docker Hub**
* How Docker enables **application portability**

This is a **foundational Docker lab** and an important milestone before moving to advanced container management.

---

## 🧠 What We Are Building

A simple **NGINX-based web application** that displays a static HTML page in the browser:

> 🚀 *Docker is working!*
> This page is running inside a Docker container.

---

## 📂 Project Structure

All commands in this lab are executed from the following directory unless mentioned otherwise:

```
docker-web-demo/
├── Dockerfile
└── index.html
```

---

## 🔹 Step 1: Create a Simple Web Application

### 📍 Location

Run the following commands from your **home directory** or any working directory.

### Create project directory

```bash
mkdir docker-web-demo
cd docker-web-demo
```

### Create HTML file

```bash
nano index.html
```

Paste the following content:

```html
<!DOCTYPE html>
<html>
<head>
  <title>My First Docker Image</title>
</head>
<body>
  <h1>🚀 Docker is working!</h1>
  <p>This page is running inside a Docker container.</p>
</body>
</html>
```

💡 **Why this step is important**

* This file proves that our application is actually running **inside a container**
* Any change here will require rebuilding the Docker image

---

## 🔹 Step 2: Create Dockerfile (Image Definition)

### 📍 Location

Make sure you are inside the `docker-web-demo` directory.

### Create Dockerfile

```bash
nano Dockerfile
```

Paste the following:

```dockerfile
# Use official NGINX image
FROM nginx:latest

# Copy custom HTML file to NGINX web root
COPY index.html /usr/share/nginx/html/index.html

# Expose port 80
EXPOSE 80
```

### 🧩 Dockerfile Explanation

| Line                | Purpose                                   |
| ------------------- | ----------------------------------------- |
| `FROM nginx:latest` | Uses official NGINX base image            |
| `COPY`              | Copies our HTML page into the container   |
| `EXPOSE 80`         | Documents that the app listens on port 80 |

📌 **No CMD needed**
The official NGINX image already starts NGINX automatically.

---

## 🔹 Step 3: Build Docker Image

### 📍 Location

Inside `docker-web-demo` directory.

### Build the image

```bash
docker build -t punitanandsre/docker-web-demo:1.0 .
```

### What happens internally?

* Docker reads the `Dockerfile`
* Pulls `nginx:latest` (if not already present)
* Creates a new image with your HTML page

### Verify image

```bash
docker images
```

You should see:

```
punitanandsre/docker-web-demo   1.0
```

---

## 🔹 Step 4: Run Container & Verify Port Mapping

### Run container

```bash
docker run -d -p 8080:80 --name web-demo punitanandsre/docker-web-demo:1.0
```

### 🧩 Flag Explanation

| Flag         | Meaning                        |
| ------------ | ------------------------------ |
| `-d`         | Run container in background    |
| `-p 8080:80` | Map host port → container port |
| `--name`     | Assign readable container name |

### 🌐 Browser Verification

Open browser and visit:

```
http://4.213.202.238:8080
```

### ✅ Expected Output

![Docker Web App Port Mapping](https://raw.githubusercontent.com/PunitAnand-SRE/docker-labs/main/lab-04-Build-Push-Pull-and-Run-Your-First-Docker-Web-Application/screenshots/01-docker-web-app-port-mapping-browser-access.png)

✔ Confirms:

* Container is running
* Port mapping is working
* App is accessible externally

---

## 🔹 Step 5: Login to Docker Hub

```bash
docker login
```

Enter:

* Docker Hub Username: `punitanandsre`
* Password / Access Token

💡 Required before pushing images to Docker Hub.

---

## 🔹 Step 6: Push Image to Docker Hub

### Push image

```bash
docker push punitanandsre/docker-web-demo:1.0
```

### What happens?

* Docker uploads image layers
* Image becomes available globally (public repo)

### Verification Screenshot

![Docker Image Push](https://raw.githubusercontent.com/PunitAnand-SRE/docker-labs/main/lab-04-Build-Push-Pull-and-Run-Your-First-Docker-Web-Application/screenshots/02-docker-image-push-to-docker-hub.png)

---

## 🔹 Step 7: Proof Test – Pull & Run from Docker Hub

This step proves **image portability**.

### Stop & remove existing container

```bash
docker rm -f web-demo
```

### Remove local image

```bash
docker rmi punitanandsre/docker-web-demo:1.0
```

### Pull image from Docker Hub

```bash
docker pull punitanandsre/docker-web-demo:1.0
```

### Run container again

```bash
docker run -d -p 8080:80 punitanandsre/docker-web-demo:1.0
```

### Final Verification

![Docker Pull and Run Verification](https://raw.githubusercontent.com/PunitAnand-SRE/docker-labs/main/lab-04-Build-Push-Pull-and-Run-Your-First-Docker-Web-Application/screenshots/03-docker-image-pull-and-container-run-verification.png)

✔ Confirms:

* Image successfully pulled
* App runs without local build
* Docker enables true portability

---

## 🎯 Key Learnings from This Lab

* Docker images are **immutable and portable**
* Port mapping connects **host → container**
* Docker Hub acts as a **central image registry**
* Same image runs anywhere: VM, cloud, laptop

Just tell me what’s next 👌

