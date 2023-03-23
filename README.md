
# Project-10
## :ledger: Index

- [About The Project](#beginner-about-the-project)
- [Problem that this project solves ](#question-problem-that-this-project-solves)
- [Solution to the problem.](#key-solution-to-the-problem)
- [Tools and Services](#hammer_and_wrench-tools-and-services)
- [Architecture of this project](#house-architecture-of-this-project)
- [Steps to execute the project](#zap-steps-to-execute-the-project)
  - [Login to AWS Account ](#key-login-to-aws-account)
  - [Create ec2 instance to run ansible playbook](#package-create-ec2-instance-to-run-ansible-playbook)
  - [Install Ansible Manually](#package-install-ansible-manually)
  - [Setup ec2 Role for ansible](#package-setup-ec2-role-for-ansible)
  - [Create a project directory](#package-create-a-project-directory)
  - [Execute a sample cloud task](#package-execute-a-sample-cloud-task)
  - [Create Variables File for VPC & Bastion host](#package-create-variables-file-for-vpc-and-bastion-host)
  - [Create VPC Setup Playbook](#package-create-vpc-setup-playbook)
  - [Create Bastion setup playbook](#package-create-bastion-setup-playbook) 
  - [Site.yml playbook to call both playbook at once](#package-site.yml-playbook-to-call-both-playbook-at-once)
- [Resources](#page_facing_up-resources)
- [Credit/Acknowledgment](#star2-creditacknowledgment)


## :beginner:About The Project

<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>

## :question: Problem that this project solves 

<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>

## :key: Solution to the problem.

<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>

## :hammer_and_wrench: Tools and Services
- visual studio code or an IDE
- AWS Account
- GitHub account
- Git

<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>


## :beginner: Architecture of this project.

![Project Image](project-image-url)

<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>

## :zap: Steps to Execute the project. 

<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>

### :key: Login to AWS Account

- Login to your AWS management console and choose ohio region for this project. Create an AWS account if you don't have one. 

 ```sh
Account ID (12 digits) or account alias: <Enter your credentials>
IAM user name: <Your IAM user name>
Password: <Your password>
   ```
   
<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>



### :package: Create ec2 instance to run ansible playbook

- On your console under `EC2`->`Instances` click `launch instances`.
- We will create ec2 instance to run ansible playbook with below details.

 ```sh
Name: Control-Machine
AMI: Ubuntu 20 and above
Instance Type: t2.micro
Security Group Nmae: Ansible-sg
Security group rules: allow SSH on port 22 from MYIP
 key pair: Ansible-ohio-key
   ```
- Let;s launch our Ansible machine with some userdata as given below.

```sh
#!/bin/bash
apt update
apt install ansible -y
   ```
   
- SSH into your Ansible machine check your version of ansible .

 ```sh
ssh -i <directory of your key pair/key pair> ubuntu
ansible --version
   ```
   

     
<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>

### :package: Install Ansible Manually 

- If you are using ubuntu 18 you have to install ansible manually from the ansible repository.
- Checkout [ansible AWS documentation here](https://ansible-docs.readthedocs.io/zh/stable-2.0/rst/guide_aws.html)

<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>


### :package: Setup ec2 Role for ansible
   
- On the console, search for  EC2 instance service. Goto `Actions` --> `Security` --> `Modify IAM Role`
- Create an IAM role for our Ansible machine to use with the following details.

```sh
Service: EC2
Policy: AdministratorAccess
Role name: ansible-admin
   ```
- Install AWS CLI on your ansible machine and test your authentication using the command below 


```sh
sudo apt install awscli -y
aws sts get-caller-identity
   ```

<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>

### :package: Create a project directory
- On your Ansible machine, create a new directory with the following details.

```sh
mkdir vpc-stack-vprofile
cd vpc-stack-vprofile
vim test-aws.yml
   ```
   
<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>

### :package: Execute a sample cloud task 
- Checkout [ansible AWS documentation here](https://docs.ansible.com/ansible/2.9/modules/list_of_cloud_modules.html) we coppied our sample cloud task below from there.
- Open the file `test-aws.yml` with this command `vim test-aws.yml` paste the content below.Make sure you take care of the indentation

```sh
- hosts: localhost
  connection: local
  gather_facts: False
  tasks:
    - name: sample ec2 key
      ec2_key:
        name: my_keypair
        region: us-east-2
   ```
- Try runing this playbook with the command `ansible-playbook test-aws.yml`, you will notice that it faills because python3 is not installed on your Ansible machine.

![Project Image](project-image-url) 

### :package: Install boto

- On AWS bot is the SDK for python.
- Lt's install bot with the following command 
- 
```sh
sudo apt search boto
sudo apt install python3-boto3 -y
   ```
- Run the playbook again,  you will notice that it is successfull because boto3 is installed.

- Add more tasks to our `test-aws.yml` playbook as given below.

```sh
- hosts: localhost
  connection: local
  gather_facts: False
  tasks:
    - name: sample ec2 key
      ec2_key:
        name: my_keypair
        region: us-east-2
      register: keyout

    - debug:
        var: keyout

    - name: store login key
      copy:
        content: "{{keyout.key.private_key}}"
        dest: ./sample-key.pem
      when: keyout.changed
   ```
   
- Our key pair is now created and store stored. In our first task, the key pair was created but not store by adding the above lines to our code, we can now store our key pair. 


<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>

### :package: Create Variables File for VPC and Bastion host

- Login into your github account.
- Let's create a github repository were our ansible-playbooks will be store. Called the repo `ansible-aws-vpc`

- On your IDE, create 2 two variables file `vpc_setup` and s `bastion_setup`.



Inside the `vpc_setup file`, copy and paste the code below.


```sh
vpc_name: "Vprofile-vpc"

#VPC Range
vpcCidr: '172.20.0.0./16'

#Subnets Range
PubSub1Cidr: 172.20.1.0/24
PubSub2Cidr: 172.20.2.0/24
PubSub3Cidr: 172.20.3.0/24
PrivSub1Cidr: 172.20.4.0/24
PrivSub2Cidr: 172.20.5.0/24
PrivSub3Cidr: 172.20.6.0/24

#Region Name
region: "us-east-2"

#Zone Names
zone1: us-east-2a
zone2: us-east-2b
zone3: us-east-2c

state: present
   ```
   
- Inside the `bastion_setupe`, copy and paste the code below.

```sh
bastion_ami: ami-0beaa649c482330f7 # Amazon Linux-2 AMI-ID from us-east-2 region
region: us-east-2
MYIP: IP_address_of_your_laptop/32
keyName: vprofile-key
instanceType: t2.micro
   ```
   
- Commit and push these two files to GitHub, also clone this repository in our Ansible machine on AWS
 
<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>

### :package: Create VPC Setup Playbook

- Create a `vpc_setup.yml` playbook with the details below

```sh
- hosts: localhost
  connection: local
  gather_facts: False
  tasks:
    - name: Import VPC Variables
      include_vars: vars/vpc_setup

    - name: Create Vprofile VPC
      ec2_vpc_net:
        name: "{{vpc_name}}"
        cidr_block: "{{vpcCidr}}"
        region: "{{region}}"
        dns_support: yes
        dns_hostnames: yes
        tenancy: default
        state: "{{state}}"
      register: vpcout
   ```
   
- Commit and push this file to GitHub, also pull this file in our Ansible machine on AWS

- RUN the play book using the following command on your AWS Ansible machine 

```sh
ansible-playbook vpc_setup.ymlo
   ```
   
  ![Project Image](project-image-url)
  
- On your AWS Console search for VPC service to view changes


<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>

#### :package: Create a Subnet Playbook

<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>

#### :package: Create Internet Gateway and  Public Route Table Playbook

<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>

#### :package: Create a NAT Gateway and Private Route Table Playbook

<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>

### :package: Create Bastion setup playbook

<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>

### :package: Site.yml playbook to call both playbook at once


<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>

## :page_facing_up: Resources

<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>


## :star2: Acknowledgment


<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>
