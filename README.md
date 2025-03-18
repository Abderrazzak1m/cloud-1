# **Cloud-1: Deploying MariaDB, WordPress, and Nginx with Ansible on AWS**

## **Project Overview**
This project automates the deployment of three services (**MariaDB, WordPress, and Nginx**) in a **Docker environment** on an **AWS EC2 instance** using **Ansible**. It follows a **modular architecture** for better scalability and reusability.

---
## **1. Clone the Project**
clone the Ansible project to your local machine:
```
git clone https://github.com/Abderrazzak1m/cloud-1.git
cd cloud-1
```
---
## **2. AWS Infrastructure Setup (Using AWS Console)**

Before running the Ansible playbooks, you need to manually create an **AWS EC2 instance** with the required configurations:

### **2.1 Create a Security Group**
1. Navigate to **AWS Console → EC2 → Security Groups**.
2. Click **Create security group** and set the following rules:

   | Protocol | Port Range | Source |
   |----------|------------|--------|
   | SSH      | 22         | Your IP (recommended) |
   | HTTP     | 80         | 0.0.0.0/0 |
   | HTTPS    | 443        | 0.0.0.0/0 |

3. Click **Create security group** and note the **Security Group ID**.

### **2.2 Create a Key Pair**
1. Go to **AWS Console → EC2 → Key Pairs**.
2. Click **Create Key Pair**.
3. Choose:
   - **Name**: `cloud-1-key`
   - **Key type**: RSA
   - **Private key format**: `.pem`
4. Click **Create key pair** and download the `.pem` file then move it to the home directory of your Ansible project:
```
mv ~/Downloads/cloud-1-key.pem ~/cloud-1/
chmod 600 ~/cloud-1/cloud-1-key.pem 
```

### **2.3 Creating an EC2 Instance**
1. Go to AWS Console → EC2 → Launch Instance
2. Select Ubuntu 22.04 LTS as the OS
3. Choose instance type t2.micro (Free Tier eligible)
4. Attach the previously created security group
5. Select the key pair (cloud-1-key.pem)
6. Launch the instance

---

## **3. Inventory File (hosts.yaml)**
The inventory file defines the remote servers (VMs) that Ansible will manage. In this project, the inventory is stored in YAML format for better readability.
### **3.1 Example Inventory File (`hosts.yaml`)**

```
all:
  hosts:
    vm1:
      ansible_host: "{{ secrets.vm1.host }}"
      ansible_user: "{{ secrets.vm1.user }}"
      ansible_ssh_private_key_file: "{{ secrets.vm1.private_key }}"
      ansible_ssh_port: "{{ secrets.vm1.port }}"

```
## **4. Securing Secrets with Ansible Vault**
This project stores sensitive information (e.g., SSH keys, IPs, usernames) in an encrypted Ansible Vault file.
### **4.1 Example Secrets File**
Example `example-secrets.yaml`:
```
secrets:
  vm1:
    host: <vm1_ip>
    user: <vm1_user>
    private_key: <path_to_private_key>
    port: <vm1_ssh_port>
```

### **4.2 Creating a Secure Secrets File**
* To create a new encrypted secrets file, run:
```
ansible-vault create secrets.yaml
```
Add the actual credentials inside `secrets.yaml`, then save and exit.
* To edit the secrets file later:
```
ansible-vault edit secrets.yaml
```
* To encrypt an existing file:
```
ansible-vault encrypt secrets.yaml
```
* To decrypt:
```
ansible-vault decrypt secrets.yaml
```
## **5. Project Structure**
      cloud-1/
      │── README.md                   # Documentation  
      │── cleanup.yaml                 # Playbook to clean up resources  
      │── deploy.yaml                  # Main deployment playbook  
      │── example-secrets.yaml         # Example secrets file (unencrypted)  
      │── hosts.yaml                   # Ansible inventory file  
      │── roles/                       # Modular Ansible roles  
      │   ├── common/                  # Common setup tasks  
      │   │   ├── tasks/  
      │   │   │   ├── main.yaml        # Common setup tasks  
      │   ├── db/                      # Database setup (MariaDB)  
      │   │   ├── files/  
      │   │   │   ├── Dockerfile       # Dockerfile for MariaDB  
      │   │   │   ├── setup.sh         # Database initialization script  
      │   │   ├── tasks/  
      │   │   │   ├── main.yaml        # MariaDB deployment tasks  
      │   ├── nginx/                   # Nginx setup  
      │   │   ├── files/  
      │   │   │   ├── Dockerfile       # Dockerfile for Nginx  
      │   │   │   ├── default          # Default Nginx config  
      │   │   ├── tasks/  
      │   │   │   ├── main.yaml        # Nginx deployment tasks  
      │   ├── wp/                      # WordPress setup  
      │   │   ├── files/  
      │   │   │   ├── Dockerfile       # Dockerfile for WordPress  
      │   │   │   ├── setup.sh         # WordPress setup script  
      │   │   ├── tasks/  
      │   │   │   ├── main.yaml        # WordPress deployment tasks  
      │   ├── orchestrate/             # Docker Compose orchestration  
      │   │   ├── files/  
      │   │   │   ├── .env  
      │   │   │   ├── docker-compose.yaml  # Compose file for all services  
      │   │   ├── tasks/  
      │   │   │   ├── main.yaml        # Docker Compose deployment tasks  


## **6. Deploying the Infrastructure**
Once the EC2 instance is ready and secrets are configured, run the following steps:
1. **Verify connectivity** to the EC2 instance:
```
ansible -i hosts.yaml all -m ping --ask-vault-pass
```
2. **Deploy the services** (MariaDB, WordPress, and Nginx):
```
ansible-playbook -i hosts.yaml deploy.yaml --ask-vault-pass
```
3. **Verify running containers on the EC2 instance**:
```
ssh -i ansible-key.pem ubuntu@<vm1_ip>
docker ps
```
4. **Access WordPress**:
* Open your browser and navigate to:
```
http://<vm1_ip>
```
You should see the WordPress page.
## **7. Cleaning Up**
To remove the deployed containers and resources, run:
```
ansible-playbook -i hosts.yaml cleanup.yaml --ask-vault-pass
```
## Contact
If you have any issues or suggestions, feel free to reach out! 🚀
