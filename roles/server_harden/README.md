# SSH Keys Deployment Role

Ansible role to deploy SSH public keys to remote servers

## Usage

1. Add public keys to files/keys/*.pub

2. Define credentials in inventory/hosts:
```ini
[servers]
server1 ansible_host=192.168.1.10 ansible_user=ubuntu ansible_password=secret123
```

3. Add vars to vars/harden_server.yml:
```yaml
---
# Настройки пользователя
new_user: "user"

# Порт SSH
ssh_port: 2222

# Явный список SSH‑ключей для пользователей
ssh_keys:
  - "{{ lookup('ansible.builtin.file', '../files/keys/key1.pub') }}"
  - "{{ lookup('ansible.builtin.file', '../files/keys/key2.pub') }}"
```

## Run

```bash
ansible-playbook --check -i inventory/hosts playbooks/harden_server.yml --extra-vars "@vars/harden_server.yml"
```

```bash
ansible-playbook -i inventory/hosts playbooks/harden_server.yml --extra-vars "@vars/harden_server.yml"
```

```bash
ansible-playbook -i inventory/hosts -l your_host playbooks/harden_server.yml --extra-vars "@vars/harden_server.yml"
```

```bash
ansible-playbook -i inventory/hosts playbooks/harden_server.yml --ssh-common-args='-o StrictHostKeyChecking=no' --extra-vars "@vars/harden_server.yml"
```