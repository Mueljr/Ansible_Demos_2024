**ANSIBLE PROJECT TO DEMONSTRATE PROVISIONING AND CONFIGURATION MANAGEMENT**

Beyond being a configuration management tool as it's popular for, Ansible is also a provisioning tool, a deployment tool (CI/CD operations), and a network automation tool. With these significant capabilities, it's safe to refer to Ansible as an **_Automation Platform._**

This particular project aims to demonstrate provisioning and configuration management for what has been provisioned. Three tasks make up this project.

**TASK 1 - PROVISIONING**

3 EC2 instances are created on AWS using Ansible loops.

2 Instances with Ubuntu distribution, 1 instance with CentOS distribution.

These 3 instances are provisioned on the Ansible Control Node with a local connection specified on the playbook.

**PREREQUISITES:**

On Control Node, install:
- Ansible
  
- Boto3
  
- AWS CLI and authenticate your AWS account.
  
- AWS Collection for Ansible Galaxy.

**STEPS:**

1. Log in to your IAM user account.

2. Ensure EC2 Full Access permissions are granted.

3. Create an access key and a secret access key and store safely.

4. Setup Vault - Create password for vault with the vault command. Edit your pass.yaml file and store your AWS sensitive credentials in there.

5. Create a playbook - Copy an EC2 module from Ansible's documentation or Ansible Galaxy. Ensure EC2 playbook has at least 1 distribution. Ensure the host is set to local host, since we want to provision an Ansible module as copped from Ansible Galaxy. Also ensure the connection is set to local, since the provisioning is done on the control node at first.

6. Duplicate the EC2 configuration using Ansible loops. So, having added the first EC2 configuration, the 'loop' configuration is added at the end with 3 different image ID's depending on the distribution requested for.

7. Ensure the name and image id tabs are added with variables - item.name; item.image. Use 'environment' tags in place of the 'name' tag in case it doesn't execute for all three instances.
   
This is to avoid the concept of **Idempotency.** Because if run without variables applied, only two instances will be created, because naturally, if two commands are sent without any slight change, Ansible executes the first and skips the second, because some state file is already available on target.

9. Execute the playbook with your vault command, since the access and secret access keys are required to complete such operation.
 

**TASK 2 - PASSWORDLESS AUTHENTICATION**

There are two notable methods to go about this - the _SSH Copy ID_ method, and the _Password_ method.

In this case, the SSH Copy ID method will be used.

**STEPS:**

1. We use the PC's terminal here, rather than an IDE. MobaXterm is a good bet for running Ubuntu on Windows, or the inbuilt terminal for Mac, if running on Mac. Why? The terminal directly accesses your local SSH agent and key management. Also, the terminal uses the correct environment for SSH configurations.

2. Download the .pem file from your AWS console on your local machine.

3. Allow SSH traffic for all 3 EC2 instances from the inbound rules settings.

4. Run SSH passwordless authentication for all 3 instances - one at a time.
ssh-copy-id -f "-o IdentityFile ~/Path/File" nameofinstance@PublicIPofInstance

5. Verify is rightly authenticated: _ssh name@PublicIP_


**TASK 3 - CONFIGURATION MANAGEMENT**

The final task is to automate the shutdown of **_Ubuntu_** instances only, using Ansible Conditionals. 

**STEPS:**

1. Create an inventory.ini file. The reason is since we want to connect to the instances, after setting up a passwordless authentication, so we need an inventory of our Ansible Manage Nodes.
N.B: The control node communicates with the manage node, but the manage node is only active once there's an 'inventory' for the control node to access.

Since we want to use the **_when_** condition here, we are not going to group the instances in the inventory file, but keep three of them joint. If not using the when condition, we could have separated Ubuntu instances from CentOS, easily.

2. Create another playbook for configuration management. Hosts is written for 'all' since we want to apply the tasks defined to all the hosts (instances) written in the inventory file. Likewise, we use "become: true" to enable elevated permissions.

3. Ensure in the tasks configuration, the ansible.builtin.command aligns with the one to shutdown an instance - _ansible.builtin.command: /sbin/shutdown -t now_

4. Ensure the _when_ condition is configured using _ansible_facts_.
   
_ansible_facts [os_family] == "Debian"_

This is because we want the condition of the task to apply when it has discovered any host/instance from the Debian OS family - Ubuntu in this case.
