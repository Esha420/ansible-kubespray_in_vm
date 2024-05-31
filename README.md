# KubeSpray Setup Using Ansible

This guide provides step-by-step instructions to set up a Kubernetes cluster using KubeSpray and Ansible on three EC2 instances.

## Requirements

- Three EC2 instances with the following IP addresses and their respective PEM keys:
  - `100.25.221.183` -> `spare.pem` [node1]
  - `54.157.138.114` -> `spare1.pem` [node2]
  - `100.25.24.39` -> `spare2.pem` [node3]

## Steps

### 1. Set Permissions for PEM Files

1. Open the folder where you have downloaded the PEM files for your EC2 instances.
2. Open PowerShell in this folder.
3. Grant the necessary permissions to the PEM files using the following commands, replacing `Owner` with the name of your PC:

   ```powershell
   icacls '.\spare.pem' /grant Owner:R
   icacls '.\spare1.pem' /grant Owner:R
   icacls '.\spare2.pem' /grant Owner:R

4. Transfer the PEM files to node1 using the following scp commands:
scp -i .\spare.pem .\spare.pem ec2-user@100.25.221.183:/home/ec2-user
scp -i .\spare.pem .\spare1.pem ec2-user@100.25.221.183:/home/ec2-user
scp -i .\spare.pem .\spare2.pem ec2-user@100.25.221.183:/home/ec2-user

5. SSH into node1 using the following command:
ssh -i .\spare.pem ec2-user@100.25.221.183

6. On node1, navigate to /home/ec2-user and set appropriate permissions for the PEM files:
chmod 400 spare.pem spare1.pem spare2.pem

7. Update the package lists and install Ansible:
sudo yum update -y
sudo yum install ansible -y

8. Install Git:
sudo yum install git -y

9. Clone the repository and navigate into it:
git clone https://github.com/Esha420/ansible_kubespray.git
cd ansible_kubespray

10. Edit the machine.ini file to reflect the IP addresses of your nodes. Ensure the IP addresses of node1, node2, and node3 are correctly configured.

11. Run the playbook to set up the Kubernetes cluster:
ansible-playbook -i machine.ini main.yml


### Explanation of the README Steps:

- **Requirements**: Lists the necessary EC2 instances and their details.
- **Set Permissions for PEM Files**: Ensures the PEM files have the correct permissions on your local machine using PowerShell.
- **Transfer PEM Files to Node1**: Uses `scp` to securely copy the PEM files to `node1`.
- **SSH into Node1**: Provides the command to SSH into `node1`.
- **Set Permissions on Node1**: Ensures the PEM files on `node1` have the correct permissions.
- **Install Ansible on Node1**: Commands to install Ansible on `node1`.
- **Install Git**: Commands to install Git.
- **Clone the Repository**: Commands to clone the Git repository and navigate into the project directory.
- **Configure the Inventory**: Instructions to edit the `machine.ini` file with your specific node IP addresses.
- **Run the Ansible Playbook**: Command to execute the Ansible playbook that sets up the Kubernetes cluster.
