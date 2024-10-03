Born2beRoot - 42 Project
========================

Table of Contents
-----------------

-   [Introduction](#introduction)
-   [System Information](#system-information)
-   [Installation Guide](#installation-guide)
-   [Partitioning Scheme](#partitioning-scheme)
-   [User and Group Management](#user-and-group-management)
-   [SSH Configuration](#ssh-configuration)
-   [Firewall Configuration (UFW)](#firewall-configuration-ufw)
-   [Password Policies](#password-policies)
-   [Monitoring Setup](#monitoring-setup)
-   [Cron Jobs](#cron-jobs)
-   [Bonus Features](#bonus-features)
-   [Conclusion](#conclusion)

Introduction
------------

The **Born2beRoot** project is part of the 42 school curriculum. The goal is to configure a Linux-based virtual machine (VM) with essential system administration tools, focusing on security, service management, and automated tasks.

The project helps students build a solid foundation in system administration by setting up a secure and efficient server environment using either **Debian** or **CentOS**.

System Information
------------------

-   **Operating System**: [Debian/CentOS] (Choose based on your project)
-   **Kernel Version**: [e.g., Linux 5.10]
-   **Virtual Machine**: VirtualBox or UTM

Installation Guide
------------------

### Step 1: VM Setup

-   **Virtualization Software**: I used [VirtualBox/UTM](specify) to create a new virtual machine.
-   **ISO Download**: Download the official Debian or CentOS ISO image.

### Step 2: Install the Operating System

-   Boot the VM with the ISO image and follow the installation steps.
-   Configure the hostname and root password during the installation.
-   Partition the disk using the LVM setup (details below).

### Step 3: Update the System

bash

Copy code

`sudo apt update && sudo apt upgrade`

or

bash

Copy code

`sudo yum update`

Partitioning Scheme
-------------------

For this project, I used **LVM** (Logical Volume Manager) to manage the disk partitions efficiently.

| Partition | Mount Point | Type | Size |
| --- | --- | --- | --- |
| `/dev/sda1` | `/boot` | ext4 | 512MB |
| `/dev/sda2` | `LVM PV` | LVM | Remaining space |
| `LVM Root` | `/` | ext4 | 10GB |
| `LVM Swap` | `swap` | swap | 2GB |
| `LVM Home` | `/home` | ext4 | 5GB |

This setup allows for flexible management of storage space and easy resizing in the future.

User and Group Management
-------------------------

I created specific users and groups as required, focusing on user permissions and security:

1.  **Root user**: The root account is secured with a strong password and is disabled for SSH login.
2.  **New User**:
    -   Username: `username`
    -   Group: `sudo`
    -   Home directory created at `/home/username`

bash

Copy code

`adduser username
usermod -aG sudo username`

1.  **Group Creation**: I created additional groups for file permissions and management as needed.

SSH Configuration
-----------------

SSH is used to remotely manage the server securely.

1.  Installed and enabled the SSH service:

    bash

    Copy code

    `sudo apt install openssh-server
    sudo systemctl enable ssh
    sudo systemctl start ssh`

2.  **Root login disabled** for security reasons:

    -   In `/etc/ssh/sshd_config`, set:

        bash

        Copy code

        `PermitRootLogin no`

3.  Changed the default SSH port from `22` to `4242` to enhance security:

    bash

    Copy code

    `Port 4242`

4.  Restarted the SSH service:

    bash

    Copy code

    `sudo systemctl restart ssh`

Firewall Configuration (UFW)
----------------------------

The firewall is configured to allow only essential services.

1.  Installed UFW and enabled it:

    bash

    Copy code

    `sudo apt install ufw
    sudo ufw enable`

2.  Allowed SSH on the new port:

    bash

    Copy code

    `sudo ufw allow 4242/tcp`

3.  Check the status:

    bash

    Copy code

    `sudo ufw status`

Password Policies
-----------------

To enforce strong password policies, I configured PAM (Pluggable Authentication Modules) by editing `/etc/login.defs` and `/etc/pam.d/common-password`:

-   Password must be at least **10 characters** long.
-   Enforced password complexity (uppercase, lowercase, digits, and special characters).

bash

Copy code

`sudo nano /etc/login.defs`

Set:

Copy code

`PASS_MAX_DAYS 30
PASS_MIN_DAYS 2
PASS_MIN_LEN 10
PASS_WARN_AGE 7`

For more control:

bash

Copy code

`sudo nano /etc/pam.d/common-password`

Add:

bash

Copy code

`password requisite pam_pwquality.so retry=3 minlen=10 ucredit=-1 lcredit=-1 dcredit=-1 ocredit=-1`

Monitoring Setup
----------------

I created a bash script that outputs system information every 10 minutes and logs it to a file.

-   **CPU Usage**
-   **RAM Usage**
-   **Disk Usage**
-   **Logged-in users**
-   **Last boot time**

Sample script:

bash

Copy code

`#!/bin/bash
wall "Monitoring:
-----------------
CPU Load: $(top -bn1 | grep load | awk '{printf "%.2f", $(NF-2)}')
Memory Usage: $(free -m | awk 'NR==2{printf "%.2f", $3*100/$2 }')
Disk Usage: $(df -h | awk '$NF=="/"{printf "%s", $5}')
Last boot: $(who -b | awk '{print $3, $4}')"`

This script is added to a cron job for execution every 10 minutes.

Cron Jobs
---------
Born2beRoot - 42 Project
========================

Table of Contents
-----------------

-   [Introduction](#introduction)
-   [System Information](#system-information)
-   [Installation Guide](#installation-guide)
-   [Partitioning Scheme](#partitioning-scheme)
-   [User and Group Management](#user-and-group-management)
-   [SSH Configuration](#ssh-configuration)
-   [Firewall Configuration (UFW)](#firewall-configuration-ufw)
-   [Password Policies](#password-policies)
-   [Monitoring Setup](#monitoring-setup)
-   [Cron Jobs](#cron-jobs)
-   [Bonus Features](#bonus-features)
-   [Conclusion](#conclusion)

Introduction
------------

The **Born2beRoot** project is part of the 42 school curriculum. The goal is to configure a Linux-based virtual machine (VM) with essential system administration tools, focusing on security, service management, and automated tasks.

The project helps students build a solid foundation in system administration by setting up a secure and efficient server environment using either **Debian** or **CentOS**.

System Information
------------------

-   **Operating System**: [Debian/CentOS] (Choose based on your project)
-   **Kernel Version**: [e.g., Linux 5.10]
-   **Virtual Machine**: VirtualBox or UTM

Installation Guide
------------------

### Step 1: VM Setup

-   **Virtualization Software**: I used [VirtualBox/UTM](specify) to create a new virtual machine.
-   **ISO Download**: Download the official Debian or CentOS ISO image.

### Step 2: Install the Operating System

-   Boot the VM with the ISO image and follow the installation steps.
-   Configure the hostname and root password during the installation.
-   Partition the disk using the LVM setup (details below).

### Step 3: Update the System

bash

Copy code

`sudo apt update && sudo apt upgrade`

or

bash

Copy code

`sudo yum update`

Partitioning Scheme
-------------------

For this project, I used **LVM** (Logical Volume Manager) to manage the disk partitions efficiently.

| Partition | Mount Point | Type | Size |
| --- | --- | --- | --- |
| `/dev/sda1` | `/boot` | ext4 | 512MB |
| `/dev/sda2` | `LVM PV` | LVM | Remaining space |
| `LVM Root` | `/` | ext4 | 10GB |
| `LVM Swap` | `swap` | swap | 2GB |
| `LVM Home` | `/home` | ext4 | 5GB |

This setup allows for flexible management of storage space and easy resizing in the future.

User and Group Management
-------------------------

I created specific users and groups as required, focusing on user permissions and security:

1.  **Root user**: The root account is secured with a strong password and is disabled for SSH login.
2.  **New User**:
    -   Username: `username`
    -   Group: `sudo`
    -   Home directory created at `/home/username`

bash

Copy code

`adduser username
usermod -aG sudo username`

1.  **Group Creation**: I created additional groups for file permissions and management as needed.

SSH Configuration
-----------------

SSH is used to remotely manage the server securely.

1.  Installed and enabled the SSH service:

    bash

    Copy code

    `sudo apt install openssh-server
    sudo systemctl enable ssh
    sudo systemctl start ssh`

2.  **Root login disabled** for security reasons:

    -   In `/etc/ssh/sshd_config`, set:

        bash

        Copy code

        `PermitRootLogin no`

3.  Changed the default SSH port from `22` to `4242` to enhance security:

    bash

    Copy code

    `Port 4242`

4.  Restarted the SSH service:

    bash

    Copy code

    `sudo systemctl restart ssh`

Firewall Configuration (UFW)
----------------------------

The firewall is configured to allow only essential services.

1.  Installed UFW and enabled it:

    bash

    Copy code

    `sudo apt install ufw
    sudo ufw enable`

2.  Allowed SSH on the new port:

    bash

    Copy code

    `sudo ufw allow 4242/tcp`

3.  Check the status:

    bash

    Copy code

    `sudo ufw status`

Password Policies
-----------------

To enforce strong password policies, I configured PAM (Pluggable Authentication Modules) by editing `/etc/login.defs` and `/etc/pam.d/common-password`:

-   Password must be at least **10 characters** long.
-   Enforced password complexity (uppercase, lowercase, digits, and special characters).

bash

Copy code

`sudo nano /etc/login.defs`

Set:

Copy code

`PASS_MAX_DAYS 30
PASS_MIN_DAYS 2
PASS_MIN_LEN 10
PASS_WARN_AGE 7`

For more control:

bash

Copy code

`sudo nano /etc/pam.d/common-password`

Add:

bash

Copy code

`password requisite pam_pwquality.so retry=3 minlen=10 ucredit=-1 lcredit=-1 dcredit=-1 ocredit=-1`

Monitoring Setup
----------------

I created a bash script that outputs system information every 10 minutes and logs it to a file.

-   **CPU Usage**
-   **RAM Usage**
-   **Disk Usage**
-   **Logged-in users**
-   **Last boot time**

Sample script:

bash

Copy code

`#!/bin/bash
wall "Monitoring:
-----------------
CPU Load: $(top -bn1 | grep load | awk '{printf "%.2f", $(NF-2)}')
Memory Usage: $(free -m | awk 'NR==2{printf "%.2f", $3*100/$2 }')
Disk Usage: $(df -h | awk '$NF=="/"{printf "%s", $5}')
Last boot: $(who -b | awk '{print $3, $4}')"`

This script is added to a cron job for execution every 10 minutes.

Cron Jobs
---------

I set up a **cron job** to run the monitoring script:

bash

Copy code

`crontab -e`

Add the following line:

bash

Copy code

`*/10 * * * * /path/to/monitoring_script.sh`

Bonus Features
--------------

As part of the bonus, I implemented:

-   **Fail2Ban**: To protect the SSH server from brute-force attacks.

-   **Custom Bash Prompt**: A personalized and more informative bash prompt.
-   **Additional Partitioning**: More complex partition setups for `/var` and `/tmp`.

Conclusion
----------

The **Born2beRoot** project was a great introduction to system administration and security practices. By completing this project, I gained hands-on experience with setting up and securing a Linux server, managing users, configuring SSH, and ensuring the system is monitored and secured with firewall rules and cron jobs.
I set up a **cron job** to run the monitoring script:

bash

Copy code

`crontab -e`

Add the following line:

bash

Copy code

`*/10 * * * * /path/to/monitoring_script.sh`

Bonus Features
--------------

As part of the bonus, I implemented:

-   **Fail2Ban**: To protect the SSH server from brute-force attacks.
-   **Custom Bash Prompt**: A personalized and more informative bash prompt.
-   **Additional Partitioning**: More complex partition setups for `/var` and `/tmp`.

Conclusion
----------

The **Born2beRoot** project was a great introduction to system administration and security practices. By completing this project, I gained hands-on experience with setting up and securing a Linux server, managing users, configuring SSH, and ensuring the system is monitored and secured with firewall rules and cron jobs.
