
# ANSIBLE-IAC
The aim of this project is to provision a **Docker Swarm** by using **Ansible**.
The **Docker Engine** is accessible from remote on a secure TLS channel.
 
## Local Swarm Testing

Local swarm cluster provisioning can be tested by creating VMs with the provided Vagrant file

### Pre-requisites

Vagrant 2.2.x
Vagrant plugin vagrant-hostsupdater #it creates host entries in your /etc/hosts
Ansible 2.9.x
Virtualbox 6.x
Docker client 19.0x

### RUN Vagrant VMs ###

Provisioned Vagrant nodes are named swarm-manager and swarm-worker. The Vagrant file provisions all the hosts with default insecure Vagrant SSH private key but you can override it with SSH_KEY environment parameter. Se example below.
```
SSH_KEY=<PATH_TO_SSH_PRIVATE_KEY> vagrant up # Assuming the public key path is <PATH_TO_SSH_PRIVATE_KEY>.pub
```
The vagrant-hostsupdater plugin will update your /etc/hosts so you can ping them
You can SSH into the VMs with:
```
vagrant ssh <NODE_NAME>
ssh -i ~/.vagrant.d/insecure_private_key vagrant@<NODE_NAME> #if you did not specify a custom SSH private key
ssh -i <PATH_TO_SSH_PRIVATE_KEY> vagrant@<NODE_NAME>         #if you specified a custom SSH private key
```

## RUN Ansible on provisioned VMs


```
ansible-playbook -r requirements.yml      # Ansible required modules download
ansible-playbook -i inventory/hosts.ini site.yml -e "ansible_ssh_private_key_file=<PATH_TO_YOUR_SSH_PRIVATE_KEY>"
```

## Accessing Docker Engine from Remote

The Ansible Playbook creates automatically TLS certs for mutual authentication. Client certs are created on remote nodes and copied locally in ~/.docker/<NODE_NAME>/.

```
source ~/.docker/<DOCKER_NODE>/docker_env.sh # it configures DOCKER_ environment varibales used by docker client
# You can now send commands to the remote Docker Engine
docker info
docker service create -p 80:80 --replicas=5 --name hello-world tutum/hello-world
curl http://swarm-manager 
curl http://swarm-worker
```
