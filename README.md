Role Name
=========

Rol para desplegar LAMP:

Linux, el sistema operativo
Apache, el servidor web
MySQL/MariaDB, el gestor de bases de datos
Perl, PHP, o Python, los lenguajes de programación


Requirements
------------

N/A

Role Variables
--------------

El rol acepta las siguientes variables:

- root_mariadb_password: Contraseña que se configurará por defecto en la BBDD

El rol acepta los siguientes tags:

- 00-deploy-httpd.yml 			tags=httpd
- 00-deploy-mariadb.yml 		tags=mariadb
- 00-deploy-php.yml 			tags=php
- 00-deploy-phpMyAdmin.yml 		tags=phpMyAdmin
- 00-deploy-HAproxy-keepalived.yml 	tags=ha

Dependencies
------------

N/A

Example Playbook
----------------

```yaml
- hosts: ansible
  user: root
  tasks:
     - name: Ensure that role are up to date
       command: ansible-galaxy install --force {{ item }}
       with_items:
          - miquelMariano.common
       when:
          - update_mode | default(False)
       tags: update
       ignore_errors: yes

- hosts: "{{ servers }}"
  user: root
  roles:
     - role: miquelMariano.lamp
```

Execute Playbook
----------------

- Despliegue de todo el stack lamp

```yaml
ansible-playbook playbooks/deploy_lamp.yml -i inventory/servers -e "servers=web update_mode=true"
```
- Despliegue de toto el stack con HAproxy y keepalived

```yaml
ansible-playbook playbooks/deploy_lamp.yml -i inventory/servers -e "servers=web ha_mode=true"
```

- Despliegue de una servicio concreto

```yaml
ansible-playbook playbooks/deploy_lamp.yml -i inventory/servers -e "servers=web" --tags=mariadb -v
``

License
-------

BSD

Author Information
------------------

[@miquelMariano](https://twitter.com/miquelMariano)
