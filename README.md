# 🚀 Ready-to-Use Wazuh Server on Multipass

Deploy a production-like Wazuh Server stack (manager + indexer + dashboard) inside a lightweight Multipass VM — safe, fast, and hassle-free.

---

## 📑 Table of Contents

- [🚀 Features](#-features)
- [🛠️ Requirements](#-requirements)
- [📦 Installation & Usage](#-installation--usage)
- [📊 Accessing the Dashboard](#-accessing-the-dashboard)
- [🛠️ Wazuh Dashboard Troubleshooting](#-wazuh-dashboard-troubleshooting)

---

## 🚀 Features

- 🔐 Wazuh Server (manager + indexer)
- 📊 Wazuh Dashboard for real-time monitoring
- ⚡ Quick deployment in an isolated VM
- 🛡️ Perfect for labs, testing, and demos without touching your host system
- ✨ Why this repo?
    - Zero clutter on your host
    - Automatic setup of Wazuh all-in-one
    - Access the dashboard in minutes
    - Works cross-platform (Linux, macOS, Windows with Multipass)

👉 Ideal for anyone who wants to try out Wazuh quickly without complex configuration.

---

## 🛠️ Requirements
- OS : Ubuntu (tested on **22.04+**) _or_ Windows (tested on **Windows11**)
- Interpreter/Runtime : **Python 3.8+** (MUST be installed in the system)

---

## 📦 Installation & Usage  

[LINUX] Clone the repository and run the setup script:
```bash
git clone https://github.com/sayandip-chatterjee/multipass-wazuh-server.git
cd multipass-wazuh-server/
python3 setup_wazuh_server.py
```

[WINDOWS] Ensure all the steps are done as mentioed:
```bash
- In the Windows machine BIOS setup, make sure that virtualization is turned on
- Install git bash - https://git-scm.com/downloads/win and close the git bash window, do not clone yet.
- Install python3.8 from Microsoft Store
- Go to Windows Features from the Start Menu -> Search and make sure You enable the
  "HyperV", "Virtual Machine Platform", and the "Windows Hypervisor Platform" to run the VM.
- Restart the machine.
- Open powershell (NOT AS Administrator)
- git clone https://github.com/sayandip-chatterjee/multipass-wazuh-server.git
- cd multipass-wazuh-server/
- python3 setup_wazuh_server.py
```

---

## 📊 Accessing the Dashboard

1.  **Get your Multipass VM IP**

    ``` bash
    multipass list
    ```

    Example output:

        Name       State    IPv4            Image
        wazuh-vm   Running  192.168.64.10   Ubuntu 22.04 LTS

2.  **Open the Dashboard in browser**

        https://<VM_IP>

    Example:

        https://192.168.64.10

3.  **Login credentials**

    -   Default username: `admin`

    -   Password: shown at the end of the installer.\
        If you missed it:

        ``` bash
        sudo cat /var/ossec/api/configuration/auth/credentials.json
        ```

    > ⚠️ You may see a browser warning about a self-signed SSL
    > certificate --- just continue.

---

## 🛠️ Wazuh Dashboard Troubleshooting

1. **Check if Wazuh Dashboard service is running**
```bash
multipass exec wazuh-vm -- sudo systemctl status wazuh-dashboard
```
Expected: `active (running)`.

---

2. **Verify Port 443 is listening**
```bash
multipass exec wazuh-vm -- sudo ss -tlnp | grep 443
```
Expected output:
```
LISTEN 0 128 *:443
```

---

3. **Check Firewall (UFW)**
If UFW is enabled inside the VM, allow HTTPS:
```bash
multipass exec wazuh-vm -- sudo ufw status
multipass exec wazuh-vm -- sudo ufw allow 443
```

---

4. **Test From Host**
Run this from your **host** machine:
```bash
curl -k https://10.157.193.80
```
The `-k` option ignores SSL warnings.  
If you see HTML output, the dashboard is working.

------------------------------------------------------------------------
