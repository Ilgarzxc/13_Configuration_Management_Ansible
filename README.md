# Ansible Configuration Management Project

This repository contains an Ansible project for basic server setup, nginx installation, SSH key deployment, and static website deployment. The project includes a `setup.yml` playbook and several roles to keep configuration modular and reusable.

## Project URL
https://roadmap.sh/projects/configuration-management

## Project structure

- `setup.yml` — main playbook that runs roles in sequence
- `ansible.cfg` — local Ansible configuration
- `inventory.ini` — target server inventory
- `base/` — basic server setup role
- `nginx/` — nginx installation and configuration role
- `app/` — static site deployment role
- `ssh/` — public key deployment role
- `site_from_git/` — optional GitHub deployment role

## Requirements

- Ansible installed
- A Linux server to configure (Ubuntu/Debian-compatible environment)
- SSH access to the server
- A public key file in `ssh/files/` for the `ssh` role
- A static site archive in `app/files/static_site.tar.gz` for the `app` role

## Configuration

The project uses `ansible.cfg` to define the default inventory and SSH key:

```ini
[defaults]
inventory = inventory.ini
private_key_file = ~/.ssh/ansible
```

Update `private_key_file` if you use a different key, or override inventory on the command line.

The inventory is defined in `inventory.ini`:

```ini
all:
  hosts:
    79.76.50.85:
      ansible_host: 79.76.50.85
      ansible_user: ubuntu
      ansible_group: ubuntu
```

## Usage

Run all roles:

```bash
ansible-playbook setup.yml
```

Run a specific role by tag:

```bash
ansible-playbook setup.yml --tags "app"
ansible-playbook setup.yml --tags "nginx"
ansible-playbook setup.yml --tags "ssh"
```

## Roles

### `base`

Basic server setup for Debian/Ubuntu systems:

- updates package cache
- upgrades packages
- installs utilities: `git`, `ufw`, `vim`, `fail2ban`
- configures UFW default policies
- allows SSH, HTTP, HTTPS
- enables UFW
- starts `fail2ban`

### `nginx`

Installs and configures nginx:

- installs `nginx`
- enables and starts nginx service
- deploys site configuration from `nginx/templates/nginx.conf.j2`
- enables the custom site and removes default nginx site

### `app`

Deploys a static website archive:

- copies `app/files/static_site.tar.gz` to `/tmp`
- recreates `/var/www/app`
- extracts archive into `/var/www/app`
- notifies nginx to reload

### `ssh`

Deploys public SSH keys:

- ensures the target user's `authorized_keys` exists
- adds keys from `ssh/files/ansible.pub` and `ssh/files/id_ed25519.pub`
- ensures SSH port is allowed in UFW

### `site_from_git`

Optional stretch-goal role:

- clones a GitHub repository into `/tmp/checkout`
- copies the site content to `/var/www/app`
- sets permissions for `www-data`
- restarts nginx

## Notes

- The project is currently designed for Debian/Ubuntu systems.
- `site_from_git` is an extra role that demonstrates deploy-from-GitHub behavior.
- If you rename `inventory.ini`, update `ansible.cfg` or run playbook with `-i` explicitly.
