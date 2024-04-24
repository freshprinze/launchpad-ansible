# Ansible Tasks to Bootstrap Homelab Services

## Table of Contents

1. [Setup Users](#setup-users)
2. [Setup PiHole Nodes](#setup-pihole-nodes)
3. [Setup Pi Iris](#setup-pi-iris)

## Setup Users

1. Setup Admin User

   ```sh
    ansible-playbook setup-admin-user.yaml -l <nodes> --user=root -k
   ```

2. Setup DevOps Users

   ```sh
    ansible-playbook setup-devops-users.yaml -l <nodes> --user=<admin-user> -K
   ```

## Setup PiHole nodes

Pihole nodes will be setup with the following features.

- High Availability
- Allows access from Home Assistant for power management tasks
- `orbital-sync` inside pi-iris handles synchronization of `gravity` database

> if needed specify become user password using -K


1. Setup pihole

   ```sh
    ansible-playbook setup-pi-hole.yaml --user=<admin-user>
   ```

## Setup Pi Iris

Ansible script to setup pi iris

> if needed specify become user password using -K

1. Setup sudo access

   ```sh
     ansible-playbook setup-sudo.yaml -l pi_iris --user=root -k
   ```

2. Bootstrap

   ```sh
     ansible-playbook setup-pi-iris.yaml --tags=bootstrap --user=serveradmin
   ```

3. Setup Docker and Copy Modules

   ```sh
     ansible-playbook setup-pi-iris.yaml --tags=docker,mods --user=serveradmin
   ```
