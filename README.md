# CKx Training Box

This is the training lab I used during preparations for the CKA, CKAD, and CKS exams.


## System requirements

- [vagrant](https://www.vagrantup.com/docs/installation)
- [virtualbox](https://www.virtualbox.org/wiki/Downloads)
- [virtualbox extension pack](https://linuxconfig.org/virtualbox-extension-pack-installation-on-ubuntu-20-04-focal-fossa-linux)

> NOTE: VirtualBox might require the Extension Pack to properly mount the required "shared folder" under the /vagrant directory in your system.


## Getting started

Once the system requirements are installed properly, you can bootstrap the whole virtual environment by executing the `vagrant up` command from the root directory of this folder.


1) Check current status of virtualized environment
```
cks-student@laptop:~# vagrant status

Current machine states:

cks-master                not created (virtualbox)
cks-worker-1              not created (virtualbox)

This environment represents multiple VMs. The VMs are all listed above with their current state. For more information about a specific VM, run `vagrant status NAME`.
```

2) Bootstrap training environment
```
cks-student@laptop:~# vagrant up

Bringing machine 'cks-master' up with 'virtualbox' provider...
Bringing machine 'cks-worker-1' up with 'virtualbox' provider...
==> cks-master: Importing base box 'ubuntu/bionic64'...
==> cks-master: Matching MAC address for NAT networking...
==> cks-master: Setting the name of the VM: cks-training-lab_cks-master_1625736994543_48271
==> cks-master: Clearing any previously set network interfaces...
==> cks-master: Preparing network interfaces based on configuration...
    cks-master: Adapter 1: nat
    cks-master: Adapter 2: hostonly
==> cks-master: Forwarding ports...
    cks-master: 22 (guest) => 2222 (host) (adapter 1)
==> cks-master: Running 'pre-boot' VM customizations...
==> cks-master: Resized disk: old 40960 MB, req 51200 MB, new 51200 MB
==> cks-master: You may need to resize the filesystem from within the guest.
==> cks-master: Booting VM...
(truncated output)
```

3) Suspend training machines
```
cks-student@laptop:~# vagrant suspend

==> cks-master: Saving VM state and suspending execution...
==> cks-worker-1: Saving VM state and suspending execution...
```

4) Destroy virtualized environment
```
cks-student@laptop:~# vagrant destroy -f

==> cks-master: Forcing shutdown of VM...
==> cks-master: Destroying VM and associated drives...
==> cks-worker-1: Forcing shutdown of VM...
==> cks-worker-1: Destroying VM and associated drives...
```

> NOTE: The environment is prepared to bootstrap a full Kubernetes cluster from the scratch, if you want to provision only certain steps (e.g., CNI, dashboard, ingress controller, OPA, etc.), you may modify any of the Ansible playbooks on the [kubernetes-setup](kubernetes-setup) folder.
