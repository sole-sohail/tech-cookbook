# How to Install and Configure Ollama at a Custom Location on Linux

This tutorial explains step-by-step how to install and configure **Ollama** at a custom directory location (`/data/ollama`) on a Linux server, including how to set up the Ollama service to listen on a custom network interface (`0.0.0.0:11434`).

## Prerequisites

- Linux OS
- SSH access with root or sudo privileges
- Internet connectivity

---

## Step 1: Prepare Custom Directory and User

Create the custom directory and a dedicated user (`ollama`) for running the Ollama service:

```bash
sudo mkdir -p /data/ollama
sudo useradd -r -s /bin/false -U -m -d /data/ollama ollama
sudo chown -R ollama:ollama /data/ollama
```

---

## Step 2: Download and Install Ollama Binaries

Download and extract the latest Ollama binaries into `/data/ollama`:

```bash
curl -L https://ollama.com/download/ollama-linux-amd64.tgz | sudo tar -xzf - -C /data/ollama
sudo chown -R ollama:ollama /data/ollama
```

> **Note:** This binary is compatible with CentOS/RHEL 9.x (`amd64`/`x86_64`).

---

## Step 3: Configure Ollama as a Systemd Service

Create a new service file at `/etc/systemd/system/ollama.service`:

```ini
[Unit]
Description=Ollama Service
After=network-online.target

[Service]
ExecStart=/data/ollama/bin/ollama serve
WorkingDirectory=/data/ollama
User=ollama
Group=ollama
Environment="HOME=/data/ollama"
Environment="OLLAMA_MODELS=/data/ollama/models"
Environment="OLLAMA_HOST=0.0.0.0:11434"
Restart=always
RestartSec=3

[Install]
WantedBy=multi-user.target
```

Activate the service:

```bash
sudo mkdir -p /data/ollama/models
sudo chown -R ollama:ollama /data/ollama/models

sudo systemctl daemon-reload
sudo systemctl enable --now ollama
```

Verify the service status:

```bash
sudo systemctl status ollama --no-pager
```

---

## Step 4: Firewall Configuration

Allow Ollama port (`11434/tcp`) through the firewall (`firewalld`):

```bash
sudo firewall-cmd --permanent --add-port=11434/tcp
sudo firewall-cmd --reload
```

---

## Step 5: Verify Installation

Test Ollama installation with a simple API call:

```bash
curl http://localhost:11434/api/version
```

A successful response returns JSON containing Ollama's version.

---

## Optional: Access Ollama Remotely

Test remote access from another machine:

```bash
curl http://<server-ip>:11434/api/version
```

Replace `<server-ip>` with your Ollama server IP address.

---

## Conclusion

You've successfully installed and configured Ollama in a custom directory (`/data/ollama`) and set it up as a robust, easily maintainable system service listening on `0.0.0.0:11434`. Your Ollama instance is now ready for AI model deployments and API integrations.

