# SSH Keys Deployment Role

Ansible role to deploy SSH public keys to remote servers

## Usage

Add keys to files/keys/*.pub

Define credentials in inventory/hosts:
```ini
[servers]
server1 ansible_host=192.168.1.10 ansible_user=ubuntu ansible_password=secret123
```

Add vars to vars/ssh_keys.yml:
```yaml
---
# Настройки пользователя
new_user: "deploy"
user_group: "sudo"
user_shell: "/bin/bash"

# Порт SSH
ssh_port: 2220

# Программы для установки
additional_packages:
  - git
  - ufw
  - nmap
  - net-tools
  - curl
  - fail2ban

# Явный список SSH‑ключей для пользователей
# Пути относительные к корню проекта
users_ssh_keys:
  - username: "{{ new_user }}"
    keys:
      - "files/keys/key1.pub"
      - "files/keys/key2.pub"
  - username: "user2"
    keys:
      - "files/keys/user2.pub"
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