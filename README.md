# ANSIBLE-IAC
The aim of this project is to provision a secured docker swarm by using Ansible to provisioning.
 
## Local Swarm Testing

Local swarm cluster provisioning by using vagrant and ansible

### Pre-requisites

vagrant
ansible
virtualbox

### How to run 

```
ansible-galaxy install -r requirements.yml # Installs required roles
vagrant up --no-provision                            # Creates VMs
vagrant provision                                    # Provisions all dev-nodes when they are up
```
