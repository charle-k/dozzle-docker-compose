Here's a sample `README.md` explaining the Docker Compose setup for Dozzle, including the available environment options like `DOZZLE_ENABLE_ACTIONS`, `DOZZLE_NO_ANALYTICS`, and `DOZZLE_AUTH_PROVIDER`.

---

````markdown
# 🐳 Dozzle Log Viewer - Docker Compose Setup

This Docker Compose configuration runs [Dozzle](https://dozzle.dev), a real-time web-based log viewer for Docker containers. It provides an easy way to monitor container logs without using the command line.

---

## 🚀 Getting Started

### 1. Clone this repo or copy the `docker-compose.yml`:
```bash
git clone https://github.com/yourusername/dozzle-setup.git
cd dozzle-setup
````

### 2. Start the container:

```bash
docker compose up -d
```

### 3. Open your browser:

```
http://localhost:8888
```

---


Here’s an updated `README.md` section including a command to **build an SSH tunnel** that forwards a remote machine's Dozzle port (`8888`) to your local machine, so you can securely access the interface even if it's not exposed publicly.

---

### 🔐 Accessing Dozzle Remotely via SSH Tunnel

If your server does **not expose port 8888 to the public**, you can create an **SSH tunnel** to access Dozzle securely from your local machine.

#### 🖥️ Command (run this on your local machine):

```bash
ssh -L 8888:localhost:8888 your-user@your-server-ip
```

#### 🔍 Explanation:

| Part                     | Meaning                                                                                  |
| ------------------------ | ---------------------------------------------------------------------------------------- |
| `ssh`                    | Secure shell command                                                                     |
| `-L 8888:localhost:8888` | Forwards your **local port 8888** to the **server's localhost:8888** (where Dozzle runs) |
| `your-user`              | Your SSH username (e.g., `ubuntu` or `charle`)                                           |
| `your-server-ip`         | The IP address or domain name of your server                                             |

#### ✅ After connecting:

Open your browser and go to:

```
http://localhost:8888
```

This will display Dozzle running on the remote server via the secure SSH tunnel.

---

Would you like me to insert this into the full `README.md` content I gave earlier?


## 🧾 docker-compose.yml Overview

```yaml
version: '3.8'

services:
  dozzle:
    image: amir20/dozzle:latest
    restart: always
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - ./auth/data:/data
    ports:
      - 8888:8080
    environment:
      DOZZLE_ENABLE_ACTIONS: true
#     DOZZLE_AUTH_PROVIDER: simple
      DOZZLE_NO_ANALYTICS: 1
```

---

## ⚙️ Environment Options

### 🔹 `DOZZLE_ENABLE_ACTIONS`

* Enables interactive log actions in the web UI (e.g., **Download**, **Clear Logs**).
* **Values**:

  * `true` – Actions enabled
  * `false` (or unset) – Actions hidden

### 🔹 `DOZZLE_NO_ANALYTICS`

* Disables anonymous usage tracking.
* **Values**:

  * `1` – Analytics disabled
  * `0` (or unset) – Analytics enabled

### 🔹 `DOZZLE_AUTH_PROVIDER`

* Enables basic authentication to secure the UI.
* **Value**:

  * `simple` – Enables built-in username/password login
* **To use it**, set the variable and create a `users.yaml` file under `./auth/data/users.yaml`:

```yaml
users:
  - username: admin
    password: secret
```

---

## 📁 Volumes

| Host Path              | Container Path         | Purpose                               |
| ---------------------- | ---------------------- | ------------------------------------- |
| `/var/run/docker.sock` | `/var/run/docker.sock` | Enables Dozzle to read container logs |
| `./auth/data`          | `/data`                | Stores config files like `users.yaml` |

---

## 🔐 Securing Access

To enable login:

1. Uncomment `DOZZLE_AUTH_PROVIDER: simple`
2. Create `./auth/data/users.yaml` with your credentials
3. Restart Dozzle:

   ```bash
   docker compose restart
   ```

---

## 🧹 Stopping the Service

```bash
docker compose down
```

---

## 📎 Reference

* Official Dozzle site: [https://dozzle.dev](https://dozzle.dev)
* Docker Hub: [https://hub.docker.com/r/amir20/dozzle](https://hub.docker.com/r/amir20/dozzle)

---

## 📄 License

This configuration is provided under the MIT License.

