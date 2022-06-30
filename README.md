# Kubernetes Cluster template repository

Need a local Kubernetes cluster to quickly get to tinkering? Worry not, it is a mere `vagrant up` away.

This repository holds a cluster template to obtain a simple setup from scratch.

**You can enable asymmetric storage and pick from**:

- 1 master node, 3 worker nodes with disks **(default)**
- 1 master node, 3 worker nodes, 3 storage nodes with disks (enabled in Vagrantfile boolean value)

## Pre-requisites

The following need to be installed on the host machine:

- [Vagrant](https://www.vagrantup.com/docs/installation)
- [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/index.html)
- [VirtualBox](https://www.virtualbox.org/wiki/Downloads) as a virtualization provider

### Starting the cluster

Start cluster using virtualization provider (VirtualBox or QEMU) given in the `Vagrantfile` config:

```bash
vagrant up
```

To change configuration settings (i.e. number of worker nodes), edit the Vagrantfile as necessary. Options include:

- Using VirtualBox or QEMU for virtualization.
- Using Rook or MinIO as an S3-storage backend
- Memory for machines

## Cluster Features

- Calico as a CNI
- NGINX Ingress controller
- Minimal other installs to let you perform your own installs!

## Authors

| Name            | Email               |
| --------------- | ------------------- |
| BIGOT Luka      | luka@adaltas.com    |
| COTTART Kellian | kellian@adaltas.com |
