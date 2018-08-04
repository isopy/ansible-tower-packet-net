# Clustered Ansible Tower on packet.net
This project was borne out of a need to quickly spin up Ansible Tower clusters for customer demos or to test the latest Tower version.

The `tower-packet-lab.yml` playbook will:
- Provision an Ansible Tower cluster on https://packet.net/ :
  - 3 clustered Tower instances
    - 2 instance groups
   - 1 isolated Tower instance
   - 1 Postgres instance
 - Install the version of Ansible Tower specified by the `tower_version` variable

## Packet.net setup
  Visit https://packet.net/:
  - Create an account and add a form of payment
  - Create a project
   - Add a personal or project api key


##  On your ansible control machine
#### Install packet-python
```pip install packet-python```

#### Update ansible.cfg to point to your vault password file*
```
[defaults]
inventory      = inventory
host_key_checking = False
vault_password_file = </path/to/your/vault/password/file>
retry_files_enabled = False
[persistent_connection]
command_timeout = 100
```
_*or remove the 'vault_password_file' line if you prefer to supply the password some other way_

#### Update roles/packet-provision/vars/vault.yml with your api key
```
---
# roles/packet-provision/vars/vault.yml
vaulted_api_token: <your_api_key>
```

#### Vault your api key
```ansible-vault encrypt roles/packet-provision/vars/vault.yml```

#### Update roles/packet-provision/defaults/main.yml with your project name
```
---
# roles/packet-provsion/defaults/main.yml
project_name: <your_project_name>
```

## Run it
#### Provision instances and install Ansible Tower version 3.2.5
```ansible-playbook tower-packet-lab.yml```


#### Install a version other than '3.2.5'
```ansible-playbook tower-packet-lab.yml --extra-vars tower_version='3.2.1'```

#### Remove your instances
```ansible-playbook tower-packet-lab.yml --extra-vars instance_state=absent```
