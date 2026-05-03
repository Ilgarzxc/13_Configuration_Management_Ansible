## Project URL
https://roadmap.sh/projects/configuration-management

ssh-keygen -t ed25519 -C "note"
ssh-copy-id -i {public key location} {login@server_ip} 

## Inventory
Inventory file intended to store ip addresses of the servers that we're going to manage with Ansible playbook.
*.ini or *.yaml format can be used or just a regular 'inventory' file.
To check if we can successfully reach out to all servers listed in the inventory file - we need to run the following command:
`ansible all --key-file {private key location} -i {inventory file location} -m ping`
or if you configured *.yaml file as I did in the repository:
`ansible all -i {inventory file location} -m ping`
if we dive deeper and set up ansible.cfg (local config for ansible connection) with a few variables - you can shorten it down to `ansible all -m ping`


## Ansible Playbook Configuration


### Useful commands
`ansible all --list-hosts`