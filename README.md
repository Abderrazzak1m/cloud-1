# **Cloud-1: Deploying MariaDB, WordPress, and Nginx with Ansible on AWS**

## **Project Overview**
This project automates the deployment of three services (**MariaDB, WordPress, and Nginx**) in a **Docker environment** on an **AWS EC2 instance** using **Ansible**. It follows a **modular architecture** for better scalability and reusability.

---

## **1. AWS Infrastructure Setup**

We will use the **AWS Management Console** to create the necessary resources.

### **1.1 Create a Security Group**
1. Navigate to **AWS Console → EC2 → Security Groups**.
2. Click **Create security group** and set the following rules:

   | Protocol | Port Range | Source |
   |----------|------------|--------|
   | SSH      | 22         | Your IP (recommended) |
   | HTTP     | 80         | 0.0.0.0/0 |
   | HTTPS    | 443        | 0.0.0.0/0 |

3. Click **Create security group** and note the **Security Group ID**.

### **1.2 Create a Key Pair**
1. Go to **AWS Console → EC2 → Key Pairs**.
2. Click **Create Key Pair**.
3. Choose:
   - **Name**: `cloud-1-key`
   - **Key type**: RSA
   - **Private key format**: `.pem`
4. Click **Create key pair** and download the `.pem` file to the **Ansible project’s home directory**.

### **1.3 Launch an EC2 Instance**
1. Navigate to **AWS Console → EC2 → Instances**.
2. Click **Launch Instance** and configure:
   - **AMI**: Ubuntu 22.04
   - **Instance Type**: `t2.micro`
   - **Key Pair**: Select `cloud-1-key`
   - **Security Group**: Select the one created earlier
   - **Storage**: 8GB (default)
3. Click **Launch Instance** and note the **public IP address**.

---

## **2. Configuring Ansible for Deployment**

### **2.1 Define Ansible Inventory (`hosts.yaml`)**
Your **Ansible inventory** file (`hosts.yaml`) should be structured as follows:

```
all:
  hosts:
    vm1:
      ansible_host: "{{ secrets.vm1.host }}"
      ansible_user: "{{ secrets.vm1.user }}"
      ansible_ssh_private_key_file: "{{ secrets.vm1.private_key }}"
      ansible_ssh_port: "{{ secrets.vm1.port }}"
```
## **3. Securing Secrets with Ansible Vault**
### **3.1 Create an Encrypted Secrets File**
To securely store secrets, we use Ansible Vault.

1. Navigate to your Ansible project directory.
2. Run the following command to create an encrypted file:
```
ansible-vault create secrets.yaml
```
3. Add your sensitive information in the following format:
```
secrets:
  vm1:
    host: <vm1_ip>
    user: <vm1_user>
    private_key: <path_to_private_key>
    port: <vm1_ssh_port>
```
4. Save and exit. Ansible will encrypt the file automatically.

### **3.2 Editing the Vault-Protected File**
```
ansible-vault edit secrets.yaml
```
### **3.3 Running Ansible with Vault-Protected Secrets**
Use the following command to run a playbook with Ansible Vault:
```
ansible-playbook -i hosts.yaml deploy.yaml --ask-vault-pass
```
## **4. Deploying Services with Ansible**
The playbook (deploy.yaml) will: 
  - ✅ Install Docker & dependencies
  - ✅ Deploy MariaDB, WordPress, and Nginx as Docker containers
  - ✅ Configure networking between services
```
ansible-playbook -i hosts.yaml deploy.yaml --ask-vault-pass
```
## **5. Conclusion**
This project automates AWS EC2 provisioning and Dockerized service deployment using Ansible with a secure and modular approach.
