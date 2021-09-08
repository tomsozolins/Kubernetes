## Manual Ansible installation on kubernetes
```
# kubectl apply -f ansible.yaml
# kubectl exec --stdin --tty deployment/ansible -- /bin/bash
```

##### Installation
https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-ansible-on-ubuntu
```
apt update
apt install software-properties-common
add-apt-repository --yes --update ppa:ansible/ansible
apt install ansible
cd /etc/ansible/
```

#### Build inventory
```
vi /etc/ansible/hosts
```
```
example-group:
  hosts:
    example-hostname:
      ansible_host: X.X.X.X
```
#### Example playbook
```
vi /etc/ansible/playbook_demo.yaml
```
```
---
- hosts: all
  become: yes
  tasks:
  - name: "update hostnames"
    hostname:
      name: "{{ new_hostname }}"

```

#### Run playbook
```
ansible-playbook playbook_demo.yaml
```
