# System Configuration Project

This project involves configuring a system to meet specific requirements, including password policies, user management, directory permissions, remote access, log file archiving, and software installation. Below is a detailed guide on how each task was accomplished.

## 1. Password Policy for New Users

All new users are required to change their passwords every 60 days, with a warning issued 7 days before the expiration date.

### Steps:
1. **Edit the Global Password Policy**
   - Open `/etc/login.defs`:
     ```
     sudo vim /etc/login.defs
     ```
   - Set the following parameters:
     ```
     PASS_MAX_DAYS   60
     PASS_MIN_DAYS   0
     PASS_WARN_AGE   7
     ```

2. **Apply the Policy to Existing Users**
   - For each user, apply the password policy using:
     ```
     sudo chage --maxdays 60 --warndays 7 username
     ```

3. **Ensure New Users Have Default Settings**
   - Edit `/etc/default/useradd` to set default values:
     ```
     sudo nano /etc/default/useradd
     ```
   - Add or modify:
     ```
     EXPIRE=60
     ```

## 2. Create Users and Manage Group Membership

Create three users and make them members of the `sales` group as a secondary group.

### Steps:
1. **Create Users**
   ```
   sudo useradd  user01
   sudo useradd  user02
   sudo useradd  user03

2. **Create Group**
    ```
    sudo groupadd sales

3. **Add Users to the Sales Group**
    ```
    sudo usermod -aG sales user1
    sudo usermod -aG sales user2
    sudo usermod -aG sales user3

## 3. Create and Configure Workspace Directory

Create a workspace directory for the sales group where members can edit files but not delete files created by others.

### Steps:
1. **Create the Directory**
```
sudo mkdir /home/workspace
sudo chown :sales /home/workspace
```

2. **Set Directory Permissions**
```
sudo chmod 2775 /home/sales_workspace
sudo chmod +t /home/sales_workspace
```

## 4. Enable SSH Access

Ensure all users can work remotely via SSH.

### Steps:
1. **Install OpenSSH Server**
```
sudo apt update
sudo apt install openssh-server
```

2. **Enable and Start SSH Service**
```
sudo systemctl enable ssh
sudo systemctl start ssh
```
3. **Check SSH Configuration**
```
sudo nano /etc/ssh/sshd_config
```
4. **Configure the Firewall (if applicable)**
```
sudo ufw allow ssh
```
## 5. Archive Log Files

Archive all log files and store the archive in the root home directory.

### Steps:
1. **Create the Archive**
```
sudo tar -czvf /root/logs_archive.tar.gz /var/log/*
```
2. **Verify the Archive**
```
sudo tar -tzvf /root/logs_archive.tar.gz
```
## 6. Install Google Chrome Browser

Install Google Chrome on the system.

### Steps:
1. **Download the .deb Package**
```
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
```
2. **Install the Package**
```
sudo dpkg -i google-chrome-stable_current_amd64.deb
```
3. **Fix Any Dependency Issues**
```
sudo apt-get install -f
```

# Summary
This document provides an overview of configuring a system with password policies, user management, workspace permissions, remote access, log file archiving, and software installation. Follow the provided steps to implement these configurations on your Ubuntu system.

For further details or troubleshooting, refer to the appropriate man pages or official documentation.