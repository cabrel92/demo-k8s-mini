VAGRANT_EXPERIMENTAL="disks" # Enable VirtualBox disks addition

# ======== CONFIG OPTIONS FOR CLUSTER USERS ========

# Pick a virtualization provider: default is "virtualbox", also accepts "libvirt"
# Refer to "Using libvirt" in the README.md for further instructions
VIRTUALIZATION_PROVIDER = "libvirt"

# Resources for master node
MASTER_MEM = 2048
MASTER_CPU = 4

# Resources for each worker node
WORKERS_MEM = 3072
WORKERS_CPU = 4
WORKERS_DISKS = 2

# Enable or disable asymmetric storage
# false: 1 master node, 3 worker nodes (with disks)
# true: 1 master node, 3 worker nodes (diskless), 3 storage nodes (with disks)
ASYMMETRIC_STORAGE = false

# Resources for each storage node (only applied when ASYMMETRIC_STORAGE is true)
STORAGE_MEM = 3072
STORAGE_CPU = 4
STORAGE_DISKS = 2 # Nb of disks for each machine

# Path for resource files relative to the Vagrantfile (such as the kubeadm config)
RESOURCES_PATH = "resources/"

# ==================================================

VIRTUALBOX_IMAGE_NAME = "bento/ubuntu-20.04"
LIBVIRT_IMAGE_NAME = "generic/ubuntu2004"

# Conditionally set number of disks
workers_disks = 0
storage_disks = 0
if ASYMMETRIC_STORAGE == true
  storage_disks = STORAGE_DISKS
else
  workers_disks = WORKERS_DISKS
end
# Structure of dictionary content for each node: ip|cpus|memory|disks
# Base dictionary for cluster
cluster = {
  "k8s-master-01" => { :ip => "192.168.56.10", :cpus => MASTER_CPU, :mem => MASTER_MEM, :disks => 0 },
  "k8s-worker-01" => { :ip => "192.168.56.11", :cpus => WORKERS_CPU, :mem => WORKERS_MEM, :disks => workers_disks },
  "k8s-worker-02" => { :ip => "192.168.56.12", :cpus => WORKERS_CPU, :mem => WORKERS_MEM, :disks => workers_disks },
  "k8s-worker-03" => { :ip => "192.168.56.13", :cpus => WORKERS_CPU, :mem => WORKERS_MEM, :disks => workers_disks }
}
# Expanded dictionary for asymmetric storage
cluster_storage_nodes = {
  "k8s-storage-01" => { :ip => "192.168.56.14", :cpus => STORAGE_CPU, :mem => STORAGE_MEM, :disks => storage_disks },
  "k8s-storage-02" => { :ip => "192.168.56.15", :cpus => STORAGE_CPU, :mem => STORAGE_MEM, :disks => storage_disks },
  "k8s-storage-03" => { :ip => "192.168.56.16", :cpus => STORAGE_CPU, :mem => STORAGE_MEM, :disks => storage_disks }
}
# Merge both dictionaries when asymmetric storage is set to true
if (ASYMMETRIC_STORAGE == true)
  cluster.merge!(cluster_storage_nodes)
end

# Begin Vagrant config
Vagrant.configure("2") do |config|
  cluster.each_with_index do |(hostname, info), index|
    config.ssh.insert_key = false
    config.vm.define hostname do |node|
      node.vm.hostname = hostname
      case VIRTUALIZATION_PROVIDER
      when "virtualbox"
        node.vm.box = VIRTUALBOX_IMAGE_NAME
        node.vm.provider VIRTUALIZATION_PROVIDER do |vb|
          vb.customize ["modifyvm", :id, "--natdnshostresolver1", "off"]
          vb.customize ["modifyvm", :id, "--natdnsproxy1", "off"]
          vb.memory = info[:mem]
          vb.cpus = info[:cpus]
          # The following adds disks to the VMs as specified
          (0...info[:disks]).each do |i|
            vb.customize ["modifyvm", :id, "--ioapic", "on"]
            # Creating a virtual disk called "<VM-name>-disk-n" with a size of 30GB
            vb.customize ["createhd", "--filename", "#{hostname}-disks-#{i}", "--size", 30 * 1024]
            # Attaching the newly created virtual disk to our node
            vb.customize ["storageattach", :id, "--storagectl", "SATA Controller", "--port", 3 + i, "--device", 0, "--type", "hdd", "--medium", "#{hostname}-disk-#{i}.vdi"]
          end
          node.vm.network "private_network", ip: "#{info[:ip]}"
        end
      when "libvirt"
        node.vm.box = LIBVIRT_IMAGE_NAME
        node.vm.provider VIRTUALIZATION_PROVIDER do |libvirt|
          libvirt.graphics_type = "none"
          libvirt.memory = info[:mem]
          libvirt.cpus = info[:cpus]
          (0...info[:disks]).each do |i|
            libvirt.storage :file,
              :size => "20G",
              :bus => "scsi",
              :type => 'raw'
          end
        end
        node.vm.network :private_network,
          :ip => info[:ip],
          :libvirt__domain_name => "cluster.local"
      end
      # For parallel usage of Vagrant at the end of the loop
      if index == cluster.length() - 1
        node.vm.provision :ansible do |ansible|
          # Disable default limit to connect to all the machines
          ansible.limit = "all"
          ansible.playbook = "orchestration-playbook.yml"
          ansible.raw_arguments = ["--forks=#{cluster.length()}"]
          ansible.extra_vars = {
            resources_path: RESOURCES_PATH,
            asymmetric_storage: ASYMMETRIC_STORAGE,
          }
        end
      end
    end
  end
end
