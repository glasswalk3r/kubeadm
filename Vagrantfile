# frozen_string_literal: true

def configure_virtualbox(vbox)
  # Display the VirtualBox GUI when booting the machine
  # Useful for troubleshooting when true
  vbox.gui = false
  vbox.memory = 2057 # 2G - 9MB for video RAM
  vbox.cpus = 2
  vbox.customize ['modifyvm', :id, '--vrde', 'off']
  vbox.customize ['modifyvm', :id, '--vram', 9]
  vbox.customize ['modifyvm', :id, '--graphicscontroller', 'vmsvga']
  vbox.check_guest_additions = false
  vbox.linked_clone = true
end

def configure_os(config, master_ip, slave_ip)
  # faster than using Ansible
  config.vm.provision 'shell', inline: 'dnf makecache && dnf upgrade -y'
  sysctl_cfg = '70.ipv6.conf'

  script = <<-SCRIPT
  echo 'master #{master_ip}' >> /etc/hosts
  echo 'slave #{slave_ip}' >> /etc/hosts
  echo 'net.bridge.bridge-nf-call-iptables = 1' > /etc/sysctl.d/k8s.conf
  echo 'net.ipv4.ip_forward = 1' >> /etc/sysctl.d/k8s.conf
  echo br_netfilter > /etc/modules-load.d/k8s.conf
  echo overlay >> /etc/modules-load.d/k8s.conf
  mv -f /tmp/#{sysctl_cfg} /etc/sysctl.d
  sysctl --system
  SCRIPT

  config.vm.provision 'file', source: "config/#{sysctl_cfg}", destination: '/tmp/'
  config.vm.provision 'shell', inline: script
end

MASTER_IP = '192.168.0.17'
SLAVE_IP = '192.168.0.18'

Vagrant.require_version '>= 1.8'

Vagrant.configure('2') do |config|
  config.vm.box = 'boxomatic/rocky-8'
  config.vm.box_check_update = true
  config.vbguest.auto_update = true

  config.vm.provider 'virtualbox' do |vb|
    configure_virtualbox(vb)
  end

  configure_os(config, MASTER_IP, SLAVE_IP)

  config.vm.define 'master' do |m|
    m.vm.provider 'virtualbox' do |vb|
      vb.name = 'kubeadm-master'
    end
    m.vm.hostname = 'master'
    m.vm.network 'public_network', bridge: 'enp2s0', ip: MASTER_IP
  end

  config.vm.define 'slave' do |s|
    s.vm.provider 'virtualbox' do |vb|
      vb.name = 'kubeadm-slave'
    end
    s.vm.hostname = 'slave'
    s.vm.network 'public_network', bridge: 'enp2s0', ip: SLAVE_IP
  end

  config.vm.provision :ansible do |ansible|
    ansible.playbook = 'role-kube.yml'
    ansible.config_file = 'ansible.cfg'
  end
end

# This software is copyright (c) 2022 of Alceu Rodrigues de Freitas Junior,
# arfreitas@cpan.org
#
# This file is part of glasswalk3r/kubeadmn.
#
# glasswalk3r/kubeadmn is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.
#
# glasswalk3r/kubeadmn is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with glasswalk3r/kubeadmn.  If not, see http://www.gnu.org/licenses.

# -*- mode: ruby -*-
# vi: set ft=ruby :
