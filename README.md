# Kubernetes Cluster template repository

Need a local Kubernetes cluster to quickly get to tinkering? Worry not, it is a mere `vagrant up` away.

This repository holds a cluster template to obtain a simple setup from scratch.

Pick from two structures:

- 1 master node, 3 worker nodes with disks **(default)**
- 1 master node, 3 worker nodes, 3 storage nodes with disks (enabled from the Vagrantfile)

## Pre-requisites

The following need to be installed on the host machine:

- [Vagrant](https://www.vagrantup.com/docs/installation)
- [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/index.html)
- [VirtualBox](https://www.virtualbox.org/wiki/Downloads) as a virtualization provider

### Starting the cluster

Start cluster with the current `Vagrantfile` config:

```bash
vagrant up
```

To change configuration settings (i.e. number of worker nodes), edit the Vagrantfile as necessary. Options include:

- Asymmetrical storage
- Memory used for VMs by category
- Disabling CNI or Ingress controller install

## Cluster Features

- Uses the private subnet `192.168.56.0/24` for nodes and `10.244.0.0/16` for pods
- Calico as a CNI
- NGINX Ingress controller
- Minimal other installs to let you perform your own installs!

## Authors

| Name            | Email               |
| --------------- | ------------------- |
| BIGOT Luka      | luka@adaltas.com    |
| COTTART Kellian | kellian@adaltas.com |
