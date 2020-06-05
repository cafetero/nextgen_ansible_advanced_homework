Homework description
=========
The goal of the lab is to deploy a Tower environment, an openstack environment, and use the openstack workstation as an isolated node to deploy a QA environment from the demo 3 Tier App, then deploy the production environment of the 3 Tier App on amazon and finally configure everything in a workflow in tower.


# Config labs to work togeter 

|-- site-setup-prereqs | Config Bastion-Workstation communication to use Workstation as isolated node
|  |
|  |-- setup-workstation
|  |   |-- pre-tasks | Config keys, install python3, git, openstacksdk, /etc/openstack/clouds.yaml 
|  |-- osp-setup | Config OSP, create imagenes, flavors, keys, network, security group
|  |   |-- create-image.yml
|  |   |-- create-flavor.yml
|  |   |-- create-keypair.yml
|  |   |-- create-network.yml
|  |   |-- create-sg.yml
|  |-- site-install-isolated-node | Config workstation as isolated node with cloud-user and openstack.pem key 


# Config jobs and workflows on tower
|-- site-config-tower.yml
|   |-- config-tower
|   |   |-- pre-config-tower.yml | Copy vars and keys
|   |   |-- post-config-tower.yml | correcto Add project in script, create credentials, inventory, groups, keys
|   |   |-- ec2_dynamic.yml | Create dynamic inventory, tags, groups..
|   |   |-- job_template.yml | Create templates to depoy instances, config vault passwords, deploy 3 tier app 
|   |   |-- workflow_template.yml | Create Workflow

# Deploy OSP Instances 
|-- site-osp-instances
|   |-- osp-servers | Create Instances, PLAYBOOK BUILD FROM SCRATCH
|   |   |-- var/main.yml | Servers to deploy

# Deploy 3 Tier App
|-- site-3tier-app
|   |-- osp-facts | Get OSP Instances facts and add to inventory
|   |-- base-config | Enable sudo, config repository, install python, git, ansible
|   |-- lb-tier | Config load balancer, PLAYBOOK BUILD FROM SCRATCH
|   |-- app-tier | Config App, PLAYBOOK BUILD FROM SCRATCH
|   |-- db-tier | config DB, PLAYBOOK BUILD FROM SCRATCH

# Test QA Servers 
|-- site-smoke-osp | BUILD FROM SCRATCH

# Delete OSP Instances if smoketest fail 
|-- site-osp-delete

# Deploy production 
- aws_provsion | Change Lab name "Ansible Advanced - Three Tier App"

# Test production  
- site-smoketest-aws




Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Se configuran las varialbes de ambiente con el archivo labrc

>> source ~/labrc

Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
