# frozen_string_literal: true

Vagrant.require_version '>= 1.8'

Vagrant.configure('2') do |config|
  config.vm.box = 'boxomatic/rocky-8'
  config.vm.box_check_update = true
  config.vbguest.auto_update = true
  config.vm.network 'public_network', bridge: 'enp2s0'

  config.vm.provider 'virtualbox' do |vb|
    # Display the VirtualBox GUI when booting the machine
    # Useful for troubleshooting when true
    vb.gui = false
    vb.memory = 2057 # 2G - 9MB for video RAM
    vb.cpus = 2
    vb.customize ['modifyvm', :id, '--vrde', 'off']
    vb.customize ['modifyvm', :id, '--vram', 9]
    vb.customize ['modifyvm', :id, '--graphicscontroller', 'vmsvga']
    vb.check_guest_additions = false
    vb.linked_clone = true
  end

  config.vm.provision 'shell', inline: 'dnf upgrade -y'

  config.vm.define 'master' do |m|
    m.vm.provider 'virtualbox' do |vb|
      vb.name = 'kubeadm-master'
    end
    m.vm.hostname = 'master'
  end

  config.vm.define 'slave' do |s|
    s.vm.provider 'virtualbox' do |vb|
      vb.name = 'kubeadm-slave'
    end
    s.vm.hostname = 'slave'
  end

  config.vm.provision :ansible do |ansible|
    ansible.playbook = 'role-kube.yml'
    config_file = 'ansible.cfg'
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
