# Ansible Tasks to Bootstrap Homelab Services

## Table of Contents

1. [Setup PiHole Nodes](#setup-pihole-nodes)
2. [Setup Pi Iris](#setup-pi-iris)
3. [Setup Synology](#setup-synology-nodes)
4. [Setup Tools](#setup-tools)

## Setup PiHole nodes

Pihole nodes will be setup with the following features.

- High Availability
- Allows access from Home Assistant for power management tasks
- `orbital-sync` inside pi-iris handles synchronization of `gravity` database

> if needed specify become user password using -K


1. Setup Admin User

   ```sh
    ansible-playbook setup-users-admin.yaml -l <nodes> --user=root -k
   ```

2. Setup DevOps Users

   ```sh
    ansible-playbook setup-users-devops.yaml -l <nodes> --user=<admin-user> -K
   ```

3. Setup pihole

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

## Setup Synology Nodes

1. Setup sudo access

   > Root user is not allowed to login. As such the admin user is a separate user ie. asiri
   > To keep it consistent across devices a ``serveradmin`` user is created. 
   > In other devices ``serveradmin`` is used to create the devops users. But since the the user and group management scripts are inside the main admin users home directory this is will introduce complexity to the ansible scripts. 
   > As such, devops users are also created using the main admin user ie. asiri

   - Create ``serveradmin`` User

      ```sh
      ansible-playbook setup-admin-user.yaml \
         -l nas-01-synology \
         --user=asiri \
         --ssh-common-args="-o PubkeyAuthentication=no -o PasswordAuthentication=yes -o PreferredAuthentications=password" \
         -k
      ```

   - Create DevOps Users

      ```sh
      ansible-playbook setup-devops-users.yaml \
         -l nas-01-synology \
         --user=asiri \
         --ssh-common-args="-o PubkeyAuthentication=no -o PasswordAuthentication=yes -o PreferredAuthentications=password" \
         -k
      ```


2. Bootstrap

## Setup Tools

   ```sh
   ansible-playbook setup-archivebox.yaml  --user=serveradmin -K
   ansible-playbook setup-snipeit.yaml  --user=serveradmin -K
   ```