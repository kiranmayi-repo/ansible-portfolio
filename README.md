# Ansible Automation Portfolio

Welcome to my Ansible Automation Portfolio!  
This repository showcases my **hands-on experience** in automating infrastructure and deployments using **Ansible** and other DevOps tools.

---

## About Me
Hi, I’m **Kiranmayi**, a DevOps Engineer with experience in automation, CI/CD, and cloud infrastructure.

---

##  Skills
- **Automation:** Ansible, Jenkins, Terraform  
- **Containers:** Docker, Kubernetes  
- **Cloud Platforms:** AWS, Azure  
- **Scripting:** Bash, Python  
- **Monitoring:** Prometheus, Grafana  

---

##  Featured Playbooks

| Playbook | Description |
|-----------|--------------|
| [nginx-deployment.yml](playbooks/nginx-deployment.yml) | Installs and configures Nginx |
| [user-management.yml](playbooks/user-management.yml) | Manages Linux users and permissions |
| [database-backup.yml](playbooks/database-backup.yml) | Automates MySQL/PostgreSQL backups |

---


## Repository Structure

playbooks/    → My Ansible playbooks  
roles/        → Custom roles for automation  
inventory/    → Inventory files (dev, prod, etc.)  
docs/         → Documentation of skills and experience

##  How to Use

1. Clone this repo:
   bash
   git clone https://github.com/kiranmayi-repo/ansible-portfolio.git
   cd ansible-portfolio

2.Review the inventory files:

Navigate to the inventory/ folder

Modify the dev, staging, or prod file with your server IPs or hostnames

3.Run a playbook:

ansible-playbook -i inventory/dev playbooks/nginx-deployment.yml


4.(Optional) Run with a specific tag or limit:

ansible-playbook -i inventory/dev playbooks/nginx-deployment.yml --tags "install"


Check playbook syntax before running:

ansible-playbook playbooks/nginx-deployment.yml --syntax-check


GitHub https://github.com/kiranmayi-repo/ansible-portfolio.git


---

 





