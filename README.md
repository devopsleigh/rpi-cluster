# Raspberry Pi Kubernetes Cluster

Configures a Raspberry Pi Kubernetes (K3S) cluster using Ansible.

## Prerequisites

- A Linux host to use as an Ansible server, separate to the cluster
  - This example uses Ubuntu - this is important because installing Ansible differs by distro
  - Tested using Ubuntu 18.04 on WSL
- Basic configuration of the RPi boards
  - Flash Raspbian Lite
  - Add `ssh` to the root
  - Add a configured `wpa_supplicant.conf` to the root
  - Set a static IP for each node

## Installation

All tasks are to be performed on the Ansible server host.

1. Download the repository:

   ```sh
   git clone https://github.com/devopsleigh/k3s-lab.git
   cd k3s-lab
   ```

2. Edit the Ansible inventory:

   ```sh
   nano hosts.ini
   ```

   ```ini
    [master]
    k3s-master ansible_ssh_host=10.0.0.10

    [node]
    k3s-node1 ansible_ssh_host=10.0.0.11
    k3s-node2 ansible_ssh_host=10.0.0.12
    k3s-node3 ansible_ssh_host=10.0.0.13

    [cluster:children]
    master
    node

    [cluster:vars]
    ansible_user=ansible
   ```

3. Edit the secrets file:

   ```sh
   nano secrets.yml
   ```

   ```yml
   ---
   TZ=Country/City
   ```

4. Run the configuration script:

   ```sh
   sudo bash config.sh
   ```

## Uninstall K3S

```sh
ansible cluster -a "/usr/local/bin/k3s-uninstall.sh" -b
```
