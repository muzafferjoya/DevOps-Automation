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

```bash
---
- name: Create users and configure SSH access
  hosts: all
  vars:
    users:
      - username: "devopsuser"
        ssh_key: "ssh-rsa XXXXXXXXX..."
      - username: "example"
        ssh_key: "ssh-rsa XXXXXXXXX..."

  tasks:
    - name: Create users
      user:
        name: "{{ item.username }}"
        state: present
      loop: "{{ users }}"

    - name: Set up SSH authorized keys
      authorized_key:
        user: "{{ item.username }}"
        key: "{{ item.ssh_key }}"
        state: present
      loop: "{{ users }}" 
```         

3. **Firewall Configuration**: Sets up UFW to allow only necessary ports, enhancing security.
   - Installs UFW, allows SSH, HTTP, and HTTPS traffic, and enables the firewall.

```bash
---
- name: Configure firewall with UFW
  hosts: all
  tasks:
    - name: Install UFW
      apt:
        name: ufw
        state: present

    - name: Allow SSH
      ufw:
        rule: allow
        port: '22'

    - name: Allow HTTP and HTTPS
      ufw:
        rule: allow
        port: "{{ item }}"
      loop:
        - '80'
        - '443'

    - name: Enable UFW
      ufw:
        state: enabled 
```           

4. **System Updates**: Automates regular updates and patching across servers.
   - Updates the package cache and upgrades all installed packages.
```bash
---
- name: Update and upgrade packages
  hosts: all
  tasks:
    - name: Update apt cache
      apt:
        update_cache: yes

    - name: Upgrade all packages
      apt:
        upgrade: dist
```           

5. **Web Server Setup**: Installs and configures Nginx for hosting web applications.
   - Installs Nginx, starts the service, and ensures it runs on boot.
```bash
---
- name: Install and configure Nginx
  hosts: web_servers
  tasks:
    - name: Install Nginx
      apt:
        name: nginx
        state: present

    - name: Start and enable Nginx service
      service:
        name: nginx
        state: started
        enabled: yes
```           

6. **Database Server Setup**: Installs and secures MySQL databases.
   - Installs MySQL and performs initial security setup, including setting the root password.
```bash
---
- name: Install and secure MySQL
  hosts: db_servers
  tasks:
    - name: Install MySQL
      apt:
        name: mysql-server
        state: present

    - name: Secure MySQL installation
      mysql_secure_installation:
        login_password: ""
        root_password: "new_password"
        change_root_password: yes
        remove_anonymous_users: yes
        disallow_root_login_remotely: yes
        remove_test_db: yes
```        

7. **Automated Backups**: Sets up rsync for syncing data to a backup server.
   - Installs rsync and sets up a job to sync data to a remote backup server.
```bash
---
- name: Set up automated backups
  hosts: all
  vars:
    backup_server: "backup.example.com"
    backup_path: "/backup"
    src_path: "/data"

  tasks:
    - name: Install rsync 
      apt:
        name: rsync
        state: present

    - name: Sync data to backup server
      command: "rsync -avz {{ src_path }} {{ backup_server }}:{{ backup_path }}"
```         

8. **Load Balancer Configuration**: Installs and configures HAProxy for traffic management.
   - Installs HAProxy and uses a template file for configuration.

```bash
---
- name: Set up HAProxy as a load balancer
  hosts: lb_servers
  tasks:
    - name: Install HAProxy
      apt:
        name: haproxy
        state: present

    - name: Configure HAProxy
      template:
        src: haproxy.cfg.j2
        dest: /etc/haproxy/haproxy.cfg

    - name: Restart HAProxy
      service:
        name: haproxy
        state: restarted 
```

9. **Docker Installation**: Automates the installation and configuration of Docker.
   - Installs required dependencies, adds the Docker repository, and installs Docker.

```bash
---
- name: Install Docker
  hosts: all
  tasks:
    - name: Install Docker dependencies
      apt:
        name: "{{ item }}"
        state: present
      loop:
        - apt-transport-https
        - ca-certificates
        - curl
        - software-properties-common

    - name: Add Docker GPG key
      apt_key:
        url: https://download.docker.com/linux/ubuntu/gpg
        state: present

    - name: Add Docker repository
      apt_repository:
        repo: deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable
        state: present

    - name: Install Docker
      apt:
        name: docker-ce
        state: present 
```        

10. **Monitoring Setup**: Installs Prometheus Node Exporter for system health monitoring.
    - Downloads, installs, and configures Node Exporter to monitor system metrics.

```bash
---
- name: Install Prometheus Node Exporter
  hosts: all
  tasks:
    - name: Download Node Exporter
      get_url:
        url: https://github.com/prometheus/node_exporter/releases/download/v1.3.1/node_exporter-1.3.1.linux-amd64.tar.gz
        dest: /tmp/node_exporter.tar.gz

    - name: Extract Node Exporter
      unarchive:
        src: /tmp/node_exporter.tar.gz
        dest: /opt
        remote_src: yes

    - name: Create Node Exporter service
      copy:
        dest: /etc/systemd/system/node_exporter.service
        content: |
          [Unit]
          Description=Prometheus Node Exporter
          [Service]
          ExecStart=/opt/node_exporter-1.3.1.linux-amd64/node_exporter
          [Install]
          WantedBy=default.target

    - name: Start and enable Node Exporter
      service:
        name: node_exporter
        state: started
        enabled: yes
```            

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