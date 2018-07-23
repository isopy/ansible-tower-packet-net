# Clustered Ansible Tower on packet.net
This project was borne out of a need to quickly spin up Ansible Tower clusters for customer demos or to test the latest Tower version.

The `tower-packet-lab.yml` playbook will:
- Provision an Ansible Tower cluster on https://packet.net/ which consists of:
	 - 3 Tower instances
	 - 1 Postgres instance
 - Install the latest version of Ansible Tower

## packet.net setup
  Visit https://packet.net/:
  - Create an account and add a form of payment
  - Create a project
   - Add a personal or project api key


##  on your ansible control machine
**Install packet-python**
```pip install packet-python```

**Update ansible.cfg to point to your vault password file**
*[ or remove the 'vault_password_file' line if you prefer to supply the password some other way ]*
```
[defaults]
inventory      = inventory
host_key_checking = False
vault_password_file = </path/to/your/vault/password/file>
retry_files_enabled = False
[persistent_connection]
command_timeout = 100
```
**Update roles/packet-net-provision/vars/vault.yml with your api key**
```
---
# roles/packet-net-provision/vars/vault.yml
vaulted_api_token: <your_api_key>
```

**Vault your api key**
```ansible-vault encrypt roles/packet-net-provision/vars/vault.yml```

**Update roles/packet-net-provsion/defaults/main.yml with your project name**
```
---
# roles/packet-net-provsion/defaults/main.yml
project_name: <your_project_name>
```

## run it
**Provision instances and install Ansible Tower version 3.2.5**
```ansible-playbook tower-packet-lab.yml```


**If you prefer a version other than '3.2.5', use `--extra-vars` to set a specific `tower_version`**
```ansible-playbook tower-packet-lab.yml --extra-vars tower_version='3.2.1'```

**Remove your instances**
```ansible-playbook tower-packet-lab.yml --extra-vars instance_state=absent```
