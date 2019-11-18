## Ansible Role: Docker Image build and push (optional) to AWS ECR

An Ansible Role that builds Docker container images and  pushes to AWS ECR Repositories.

## Requirements
  - Ansible
  - Python
  - Docker
  - Pip, Pip3, Boto, Boto3

## Structure:

![w3DockerPush](https://i.imgur.com/7pZdx7e.png)

## AWS Credential Menagement:

#### Way1: 

Edit file `~/.aws/credentials` , if not exists create and put entry like below.

    # nano ~/.aws/credentials 
    [default]
    AWS_ACCESS_KEY_ID=AKIAYCOxxxxxxRXRFCEWWF
    AWS_SECRET_ACCESS_KEY=W1pZxxxCnfydRgp2DBNIxxxxxxxxxNcFqX1Kn3vZ
   
For first time you may need to reload the `bash`.

##### Way2: Export it as environmental variable as follows:

    export AWS_ACCESS_KEY_ID=xxxxxxxxxxxxxx
    export AWS_SECRET_ACCESS_KEY=xxxxxxxxxxxxxxxxxxxxxxx
    export ANSIBLE_HOST_KEY_CHECKING=False  ( Optional )


## Role Variables files.

Available variables are listed in 

    dockerrunpush.yaml

During Ansible run this file will be called.

The **vars**: declaration control all the required variables to create the Image and push (optional) to AWS ECR. 

##### # date: required to add date-time tag with Image
 `date: "{{ lookup('pipe', 'date +%Y%m%d-%H%M') }}"`

##### # This directory is used to look for Dockerfile to create images and others necessary files to copy during image creation.
    ecr_image_src_dir: dockerfile

###### # Name for the ECR repository 
    ecr_image_name: ansible

    
##### # here is the image tag, do not change anything in {{ date }}
    ecr_image_tags: ['latest-{{ date }}']

##### # ecr_region: region where the image to upload.
    ecr_region: us-west-2

##### # AWS ECR account id, where the image will be pushed.
    ecr_account_id: '555000555000'


## The file defaults/main.yaml give some required instruction during Ansible run. 


##### # Image build options.
    ecr_image_buildargs: {}

##### # Set this to true if you need to pull from ECR for the image build.
    ecr_login_required: false

##### # Whether to push the built image to ECR.
    ecr_push: true

##### # AWS account details for ECR.
    ecr_url: "{{ ecr_account_id }}.dkr.ecr.{{ ecr_region }}.amazonaws.com"


## The directory  "tasks":

contains two *.yaml file, describes all the task, those will be perform during Ansible run.


## Installation instruction.

Install this role in Ansible default /role folder. Find the role folder declaration in `/etc/ansible/ansible.cfg` . You may download as zip and unzip that in the Ansible `role` folder. 
##### # additional paths to search for roles in, colon separated
`# roles_path    = /etc/ansible/roles` 

You may change the roles path in here.

**For Run**

Go to the folder `/path/to/roles/w3docker-ecr-build`  make changes on `dockerrunpush.yaml` - vars and `defaults/main.yaml` file and execute the following command. 

    ansible-playbook dockerrunpush.yaml

This will create docker image and optionally push the image to **AWS-ECR**.