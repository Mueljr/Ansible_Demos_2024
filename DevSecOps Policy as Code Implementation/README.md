**DEVSECOPS - POLICY AS CODE IMPLEMENTATION**

A policy is simply a rule put in place for the purpose of ensuring security and compliance of files and resources within an organization, or largely, within an environment.

**Policy as Code** is the act of programmatically updating policies so that it applies to your target resources, thus, ensuring security and compliance of those resources as indicated by an organization.

**DEMO - TO ENFORCE AWS S3 BUCKET VERSIONING POLICY**

Just as many resources in cloud platforms are required that the compliance and security guidelines are met, in this case, the buckets as created on the AWS platform will face a Policy as Code Implementation.

The goal is to ensure that every object stored in every bucket is secured, and most importantly, the versioning policy of all S3 buckets enabled to see that the compliance for this is met in an organization.

**PREREQUISITES**

1.  AWS Configure - Locally connect your AWS account to your terminal.

2.  Install Python and Boto3 Modules - To allow Ansible make an API call to your AWS platform.

3.  Install Ansible Galaxy Collection - To allow the use of modules for different resources as found on the Ansible Galaxy platform.

**No need for passwordless authentication since we're not talking to virtual machines, but only the cloud server - AWS' API as in this case.

**STEPS**

1. Create and ensure your S3 buckets are active and in place.

2. Write a playbook to enforce AWS S3 bucket versioning policy.

**Step 1** - ensure the host is set on _localhost_ since we're running all operations on the control node.

**Step 2** - Specify your tasks.

Task 1 - List all the S3 buckets in your AWS account. Then store the buckets present in an array with the use of a variable - _register: result_

Task 2 - Ensure the name of each bucket as in the array is focused on by using a variable to represent the name - _name: "{{ item.name }}"_

Enable versioning for the very first bucket name found - _versioning: yes_

Finally, ensure the array all the buckets are stored is **looped** to perform the _focusing on name_, and _enabling versioning_ for subsequent S3 buckets found not to be enabled - _loop: "{{ result.buckets }}"_. Ideally, _loop: "{{ arrayvariable.nameofitem }}"_

3. Execute your playbook.

_ansible-playbook filename.yaml_
