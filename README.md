# Ansible Raspi Provisoner

Provisions a raspberry pi with docker and docker-compose.

Created to simplify home 64bit UnifiController raspi setup from a cold SD Card burn of the 64 bit images available on https://downloads.raspberrypi.org/raspios_arm64/images/.

---

## Important considerations / TODO's

There is still some static key imports within the `main.yml` file (look for `authorized_key` tasks) that I need to make variable and loop.
These will need changing for your own configuration, or removing if y

The variables within `inventory.yml` will need to be configured for your needs.

The static IP section at the bottom of the `main.yml` file needs to be configured for your network requirements.

---

## Pre-Requisites

There are some minimal requirements for this playbook to work, other than ansible obviously.

### SSHPass

Used for authenticating by password to hosts
On a mac: `brew install hudochenkov/sshpass/sshpass`

### Python passlib
Used for generation of secure passwords
Symptoms: `ERROR! crypt.crypt not supported on Mac OS X/Darwin, install passlib python module`
Cure:
Find the python used by your ansible instance (mine was in `~/.ansible.cfg`) then perform:
`python3 -m pip install passlib`

## Raspi Pre-Requisites

As the 64 bit image provides a desktop, and no SSH is configured prior.
Open a terminal and run the command: `sudo touch /boot/ssh` which will enable ssh then `sudo systemctl restart ssh` which will start the ssh service and allow login, and the playbooks to run.

---

## Running

- Configure `inventory.yml` as needed
- Configure `authorized_keys` sections within `main.yml` as needed
- Configure the static ip section within `main.yml` as needed
- Run `ansible-playbook -i inventory.yml main.yml`

Upon successful completion, change the `perform_initial_root_config` setting within `inventory.yml` to `no` to prevent attempting to login with `pi` after the username has been changed.

It is also expected that the 2nd play in the playbook will fail after first run as root login is disabled. To allow it to run again, you will need to enable root login within `/etc/ssh/sshd_config` and add a key to the root users `authorized_keys` file.

---

This was created for my own use, and I've probably missed _something_ reach out via an issue if you want to ask anything =)