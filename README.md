# Ansible Playbook Template

## Requirements

* WSL or linux
* ansible 2.8 or newer

## Usage

### Prepare your environment

* start ssh-agent (`eval ssh-agent`)
* add your ssh key to agent (i.e. `ssh-add ~/.ssh/id_rsa`)
* `export ANSIBLE_CONFIG=./ansible.cfg`

### Run the desired Playbook

* `ansible-playbook -i inventory/[ENVIRONMENT]/hosts playbooks/[playbook]`

## Playbook Layer
* 0.x - probing Playbooks. They don't change anything like 'ping'
* 1.x - basic Provisioning (hardening, Compliance Playbooks, access/sshkey-management)
* 2.x - Prepare System - choose what applies on your system
* 3.x - Install Applications (mostly restart application or take way longer than corresponding 4.x books)
* 4.x - Update/Upgrade Applications
* 5.x - starting stopping or restarting applications - basic operation makes use of interactive parameters
* 6.x - master playbook for deploying a whole applicatiomn stack which includes other playbooks - not recommended

## Detailed Playbook Descriptions
##### Hint: You find detailed role descriptions in the roles/\<role\>/README.md files

`ansible-playbook -i inventory/[production|staging|test|dev]/hosts playbooks/[playbook] --extra-vars '{"var1":"value1","var2":["listitem1","listitem2"]`

[See also User guide](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html)

Usually you don't need extra-vars. But under certain circumstances you might manually check something up and need it.
In this Readme you find a little description what it is for.

| Playbook  | Description | extra-vars |
| --------- | ----------- | ---------- |
| 0.0-ping-all.yml | Pings all Hosts defined in hosts-file. Fails if one host is not reachable. Purpose is to check hosts file conformity | `none` |
| 1.0-harden-ssh.yml | Runs dev-sec os- and ssh-hardening. Then rolls out team's ssh keys. Should be regularily updated from upstream and should be run regularily on every docs host. | `{"additional_ssh_users":["user1","user2"]}'` |
| 1.1-ssh.yml | only rolls out team's ssh keys. Purpose to give a quick access for a guest. |  `{"additional_ssh_users":["user1","user2"]}'` <br> TODO: User Key must be in files/\<user\>.pub |
| 2.0-prepare-docker-host.yml | Contains all roles and tasks to install and configure DockerD on a host | `none` |

   