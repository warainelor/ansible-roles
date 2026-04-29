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
ssh_keys_users:
  - name: user
    home_dir: /home/user
    group: user
    ssh_keys:
      - "{{ lookup('ansible.builtin.file', 'files/keys/foo.pub') }}"
      - "{{ lookup('ansible.builtin.file', 'files/keys/bar.pub') }}"
```

## Run

```bash
ansible-playbook -i inventory/hosts playbooks/deploy_ssh_keys.yml
```

```bash
ansible-playbook -i inventory/hosts playbooks/deploy_ssh_keys.yml --ssh-common-args='-o StrictHostKeyChecking=no'
```