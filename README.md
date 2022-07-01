# Kubernetes Cluster template repository

Need a local Kubernetes cluster to quickly get to tinkering? Worry not, it is a mere `vagrant up` (and a couple of minutes) away.

This repository holds a cluster template to obtain a simple setup from scratch.

Pick from two structures:

- 1 master node, 3 worker nodes with disks **(default)**
- 1 master node, 3 worker nodes, 3 storage nodes with disks (enabled from the Vagrantfile)

## Pre-requisites

The following need to be installed on the host machine:

- [Vagrant](https://www.vagrantup.com/docs/installation)
- [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/index.html)
- [VirtualBox](https://www.virtualbox.org/wiki/Downloads) as a default virtualization provider (libvirt is an [alternative](#using-libvirt))

### Starting the cluster

Start cluster with the current `Vagrantfile` config:

```bash
vagrant up
```

To change configuration settings (i.e. number of worker nodes), edit the Vagrantfile as necessary. Options include:

- Asymmetrical storage
- Memory used for VMs by category
- Disabling CNI install
- [Changing the virtualization provider](#using-libvirt)

## Cluster Features

- Uses the private subnet `192.168.56.0/24` for nodes and `10.244.0.0/16` for pods
- Nodes equipped with 2 disks each by default - either on workers or dedicated nodes
- Calico as a CNI
- Minimal other installs to leave room for your own!

## Using libvirt

See [Installing libvirt](./docs/virt-providers.md/#installing-libvirt).

## Authors

| Name            | Email               |
| --------------- | ------------------- |
| BIGOT Luka      | luka@adaltas.com    |
| COTTART Kellian | kellian@adaltas.com |
