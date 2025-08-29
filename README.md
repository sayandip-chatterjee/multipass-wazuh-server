# Wazuh Server Installation in Multipass VM (Ubuntu)

This guide explains how to install **Wazuh Server** inside a Multipass VM and access the **Wazuh Dashboard** from your host machine.

------------------------------------------------------------------------

## 🚀 Installation

1.  **Download and run the installer (all-in-one)**

    ``` bash
    curl -sO https://packages.wazuh.com/4.12/wazuh-install.sh
    sudo bash ./wazuh-install.sh -a
    ```

    This installs:

    -   Wazuh Manager
    -   Wazuh Indexer
    -   Wazuh Dashboard

2.  **(Optional and NOT RECOMMENDED) Install Wazuh Agent on the same VM**

    ``` bash
    sudo apt update
    sudo apt install wazuh-agent
    ```

    Configure agent to send logs to the local manager:

    ``` bash
    sudo nano /var/ossec/etc/ossec.conf
    # Set the address:
    <address>127.0.0.1</address>
    ```

    Then start the agent:

    ``` bash
    sudo systemctl daemon-reload
    sudo systemctl enable --now wazuh-agent
    ```

------------------------------------------------------------------------

## 🌐 Accessing the Dashboard

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

------------------------------------------------------------------------

## 🛠️ Wazuh Dashboard Access Troubleshooting

If you cannot access the Wazuh Dashboard in your Multipass VM, follow these steps.

1. Check if Wazuh Dashboard service is running
```bash
multipass exec wazuh-vm -- sudo systemctl status wazuh-dashboard
```
Expected: `active (running)`.

---

2. Verify Port 443 is listening
```bash
multipass exec wazuh-vm -- sudo ss -tlnp | grep 443
```
Expected output:
```
LISTEN 0 128 *:443
```

---

3. Check Firewall (UFW)
If UFW is enabled inside the VM, allow HTTPS:
```bash
multipass exec wazuh-vm -- sudo ufw status
multipass exec wazuh-vm -- sudo ufw allow 443
```

---

4. Test From Host
Run this from your **host** machine:
```bash
curl -k https://10.157.193.80
```
The `-k` option ignores SSL warnings.  
If you see HTML output, the dashboard is working.

------------------------------------------------------------------------

## ✅ Summary

-   Install with: `wazuh-install.sh -a`
-   Access via: `https://<VM_IP>` or forwarded port
-   Default user: `admin`
-   Password: stored in
    `/var/ossec/api/configuration/auth/credentials.json`
