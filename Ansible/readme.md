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
    example-hostname2:
      ansible_host: X.X.X.X
```
#### Example playbook
```
vi /etc/ansible/playbook.yaml
```
```
---
- name: Zabbix agent2 update
  hosts: flexstor
  remote_user: root
  debugger: on_failed
  tasks:
  - name: Ensure zabbix-agent2 is at the latest version
    ansible.builtin.yum:
      name: zabbix-agent2
      state: latest
    notify:
    - Restart zabbix-agent2
  - name: Ensure that zabbix-agent2 is started
    ansible.builtin.service:
      name: zabbix-agent2
      state: started
  handlers:
    - name: Restart zabbix-agent2
      ansible.builtin.service:
        name: zabbix-agent2
        state: restarted
```

#### Root privileges example
```
- name: Ensure the httpd service is running
  service:
    name: httpd
    state: started
  become: yes
```
#### Disable SSH key host checking
```
vi /etc/ansible/ansible.cfg
```
```
[defaults]
host_key_checking = False
```

#### Run playbook
```
ansible-playbook playbook_demo.yaml
```

#### Run example command
```
ansible -m ping all --ask-pass
```
