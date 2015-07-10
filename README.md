# Kenzan Build Pipeline Documentation

This repository contains scripts that provision, build and deploy infrastructure into Amazon AWS.

## Setting up Amazon AWS

---

Pre-Requites

- Amazon AWS account credentials with enough permissions to deploy infrastructure into Amazon AWS
- Set Environment Variables – AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY

---

Setup

script 1 -> 'setup_vpc_subnets.yml'
	ansible-playbook -vvvv setup_vpc_subnets.yml
	ansible-playbook -vvvv setup_vpc_subnets.yml -e "aws_region=us-west-2" -e "az0=a" -e "az1=b"

	This script will setup VPC with subnets. The created vpc.id and subnets.id and updated to group_vars/all for the other scripts to refer


script 2 -> 'setup_sg_keypair.yml'
	ansible-playbook -vvvv setup_sg_keypair.yml

	This script will create Security Group in the previously created VPC and will also creates keypair (generates file 'jenkins_access.pem')
		Note: Need to have vpc_id updated to the group_vars/all (which is done by script 'setup_vpc_subnets.yml')


script 3 -> 'launch_jenkins.yml'
	ansible-playbook -vvvv launch_jenkins.yml
	This script is used to launch Amazon AWS ami and then Install and configure Jenkins on it
		Note: Need to have vpc_id and subnets.id updated to the group_vars/all (which is done by script 'setup_vpc_subnets.yml')

---

Provision other AMIs using Jenkins

- After launch_jenkins.yml to configure and launch main jenkins

- load Jenkin URL in browser

- Add git credentials to credential-store in jenkins
	Note:  add Description as "GitHub", as the same is being refered in Jenkins DSL groovy scripts

- Trigger job 'dsl-ami-provisioning', which will create other jenkins jobs to create/configure other base ami's
