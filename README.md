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

## 🔄 Alternative: Port Forwarding

If your VM IP is not directly reachable, forward port 443 to your host:

``` bash
multipass exec wazuh-vm -- sudo snap set multipass forward=tcp:0.0.0.0:8443:443
```

Then access from host:

    https://127.0.0.1:8443

------------------------------------------------------------------------

## ✅ Summary

-   Install with: `wazuh-install.sh -a`
-   Access via: `https://<VM_IP>` or forwarded port
-   Default user: `admin`
-   Password: stored in
    `/var/ossec/api/configuration/auth/credentials.json`
