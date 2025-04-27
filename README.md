# 🛠 Dummy Systemd Service Project

This project demonstrates how to create a custom Linux **systemd service** that runs a dummy script in the background, writes logs, restarts automatically if it crashes, and enables itself at boot.

---

## ✨ Project Overview

- **`dummy.sh`**: A simple bash script that runs forever and writes to `/var/log/dummy-service.log` every 10 seconds.
- **`dummy.service`**: A systemd service file to manage the dummy script.
- Service automatically **restarts** if it fails.
- Service **enables on boot**.

---

## 📂 Project Structure

```
dummy-service/
├── dummy.sh           # Bash script that simulates an application
├── README.md          # This file
```

Systemd service file:

```
/etc/systemd/system/dummy.service
```

Log file generated:

```
/var/log/dummy-service.log
```

---

## ⚙️ Setup Instructions

### 1. Clone or create the project directory

```bash
mkdir ~/dummy-service
cd ~/dummy-service
```

### 2. Create `dummy.sh`

Create a file called `dummy.sh`:

```bash
nano dummy.sh
```

Paste the following content:

```bash
#!/bin/bash

while true; do
  echo "Dummy service is running..." >> /var/log/dummy-service.log
  sleep 10
done
```

Make it executable:

```bash
chmod +x dummy.sh
```

---

### 3. Create the systemd service

Create a systemd service file:

```bash
sudo nano /etc/systemd/system/dummy.service
```

Paste the following content:

```ini
[Unit]
Description=Dummy Service for Testing
After=network.target

[Service]
ExecStart=/home/YOUR_USERNAME/dummy-service/dummy.sh
Restart=always
RestartSec=5
User=YOUR_USERNAME

[Install]
WantedBy=multi-user.target
```

> 🧠 Replace `YOUR_USERNAME` with your Linux username (you can check it by running `whoami`).

---

### 4. Reload systemd

```bash
sudo systemctl daemon-reload
```

---

### 5. Start and Enable the Service

Start the service:

```bash
sudo systemctl start dummy
```

Enable it to start automatically at boot:

```bash
sudo systemctl enable dummy
```

---

## 🔥 Managing the Service

| Command | Description |
|:-------:|:------------|
| `sudo systemctl start dummy` | Start the service manually |
| `sudo systemctl stop dummy` | Stop the service manually |
| `sudo systemctl enable dummy` | Enable the service at boot |
| `sudo systemctl disable dummy` | Disable the service at boot |
| `sudo systemctl status dummy` | Check the current status |
| `sudo journalctl -u dummy -f` | View service logs live |

---

## 📈 Verifying the Log Output

You can check the custom log file created by the dummy service:

```bash
tail -f /var/log/dummy-service.log
```

You should see:

```
Dummy service is running...
Dummy service is running...
...
```
updated every 10 seconds!

---

## 🛠 How it Works

- `dummy.sh` keeps running an infinite loop.
- Every 10 seconds, it appends a log message to `/var/log/dummy-service.log`.
- If the script crashes, **systemd** restarts it automatically (`Restart=always`).
- Service is automatically started when the machine reboots (`WantedBy=multi-user.target`).

---

## 🚀 Useful Systemd Tips

- **View all services:**
  ```bash
  systemctl list-units --type=service
  ```

- **Debug service issues:**
  ```bash
  sudo journalctl -xe
  ```

- **Restart a service after making changes:**
  ```bash
  sudo systemctl daemon-reload
  sudo systemctl restart dummy
  ```

---

## ✅ Final Notes

After completing this project, you will have a strong understanding of:

- How systemd services work
- Creating custom service units
- Managing services (start, stop, enable, disable, logs)
- Troubleshooting systemd services
- Running background applications automatically and reliably

https://roadmap.sh/projects/dummy-systemd-service
