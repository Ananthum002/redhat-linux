# 27 - Automation with Ansible

## Install & inventory
```bash
dnf install ansible-core
ansible --version
```
`inventory.ini`:
```ini
[web]
web1 ansible_host=10.0.0.11
web2 ansible_host=10.0.0.12
[db]
db1 ansible_host=10.0.0.21
[all:vars]
ansible_user=admin
```
`ansible.cfg`:
```ini
[defaults]
inventory = ./inventory.ini
host_key_checking = False
```

## Ad-hoc
```bash
ansible all -m ping
ansible web -m command -a "uptime"
ansible web -m dnf -a "name=httpd state=present" -b
ansible web -m service -a "name=httpd state=started enabled=yes" -b
ansible all -m copy -a "src=f dest=/tmp/f"
ansible all -m setup | less    # facts
ansible-inventory --list
```

## Playbook `site.yml`
```yaml
---
- name: Configure web servers
  hosts: web
  become: true
  tasks:
    - name: Install httpd
      ansible.builtin.dnf:
        name: httpd
        state: present
    - name: Start httpd
      ansible.builtin.service:
        name: httpd
        state: started
        enabled: true
    - name: Open firewall
      ansible.posix.firewalld:
        service: http
        permanent: true
        state: enabled
        immediate: true
```
```bash
ansible-playbook site.yml
ansible-playbook site.yml --check          # dry run
ansible-playbook site.yml -v --limit web1
ansible-playbook site.yml --syntax-check
ansible-playbook site.yml --ask-become-pass
```

## Vault & roles
```bash
ansible-vault create secrets.yml
ansible-vault edit secrets.yml
ansible-vault encrypt file ; ansible-vault decrypt file
ansible-playbook site.yml --ask-vault-pass
ansible-galaxy init myrole
ansible-galaxy collection install ansible.posix
```
