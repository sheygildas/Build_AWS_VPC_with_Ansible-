
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

<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>

### :package: Execute a sample cloud task 

<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>

### :package: Create Variables File for VPC and Bastion host

<br/>
<div align="right">
    <b><a href="#Project-10">↥ back to top</a></b>
</div>
<br/>

### :package: Create VPC Setup Playbook

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
