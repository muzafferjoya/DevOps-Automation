# Ansible Automation Scripts for Infrastructure Management

## Description

This repository contains a collection of Ansible automation scripts designed to streamline infrastructure management tasks. These playbooks automate essential processes such as server provisioning, user management, firewall configuration, system updates, and monitoring setup, ensuring a reliable and secure infrastructure.

## Features

1. **Server Provisioning**: Automates the installation of essential packages on new servers.
   - Updates package cache and installs packages like vim, curl, and git.

```bash
---
- name: Provision servers with essential packages
  hosts: all
  tasks:
    - name: Update apt cache
      apt:
        update_cache: yes

    - name: Install essential packages
      apt:
        name:
          - vim
          - curl
          - git
        state: present 
```

2. **User Management**: Creates user accounts and configures SSH access securely.
   - Defines users in a variable and iterates through them to create accounts and configure SSH keys.

3. **Firewall Configuration**: Sets up UFW to allow only necessary ports, enhancing security.
   - Installs UFW, allows SSH, HTTP, and HTTPS traffic, and enables the firewall.

4. **System Updates**: Automates regular updates and patching across servers.
   - Updates the package cache and upgrades all installed packages.

5. **Web Server Setup**: Installs and configures Nginx for hosting web applications.
   - Installs Nginx, starts the service, and ensures it runs on boot.

6. **Database Server Setup**: Installs and secures MySQL databases.
   - Installs MySQL and performs initial security setup, including setting the root password.

7. **Automated Backups**: Sets up rsync for syncing data to a backup server.
   - Installs rsync and sets up a job to sync data to a remote backup server.

8. **Load Balancer Configuration**: Installs and configures HAProxy for traffic management.
   - Installs HAProxy and uses a template file for configuration.

9. **Docker Installation**: Automates the installation and configuration of Docker.
   - Installs required dependencies, adds the Docker repository, and installs Docker.

10. **Monitoring Setup**: Installs Prometheus Node Exporter for system health monitoring.
    - Downloads, installs, and configures Node Exporter to monitor system metrics.

## Getting Started

### Prerequisites

- Ansible installed on your control machine.
- Access to the target servers with SSH.

### Usage

1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/ansible-automation-scripts.git
   cd ansible-automation-scripts
```