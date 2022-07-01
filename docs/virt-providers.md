# Available virtualization providers for the cluster

The VMs part of the cluster are brought up thanks to virtualization providers. Vagrant's default provider is [VirtualBox](https://www.virtualbox.org/). However, other virtualization providers offer different benefits, such as better startup time or better performance.

While a lot of different providers exist, as of now, only **VirtualBox** and **libvirt** with KVM/QEMU are supported out of the box in this Vagrantfile: libvirt merely needs [additional installs on the host machine, described just below](#installing-libvirt). To switch between the two, simply modify the following line in `Vagrantfile`:

```ruby
VIRTUALIZATION_PROVIDER = # virtualbox|libvirt
```

VirtualBox also provides the benefit of being easy to install: in most cases, only virtualization needs to be enabled in the BIOS in addition to the [VirtualBox install](https://www.virtualbox.org/wiki/Downloads).

## Installing libvirt

The VMs are provided by VirtualBox by default. However, as there are incompatibilities across different virtualization providers, having the option to switch to using **libvirt and KVM/QEMU** is possible.

Install the following Vagrant plugins:

```bash
vagrant plugin install vagrant-libvirt
```

Then, set the `VIRTUALIZATION_PROVIDER` config value in the [Vagrantfile](./Vagrantfile) to `libvirt`.

> The Vagrant box used for the VMs is different for both providers. All other configuration is handled by conditions in the Vagrantfile, you do not have to worry about anything other than choosing the provider value.

## Adding additional virtualization providers

If you wish to implement other virtualization providers, only the Vagrantfile needs to be edited. Provisioning is supposedly the same across all providers.

Additional providers need to implement dynamic VM disks creation, as this differs with every provider.

> Not all Vagrant boxes are compatible with all providers. The generic box used with libvirt is compatible with a number of providers, as indicated [here](https://app.vagrantup.com/generic/boxes/ubuntu2004)
