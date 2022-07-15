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

## Commands

  ```
  sudo kubeadm init --pod-network-cidr 192.168.1.0/16 --node-name master --apiserver-advertise-address 192.168.0.17
  curl https://projectcalico.docs.tigera.io/manifests/calico.yaml -O
  kubectl apply -f calico.yaml
  ```

## References

- [Install kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)
- [ansible-kubeadm](https://github.com/gaurav-gupta-gtm/ansible-kubeadm): borrowed several implementations
- [Rocky Linux](https://rockylinux.org/)
- [Virtualbox](https://www.virtualbox.org/)
- [Vagrant](https://www.vagrantup.com/)
- [Ansible](https://www.ansible.com/)
- [Issue when using kubeadm with multiple network interfaces](https://github.com/kubernetes/kubernetes/issues/33618)
