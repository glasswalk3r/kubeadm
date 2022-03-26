# kubeadm

kubeadm based K8s cluster provisioned with Vagrant and Ansible to implement
a studying environment.

## Features

- Manages two VM's (master and slave) based on Rocky Linux and VirtualBox.
- VM's already include the VirtualBox Extension Pack.
- kubeadm is configured with Ansible.

## Requirements

- Vagrant version 2.2.19 or higher
- Ansible version 2.12.1 or higher
- VirtualBox 6.1 or higher

## References

- [Install kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)
- [ansible-kubeadm](https://github.com/gaurav-gupta-gtm/ansible-kubeadm): borrowed several implementations
- [Rocky Linux](https://rockylinux.org/)
- [Virtualbox](https://www.virtualbox.org/)
- [Vagrant](https://www.vagrantup.com/)
- [Ansible](https://www.ansible.com/)
