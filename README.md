🚀 **Ansible Role: Apache Guacamole Docker Deployment with HAProxy & SSL**

📌 **Overview**

This Ansible role automates the deployment of Apache Guacamole with HAProxy as a reverse proxy and a self-signed SSL certificate for secure remote access.

✅ **Features**

✔️ Deploys Guacamole using Docker & Docker Compose

✔️ Sets up MariaDB as the database backend

✔️ Configures HAProxy for load balancing and SSL termination

✔️ Generates a self-signed SSL certificate for HTTPS access

✔️ Initializes the Guacamole database schema

✔️ Ensures all services are running and configured properly

📋 **Prerequisites**

🛠 **Supported Operating Systems (Tested)**

Debian 12+

⚙️ **Required Dependencies**

  Ensure the control machine has :
  
    Ansible 2.10+
  
    Python 3.6+
  
    Ansible collection community.docker
  
    Ansible collection community.crypto
  
  Ensure the target machine has :
  
    Internet access (for package downloads)
  
    Docker and Docker Compose installed

🔑 **Privileges**

    Run playbooks as a user with sudo privileges and SSH key-based authentication.
    
    In this role, the default user is control.

🚀 **Quick Start Guide**

1️⃣ **Install the Role**

Clone this repository or download it:

    git clone https://github.com/Kharune/ansible_guacamole_docker.git
    cd ansible_guacamole_docker

2️⃣ **Configure Inventory, Playbook, and Ansible Configuration**

Before running the playbook, ensure the following files are properly configured based on your environment.

Inventory Configuration (hosts.yml)

    lab:
      vars:
        ansible_python_interpreter: auto_silent
      hosts:
        192.168.253.130:22450  # Change this based on your environment
    preprod:
      children:
        lab:

Playbook Configuration (PB_deploy_guacamole.yml)

    ---
    - name: Deploy Guacamole with HAProxy & SSL
      hosts: preprod  # Change this based on your target group
      remote_user: control  # Change this based on your user
      become: true
      roles:
        - guacamole_docker_with_https

Ansible Configuration (ansible.cfg)

Ensure your Ansible configuration is set correctly :

    [defaults]
    inventory=/home/control/ansible_guacamole_docker/hosts.yml  # Change to your inventory file
    transport=paramiko  # Change if you don't use paramiko

3️⃣ **Run the Playbook**

Run the playbook :

    ansible-playbook PB_deploy_guac_services.yml

⚙️ **Role Variables**

Customize the role variables by modifying defaults/main.yml :

    # Database Configuration
    mysql_root_password: "your_password"
    mysql_database: "guacamole"
    mysql_user: "guacamole"
    mysql_password: "your_password"
    
    # SSL Configuration
    fqdn: "guacamole.example.com"
    ssl_cert_path: "/etc/ssl/certs/guacamole.pem"
    ssl_key_path: "/etc/ssl/private/guacamole.key"

🔒 **SSL & HAProxy Configuration**

This role automatically generates a self-signed SSL certificate for HAProxy :

    Certificate Path: /etc/ssl/certs/guacamole.pem
    
    Key Path: /etc/ssl/private/guacamole.key

To use a Let's Encrypt certificate instead :

    Replace the generated certificate with a valid Let's Encrypt cert.
    
    Update the HAProxy configuration to point to the new certificate paths.

    File : HAProxy Configuration (haproxy.cfg.j2)

This role configures HAProxy to :

    Terminate SSL at the proxy
    
    Forward traffic to Guacamole
    
    Redirect HTTP to HTTPS

🥑 **Demo**



https://github.com/user-attachments/assets/44d221ad-393d-42fe-8988-f01f2b84b1a3



🥑 **Apache Guacamole default credential : guacadmin/guacadmin**
