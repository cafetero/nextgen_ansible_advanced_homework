Role Name
=========
El objetivo del laboratorio es configurar un ambiente de QA y un ambiente de producciÃn de la aplicaciÃn 3 tier app. 

# Desplegar el laboratorio de Ansible tower donde se configura tower en HA y el laboratorio de OpenStack

# Configurar la comunicacion entre Bastion del lab de Tower con Workstation del lab de OpenStack para que este ultimo sea el nodo aislado
- site-setup-prereqs 
##
-- setup-workstation
### Configuar llave "http://www.opentlc.com/download/ansible_bootcamp/openstack_keys/openstack.pub" , instalar python3, git, openstacksdk, /etc/openstack/clouds.yaml 
--- pre-tasks
##
-- osp-setup
### Configuar OSP, crear imagenes, sabores, llaves, red, security group
--- create-image.yml
--- create-flavor.yml
--- create-keypair.yml
--- create-network.yml
--- create-sg.yml
## Configurar workstation como nodo aislado de ansible con usuario cloud-user y llave openstack.pem,script /root/ansible-tower-setup-latest/setup.sh
-- site-install-isolated-node


# Configurar los jobs y workflows en tower
- site-config-tower.yml
##
-- config-tower
### Copiar variables y llaves
--- pre-config-tower.yml
### __Crear proyecto a mano en tower 00_Homework_Asignment relacionando el repositorio de GIT https://github.com/cafetero/nextgen_ansible_advanced_homework.git
### crear credenciale en tower, crear inventario, grupo, agregar workstation y asociarlo a un grupo, llaves
--- post-config-tower.yml
### Crear inventario dinamico, crear grupos, crear tags, crear grupos relacionados con los tags, mas relaconado con Amazon
--- ec2_dynamic.yml
### Crear templates en tower, template para desplegar instancias, vault password, template para deslegar 3 tier app en QA, template para llaves en AWS
--- job_template.yml
### Crear el workflow uniendo todos los templates creados
--- workflow_template.yml


# playbook para desplegar las instancias
- site-osp-instances
## __ Crear las intancias, __ este archivo se creo desde cero
-- osp-servers
### informacion de los servidores a desplegar con las llaves
--- var/main.yml

# playbook para desplegar los servicios
- site-3tier-app
## obtener informacion de las instancias y agregarlas a un nventario
-- osp-facts
## ___habilitar sudo, configurar repositorio, instalar python, git, ansible
-- base-config
## __Configurar balanceador, archivo creado desde cero
-- lb-tier
## __Configurar app, archivo creado desde cero
-- app-tier
## __configurar base de datos, archivo creado desde cero
-- db-tier

# ___probar que los servidores de QA tienen configurado el mensaje en html 
- site-smoke-osp

# ___si la prueba de QA falla se borran las instancias de OpenStack
- site-osp-delete

# ___desplegar el ambiente, cambiar el nombre del lab "Ansible Advanced - Three Tier App"
- aws_provsion

# ___probar que los servidores de produccion funconan correctamente 
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
