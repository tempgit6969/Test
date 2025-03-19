Sure! Here’s a **nutshell summary** of how to set up **RDP (Remote Desktop) on Codespace** and easily reconnect next time.

---

### **💡 Step-by-Step Process to Set Up RDP in Codespace**
---

### **1️⃣ Install and Start XRDP (Remote Desktop Protocol)**
When you create a new Codespace, you need to install and configure `xrdp` to allow remote desktop connections.

```bash
sudo apt update && sudo apt install -y xrdp xfce4 xfce4-goodies firefox
```
- `xrdp` → Enables **RDP access**
- `xfce4 xfce4-goodies` → **Lightweight desktop environment**
- `firefox` → Web browser

#### **Start & Enable XRDP**
```bash
sudo service xrdp start
```
#### Check XRDP is running
```bash
sudo service xrdp status
```

#### Set admin password
```bash
sudo passwd codespace
```

#### Auto-start xRDP on Container Restart
GitHub Codespaces does not persist services across restarts by default. To ensure xrdp starts automatically, add this to your `.bashrc` or `.bash_profile`:
```bash
echo "sudo service xrdp start" >> ~/.bashrc
```
Then Apply changes
```bash
source ~/.bashrc
```
- Ensures XRDP **starts automatically** on reboot.

---

### **2️⃣ Set Up the Desktop Environment for RDP**
- XRDP doesn’t start a desktop automatically, so we need to configure it.

Edit **startwm.sh** to use XFCE:
```bash
sudo nano /etc/xrdp/startwm.sh
```
Replace **everything inside** with:
```bash
#!/bin/sh
unset DBUS_SESSION_BUS_ADDRESS
unset XDG_RUNTIME_DIR
startxfce4
```
**Save and exit** (`CTRL+X`, then `Y`, then `ENTER`).

---

### **3️⃣ Restart XRDP & Allow Remote Connections**
```bash
sudo service xrdp restart
```

#### Configure your local machine
1. Now first of all we have to install [Git CLI](https://cli.github.com/)
2. Login to your github account where you have your codespace by
  ```bash
  gh auth login -s codespace
  ```
  follow on screen instructions set access to ssh and create ssh key and passphrase and then run the command to see list of running codespaces in your account:
  ```bash
  gh codespace list
  ```
  Then copy the name of of your codespace and connect it by the command:
  ```bash
  gh codespace ssh -c yourcodespacename -- -L 3389:localhost:3389
  ```
  That's it it will connect to your codespace via terminal and expose it to the localhost for rdp client to connect it.
