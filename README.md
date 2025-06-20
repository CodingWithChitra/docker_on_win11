## 🐳 Docker on Windows 11 - Step-by-Step Tutorial

### 📘 Introduction to Docker

**Docker** is an open platform for developing, shipping, and running applications. It uses **containers**—lightweight, portable environments—to ensure your application behaves the same across all systems.

With Docker, you can:

* Avoid "it works on my machine" problems.
* Run multiple apps with isolated environments.
* Easily share, test, and deploy your apps.

---

## 🧑‍💻 Tutorial Objective

This guide will walk you through installing Docker on Windows 11 and running a **Spring Boot Hello World** app using Docker.

---

## ✅ Prerequisites

* Windows 11 (Home or Pro)
* Internet connection
* Basic knowledge of Java and Spring Boot

---

## 🔧 Step 1: Enable WSL2 and Virtualization

1. **Open PowerShell as Administrator** and run:

   ```powershell
   dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
   dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
   ```

2. **Download WSL2 kernel update package**:
   [https://aka.ms/wsl2kernel](https://aka.ms/wsl2kernel)

3. Set WSL2 as the default:

   ```powershell
   wsl --set-default-version 2
   ```

4. **Restart** your computer.

---

## 🐳 Step 2: Install Docker Desktop

1. Go to [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)

2. Download and install Docker Desktop for Windows.

3. During installation:

   * Choose **WSL2** as the backend.
   * Complete the installation and restart if required.

4. **Sign in to Docker Hub** (create an account if needed).

---

## 🔍 Step 3: Verify Docker Installation

Open **PowerShell** or **Terminal**:

```bash
docker --version
```

You should see something like:

```
Docker version 24.x.x, build abcdefg
```

Then run the test image:

```bash
docker run hello-world
```

---

## ☕ Step 4: Create a Spring Boot Hello World App

1. Go to [https://start.spring.io](https://start.spring.io)

2. Generate a Spring Boot project:

   * Language: Java
   * Dependencies: Spring Web
   * Group: `com.example`
   * Artifact: `helloworld`

3. Extract and open it in your IDE (e.g. IntelliJ or VS Code).

4. Replace the main controller:

```java
// src/main/java/com/example/helloworld/HelloController.java

package com.example.helloworld;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {
    @GetMapping("/")
    public String hello() {
        return "Hello from Docker + Spring Boot!";
    }
}
```

5. Build the JAR:

```bash
./mvnw clean package
```

---

## 📦 Step 5: Create a Dockerfile

Inside your project root, create a file named `Dockerfile`:

```Dockerfile
# Use the official OpenJDK 17 image from Docker Hub as the base image.
# This image contains the Java runtime environment required to run your Spring Boot app.
FROM openjdk:17

# Declare a build-time variable called JAR_FILE.
# It points to the compiled JAR file in the target directory (generated by Maven).
ARG JAR_FILE=target/*.jar

# Copy the JAR file from the host machine (your local system) into the container,
# and rename it as app.jar inside the container.
COPY ${JAR_FILE} app.jar

# Set the command that will be executed when the container starts.
# It tells the container to run the JAR file using the java -jar command.
ENTRYPOINT ["java", "-jar", "/app.jar"]

```

---

## 🏗️ Step 6: Build and Run the Docker Image

1. Build the image:

```bash
docker build -t springboot-hello .
```

2. Run the container:

```bash
docker run -p 8080:8080 springboot-hello
```

3. Open a browser and visit:

```
http://localhost:8080
```

You should see:

```
Hello from Docker + Spring Boot!
```

---

## 🧹 Step 7: Docker Cleanup (Optional)

```bash
# Stop all containers
docker stop $(docker ps -q)

# Remove all containers
docker rm $(docker ps -a -q)

# Remove all images
docker rmi $(docker images -q)
```

---

## 📁 Suggested Project Structure

```plaintext
docker-springboot-tutorial/
├── README.md
├── Dockerfile
├── pom.xml
├── src/
│   └── main/
│       └── java/
│           └── com/example/helloworld/
│               └── HelloController.java
└── screenshots/
```

---

## 📌 Conclusion

You’ve successfully:

* Installed Docker on Windows 11
* Built and containerised a Spring Boot app
* Ran it using Docker

✅ Now you’re ready to deploy and scale your Java apps in Dockerised environments!

---
