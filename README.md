# Homelab

My attempt to create a homelab. This repo will contain various configuration files for my homelab.


## Ansible 

First install the ansible in the host machine only. 

Then create a ssh key on the host machine and copy it to the remote machines / nodes / worker nodes. 

Remember to add ssh key in the `~/.ssh/authorized_keys` and give it proper permissions. 

In order to use `pacman` for arch linux, install `ansible-galaxy collection install community.general`

Then run the playbook: `ansible-playbook -i inventory/inventory.ini playbooks/update_system.yaml --ask-become-pass` 

Here:

    - `-i inventory/inventory.ini` will specify which inventory to use
    - `--ask-become-pass` will ask for the root password for the remote machines


