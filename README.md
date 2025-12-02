# 🐳 Docker Environment Variables Lab

In this lab, I learned how to **inspect environment variables inside containers, run containers with custom environment settings, and deploy a MySQL container with a root password configured via environment variables**.

---

## 📋 Lab Overview

**Goal:**

* Inspect environment variables inside a running container
* Set environment variables during container creation
* Deploy a MySQL database container with the correct password variable
* Verify environment variables using `docker inspect` and `docker exec`

**Learning Outcomes:**

* Use `docker inspect` to view environment variables
* Use `-e` / `--env` to set environment variables when running containers
* Publish ports with `-p HOST:CONTAINER`
* Deploy MySQL with required Docker Hub environment variables
* Validate environment variable settings inside a container

---

## 🛠 Step-by-Step Journey

### **Step 1: Inspect Existing Container Environment Variables**

**Command:**

```bash
docker ps
```

* Identified the running container:
  `kodekloud/simple-webapp`

**Inspect environment variables:**

```bash
docker inspect <container_id>
```

* Located value of `APP_COLOR`
* `APP_COLOR = pink`

---

### **Step 2: Run a New Container With Environment Variable `APP_COLOR=blue`**

**Task:**
Run a container named **blue-app** from `kodekloud/simple-webapp` and set:

* `APP_COLOR=blue`
* Host port **8282** → container port **8080**

**Command:**

```bash
docker run -d \
  -p 8282:8080 \
  -e APP_COLOR=blue \
  --name blue-app \
  kodekloud/simple-webapp
```

✔️ Application loads successfully
✔️ Background color is **blue**
✔️ Page output: *Hello from <container-id>*

---

### **Step 3: Deploy a MySQL Database With Root Password Set**

**Goal:**
Run a container named **mysql-db** using the `mysql` image and set root password:

```
DB_PASS123
```

**Step 3.1 – Identify correct environment variable from Docker Hub**

* On Docker Hub → MySQL image documentation
* Correct variable is:

```
MYSQL_ROOT_PASSWORD
```

---

### **Step 4: Run the MySQL Container**

**Command:**

```bash
docker run -d \
  -e MYSQL_ROOT_PASSWORD=DB_PASS123 \
  --name mysql-db \
  mysql
```

✔️ MySQL container created
✔️ Password configured correctly

---

### **Step 5: Verify the Environment Variable**

**Command:**

```bash
docker exec -it mysql-db env
```

* Found the variable:

```
MYSQL_ROOT_PASSWORD=DB_PASS123
```

✔️ Verified password successfully
✔️ End of lab

---

## ✅ Key Commands Summary

| Task                                | Command / Notes                                      |
| ----------------------------------- | ---------------------------------------------------- |
| List running containers             | `docker ps`                                          |
| Inspect container for env variables | `docker inspect <id>`                                |
| Run container with env variable     | `docker run -e APP_COLOR=blue IMAGE`                 |
| Publish port                        | `-p HOST:CONTAINER`                                  |
| Run MySQL with root password        | `docker run -e MYSQL_ROOT_PASSWORD=DB_PASS123 mysql` |
| Check environment variables         | `docker exec -it mysql-db env`                       |

---

## 💡 Notes / Tips

* Use `docker inspect` for detailed metadata (env vars, mounts, networks).
* Use `docker exec -it <container> env` to confirm active environment variables.
* MySQL **requires** `MYSQL_ROOT_PASSWORD` when running the container for the first time.
* Environment variables set with `-e` only exist *inside the container*, not on the host.
* Port mapping must match app requirements → `HOST:CONTAINER`.

---

## 📌 Lab Summary

| Step                                  | Status | Key Takeaways                    |
| ------------------------------------- | ------ | -------------------------------- |
| Inspect environment variable          | ✅      | `APP_COLOR = pink`               |
| Run new container with APP_COLOR=blue | ✅      | Exposed on port 8282             |
| Check Docker Hub for MySQL variable   | ✅      | Variable = `MYSQL_ROOT_PASSWORD` |
| Deploy MySQL                          | ✅      | Container: `mysql-db`            |
| Verify env variable                   | ✅      | Password confirmed               |

---

## ✅ References

* [Docker Environment Variables Guide](https://docs.docker.com/engine/reference/run/#env-environment-variables)
* [MySQL Docker Image Documentation](https://hub.docker.com/_/mysql)
* [Docker Inspect Documentation](https://docs.docker.com/engine/reference/commandline/inspect/)
