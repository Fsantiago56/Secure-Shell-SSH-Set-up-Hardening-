# 🔐 Secure Shell (SSH) Setup & Hardening (Ubuntu VM Lab)

## 📖 Description
This lab focuses on setting up and hardening **SSH (Secure Shell)** on an Ubuntu virtual machine running in VirtualBox. The goal is to configure secure remote access, enforce **key-based authentication**, and apply **basic hardening techniques** such as disabling password login and changing the default SSH port.

## 🖥️ Environments Used
- **Host OS:** Windows 11  
- **Guest OS:** Ubuntu  
- **Virtualization:** VirtualBox  

## 🛠️ Lab Walkthrough

### Step 1: Install and Enable OpenSSH

**Description:**  
Install the OpenSSH server on Ubuntu and ensure the service is running.

<details>
  <summary>Click to view commands & results</summary>

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
<details>
  <summary>Click to view results</summary>
<p align="center">✅Execution of commands resulted in SSH being active and running✅!
<p align="center">

<img src="https://i.imgur.com/tvAz3SS.png" height="60%" width="60%" alt="SSH Setup"/>

