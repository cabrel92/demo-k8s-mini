# Kubernetes Cluster template repository

Need a local Kubernetes cluster to quickly get to tinkering? Worry not, it is a mere `vagrant up` away.

This repository contains a cluster template to obtain a simple setup from scratch.

Two structure options:

- 1 master node, 3 worker nodes with disks **(default)**
- 1 master node, 3 worker nodes, 3 storage nodes with disks (enabled from the Vagrantfile)

## Pre-requisites

The following need to be installed on the host machine:

- [Vagrant](https://www.vagrantup.com/docs/installation)
- [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/index.html)
- [VirtualBox](https://www.virtualbox.org/wiki/Downloads) as a default virtualization provider (libvirt is an [alternative](#using-libvirt))

### Starting the cluster

Start the cluster with the current `Vagrantfile` config:

```bash
vagrant up
```
 I got this Error while connecting to libvirt error but it was simply a case that I was running vagrant up without specifying the provider
```bash
vagrant up --provider virtualbox
```
Access the cluster's master node:

```bash
vagrant ssh demo-master-01
```

To change configuration settings (i.e. number of worker nodes), edit the Vagrantfile as necessary. Options include:

- Asymmetrical storage
- Memory used for VMs by category
- Disabling CNI install
- [Changing the virtualization provider](#using-libvirt)

Turn off the cluster and clean up its resources:

```bash
vagrant destroy -f
```

## Cluster Features

- Uses the private subnet `192.168.56.0/24` for nodes and `10.244.0.0/16` for pods
- Each node equipped with 2 disks by default - either on workers or dedicated nodes
- Calico as a CNI
- Minimal other installs to leave room for your own!

## Using libvirt

`libvirt` can be used as an alternate virtualization provider.

See [Installing libvirt](./docs/virt-providers.md/#installing-libvirt).

## Authors

| Name            | Email               |
| --------------- | ------------------- |
| BIGOT Luka      | luka@adaltas.com    |
| COTTART Kellian | kellian@adaltas.com |
