# vagrant.geotrellis
A vagrant environment for doing GeoTrellis development.

Vagrant and ansible must be installed.
=======
# Overview

This repository can be used to set up a virtual machine environment to develop on GeoTrellis using Vagrant. The virtual machine will include [GeoTrellis](http://geotrellis.io/), [Spark](http://spark.apache.org/), [HDFS](http://hadoop.apache.org/docs/r1.2.1/hdfs_design.html), [ZooKeeper](http://zookeeper.apache.org/), and [Accumulo](https://accumulo.apache.org/).

## Installation Requirements

In order to get started with this virtual machine, some software must be installed on the host machine and the host machine must support virtualization.

### Software

#### Vagrant
[Vagrant](https://www.vagrantup.com/) (version >= 1.7.2) is required on the host to manage the virtual machine. [Binaries](https://www.vagrantup.com/downloads.html) are available for most operating systems.

#### Ansible
[Ansible](http://www.ansible.com/home) (version >= 1.8.2) is required to handle configuration of the virtual machine. Ansible is officially supported for Mac OSX and Linux host environments, though it can be used with a [Windows host machine](http://www.azavea.com/blogs/labs/2014/10/running-vagrant-with-ansible-provisioning-on-windows/). There are multiple ways to [install Ansible](http://docs.ansible.com/intro_installation.html), choose the most appropriate one for your operating system.

#### VirtualBox
[VirtualBox](https://www.virtualbox.org/) is an open source virtual software package used to handle virtual machines. There are binaries [available](https://www.virtualbox.org/wiki/Downloads) for most operating systems.

Note: It is also possible to run the virtual machine using a [Kernel Based Virtual Machine](http://www.linux-kvm.org/page/Main_Page) on Linux. This can be done using the [Vagrant Libvirt Provider](https://github.com/pradels/vagrant-libvirt). Vagrant Libvirt is still under active development and there are additional requirements if KVM is used.

#### Git
[Git](http://git-scm.com/) is used for version control. It is necessary to use Git to download the GeoTrellis code and submit patches for development.

### Sytem Requirements
Your host machine should have at least 6GB of memory, a modern x86-64 processor, and virtualization support must be [enabled for the processor being used](http://virt-tools.org/learning/check-hardware-virt/).

## Getting Started

1. [Clone](http://git-scm.com/docs/git-clone) this repository.

   `git clone https://github.com/geotrellis/vagrant.geotrellis.git`

   Note: If you wish to submit patches to this repository, you should consider [forking this repository](https://help.github.com/articles/fork-a-repo/).

2. Install required software listed above

3. Fork [GeoTrellis](https://github.com/geotrellis/geotrellis)

4. Navigate into the directory created by cloning `vagrant.geotrellis`

5. Clone GeoTrellis from your forked repository.

   At this point, the directories in the `vagrant.geotrellis` directory should look like this.
    ```
    vagrant.geotrellis
    ├── ansible
    ├── geotrellis
    ├── README.md
    └── Vagrantfile
	```

6. Determine the appropriate [folder syncing](http://docs.vagrantup.com/v2/synced-folders/index.html) option by [setting](https://www.digitalocean.com/community/tutorials/how-to-read-and-set-environmental-and-shell-variables-on-a-linux-vps) the `VAGRANT_GEOTRELLIS_SYNC` environment variable
   - For Linux and Mac OSX, NSF is likely the best option
   - For Windows, consider using [rsync](http://docs.vagrantup.com/v2/synced-folders/rsync.html)

   Rsync requires an extra process to be run to sync folders when developing, but has huge performance benefits compared to other options. This will greatly speed up compiling and running GeoTrellis since build products will not need to be synced back and forth between your guest and host machine.

| Value   | Sync Folder Type  |
|---------|-------------------|
| nfs     | NFS               |
| rsync   | RSYNC             |
| <none>  | OS Default        |

7. In the top level directory with the `Vagrantfile` bring up the virtual machine at the command line.

   `vagrant up`

   At this point Vagrant will start the virtual machine and begin provisioning it with Ansible. Depending on internet connection speeds, installation and downloading of all dependencies could take some time.

8. Once the machine finishes provisioning, you can verify that Accumulo and HDFS are running by navigating to their web UIs.
   - Accumulo: http://localhost:50095
   - HDFS: http://localhost:50070

9. If using the RSync shared folder option, start the vagrant [rsync process](http://docs.vagrantup.com/v2/cli/rsync-auto.html) to ensure your changes in the GeoTrellis code get synced to the virtual machine
   `vagrant rsync-auto`

10. Once finished, you can ssh into the machine, navigate to the GeoTrellis directory, and start hacking on GeoTrellis.

   ```
   vagrant ssh
   cd /home/vagrant/geotrellis/
   ```
