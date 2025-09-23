# 🔐 Secure Shell (SSH) Setup & Hardening (Ubuntu VM Lab)

## 📖 Description
This lab focuses on setting up and hardening **SSH (Secure Shell)** on an Ubuntu virtual machine running in VirtualBox. The goal is to configure secure remote access, enforce **key-based authentication**, and apply **basic hardening techniques** such as disabling password login and changing the default SSH port.

## 🖥️ Environments Used
- **Host OS:** Windows 11  
- **Guest OS:** Ubuntu  
- **Virtualization:** VirtualBox  

## 🛠️ Lab Walkthrough

### 🔹Step 1: Install and Enable OpenSSH
---
**Description:**  
Install the OpenSSH server on Ubuntu and ensure the service is running.

### 🖥️ Commands to Run (Ubuntu Linux Terminal)
<details>
  <summary>📌Show Commands  </summary>

  ```bash
  # Update system packages
  sudo apt update && sudo apt upgrade -y

  # Install the OpenSSH server package
  sudo apt install openssh-server -y

  # Enable and start the SSH service
  sudo systemctl enable ssh
  sudo systemctl start ssh

  # Check SSH service status
  sudo systemctl status ssh
```
 </details>
  <details>
    <summary>Click to view results✅</summary>
<p align="center">✅Execution of commands resulted in SSH being active and running✅!
<img src="https://i.imgur.com/tvAz3SS.png" height="60%" width="60%" alt="SSH Setup"/>
 </details>

### 🔹Step 2 – Generate SSH Keys (on Windows Host)
---
Instead of using passwords, we’ll generate an **SSH key pair** on the Windows host machine.  
This will create a **private key** (kept secret) and a **public key** (shared with the server).  



### 🖥️ Commands to Run (in PowerShell)

<details>
<summary>📌 Show Commands</summary>

```powershell
# Generate a new 4096-bit RSA key pair
ssh-keygen -t rsa -b 4096
```
</details>
  <details>
    <summary>Click to view results✅</summary>
    <p>
👉 Two files are created inside your `~/.ssh/` directory:<p>
-**id_rsa** → 🔒 Private key(keep this safe, do **not** share)<p>
-**id_rsa.pub** → 🔑 Public key (this will be copied to the Ubuntu VM in Step 3)<p>
  <p align="center">
<img src="https://i.imgur.com/3pErgXH.png" height="60%" width="60%" alt="SSH Setup"/>
 </details>
 
### 🔹Step 3 - Copy/Add Public Key to Ubuntu  
---
Now we’ll add the **public key** generated on your Windows host into the Ubuntu VM to allow SSH key-based authentication.  

### 🖥️ Commands to Run (in Ubuntu Linux)

### 🔑 Manual Method (on Ubuntu VM)  

<details>
  <summary>📌Show commands</summary>

```bash
# 1. Create the .ssh directory with secure permissions
mkdir -p ~/.ssh && chmod 700 ~/.ssh  

# 2. Edit or create the authorized_keys file
nano ~/.ssh/authorized_keys  

# (Paste the contents of your Windows host's id_rsa.pub here)
# Save with CTRL+O, exit with CTRL+X

# 3. Secure the authorized_keys file
chmod 600 ~/.ssh/authorized_keys
```
</details>
 <details>
  <summary>Click to view results✅</summary> 
You'll know when your in the file on your VM Lab when it looks like this: <p>
  <p align="center">
<img src="https://i.imgur.com/sC89QGH.png" height="60%" width="60%" alt="SSH Setup"/>    <p>
   <p>
After adding the SSH public key and fixing permissions, I verified the setup: <p>

```bash
ls -ld ~/.ssh
ls -l ~/.ssh/authorized_keys
```
  <p align="center">
<img src="https://i.imgur.com/U6buB5p.png" height="60%" width="60%" alt="SSH Setup"/>

 
Explanation:

.ssh directory (drwx------) → 700<p>
- d     → it’s a directory<p>
- rwx   → the owner (you) can read, write, and enter<p>
- ------ → everyone else cannot see or access<p>

authorized_keys file (-rw-------) → 600<p>
- -  → it’s a file<p>
- rw-   → the owner can read and write (manage keys)<p>
- ------ → everyone else cannot read or change it<p>

File sizes (4096 for folder, 725 for file) → just how much data is inside, not related to permissions<p>
 </details>
 
### Step 4: Test SSH Connection from Windows Host

This step verifies that the SSH key-based authentication works, allowing secure login to the Ubuntu/Kali VM without using a password (after entering the passphrase if your key has one). <p>
**Be Prepared to encounter some roadblocks as this step can be challenging when troubleshooting possible issues**<p>


---
### 🖥️ Commands to Run (in PowerShell)
<details>
  <summary>📌Show commands</summary>


```powershell
# Connect to the VM using the private key
ssh -i $env:USERPROFILE\.ssh\id_rsa <username>@<vm-ip>
```
Notes:
- Replace <username> with your VM user<p>
- Replace <vm-ip> with your VM’s IP address.<p>
- If your key has a passphrase, you will be prompted to enter it once.<p>
</details>
 <details>
  <summary>Click to view results✅</summary> 
<p>
When successful, you should see:<p>
  <p align="center">
<img src="https://i.imgur.com/aiJXZXW.png" height="60%" width="60%" alt="SSH Setup"/>
    <p>
Explanation :<p>
  
-Private Key (id_rsa): Stays on your Windows host; used to authenticate you to the VM.<p>

-Public Key (id_rsa.pub): Stored on the VM in ~/.ssh/authorized_keys.<p>

-SSH first checks if the key pair matches. If correct, it logs you in without asking for your VM password.<p>

-The passphrase on your private key is an additional security layer. You can keep it for safety or remove it if you want automatic login.<p>

-The warning about .zsh_history is not related to SSH and can be ignored or fixed separately.<p>
</details>




