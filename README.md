Born2beRoot - 42 Project
========================

Table of Contents
-----------------

*   [Introduction](#introduction)
    
*   [System Information](#system-information)
    
*   [Installation Guide](#installation-guide)
    
*   [Partitioning Scheme](#partitioning-scheme)
    
*   [User and Group Management](#user-and-group-management)
    
*   [SSH Configuration](#ssh-configuration)
    
*   [Firewall Configuration (UFW)](#firewall-configuration-ufw)
    
*   [Password Policies](#password-policies)
    
*   [Monitoring Setup](#monitoring-setup)
    
*   [Cron Jobs](#cron-jobs)
    
*   [Bonus Features](#bonus-features)
    
*   [Conclusion](#conclusion)
    

Introduction
------------

The **Born2beRoot** project is part of the 42 school curriculum. The goal is to configure a Linux-based virtual machine (VM) with essential system administration tools, focusing on security, service management, and automated tasks.

The project helps students build a solid foundation in system administration by setting up a secure and efficient server environment using either **Debian** or **CentOS**.

System Information
------------------

*   **Operating System**: \[Debian/CentOS\] (Choose based on your project)
    
*   **Kernel Version**: \[e.g., Linux 5.10\]
    
*   **Virtual Machine**: VirtualBox or UTM
    

Installation Guide
------------------

### Step 1: VM Setup

*   **Virtualization Software**: I used [VirtualBox/UTM](specify) to create a new virtual machine.
    
*   **ISO Download**: Download the official Debian or CentOS ISO image.
    

### Step 2: Install the Operating System

*   Boot the VM with the ISO image and follow the installation steps.
    
*   Configure the hostname and root password during the installation.
    
*   Partition the disk using the LVM setup (details below).
    

### Step 3: Update the System

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   sudo apt update && sudo apt upgrade   `

or

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   sudo yum update   `

Partitioning Scheme
-------------------

For this project, I used **LVM** (Logical Volume Manager) to manage the disk partitions efficiently.

PartitionMount PointTypeSize/dev/sda1/bootext4512MB/dev/sda2LVM PVLVMRemaining spaceLVM Root/ext410GBLVM Swapswapswap2GBLVM Home/homeext45GB

This setup allows for flexible management of storage space and easy resizing in the future.

User and Group Management
-------------------------

I created specific users and groups as required, focusing on user permissions and security:

1.  **Root user**: The root account is secured with a strong password and is disabled for SSH login.
    
2.  **New User**:
    
    *   Username: username
        
    *   Group: sudo
        
    *   Home directory created at /home/username
        

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   adduser username  usermod -aG sudo username   `

1.  **Group Creation**: I created additional groups for file permissions and management as needed.
    

SSH Configuration
-----------------

SSH is used to remotely manage the server securely.

1.  bashCopy codesudo apt install openssh-serversudo systemctl enable sshsudo systemctl start ssh
    
2.  **Root login disabled** for security reasons:
    
    *   bashCopy codePermitRootLogin no
        
3.  bashCopy codePort 4242
    
4.  bashCopy codesudo systemctl restart ssh
    

Firewall Configuration (UFW)
----------------------------

The firewall is configured to allow only essential services.

1.  bashCopy codesudo apt install ufwsudo ufw enable
    
2.  bashCopy codesudo ufw allow 4242/tcp
    
3.  bashCopy codesudo ufw status
    

Password Policies
-----------------

To enforce strong password policies, I configured PAM (Pluggable Authentication Modules) by editing /etc/login.defs and /etc/pam.d/common-password:

*   Password must be at least **10 characters** long.
    
*   Enforced password complexity (uppercase, lowercase, digits, and special characters).
    

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   sudo nano /etc/login.defs   `

Set:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   PASS_MAX_DAYS 30  PASS_MIN_DAYS 2  PASS_MIN_LEN 10  PASS_WARN_AGE 7   `

For more control:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   sudo nano /etc/pam.d/common-password   `

Add:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   password requisite pam_pwquality.so retry=3 minlen=10 ucredit=-1 lcredit=-1 dcredit=-1 ocredit=-1   `

Monitoring Setup
----------------

I created a bash script that outputs system information every 10 minutes and logs it to a file.

*   **CPU Usage**
    
*   **RAM Usage**
    
*   **Disk Usage**
    
*   **Logged-in users**
    
*   **Last boot time**
    

Sample script:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   #!/bin/bash  wall "Monitoring:  -----------------  CPU Load: $(top -bn1 | grep load | awk '{printf "%.2f", $(NF-2)}')  Memory Usage: $(free -m | awk 'NR==2{printf "%.2f", $3*100/$2 }')  Disk Usage: $(df -h | awk '$NF=="/"{printf "%s", $5}')  Last boot: $(who -b | awk '{print $3, $4}')"   `

This script is added to a cron job for execution every 10 minutes.

Cron Jobs
---------

I set up a **cron job** to run the monitoring script:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   crontab -e   `

Add the following line:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   */10 * * * * /path/to/monitoring_script.sh   `

Bonus Features
--------------

As part of the bonus, I implemented:

*   **Fail2Ban**: To protect the SSH server from brute-force attacks.
    
*   **Custom Bash Prompt**: A personalized and more informative bash prompt.
    
*   **Additional Partitioning**: More complex partition setups for /var and /tmp.
    

Conclusion
----------

The **Born2beRoot** project was a great introduction to system administration and security practices. By completing this project, I gained hands-on experience with setting up and securing a Linux server, managing users, configuring SSH, and ensuring the system is monitored and secured with firewall rules and cron jobs.
