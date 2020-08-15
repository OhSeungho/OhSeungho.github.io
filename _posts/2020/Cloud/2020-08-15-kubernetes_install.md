---
title: "Vagrant 활용 Kubernetes 실습 환경 구성"
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
categories:
- Cloud
toc: true
toc_sticky: true
toc_label: 목차
description: 
article_tag1: 
article_tag2: 
article_section: 
meta_keywords: 
last_modified_at: 2020-08-15T00:00:00+00:00
---
## Vagrant 활용 Kubernetes 실습 환경 구성

Vagrantfile

~~~ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.define "kube-master1" do |config|
    config.vm.box = "ubuntu/bionic64"
    config.vm.provider "virtualbox" do |vb|
      vb.name = "kube-master1"
      vb.cpus = 2
      vb.memory = 3072
    end
    config.vm.hostname = "kube-master1"
    config.vm.network "private_network", ip: "192.168.56.11"
    config.disksize.size = "30GB"
  end
  config.vm.define "kube-node1" do |config|
    config.vm.box = "ubuntu/bionic64"
    config.vm.provider "virtualbox" do |vb|
      vb.name = "kube-node1"
      vb.cpus = 2
      vb.memory = 3072
    end
    config.vm.hostname = "kube-node1"
    config.vm.network "private_network", ip: "192.168.56.21"
    config.disksize.size = "30GB"
  end
  config.vm.define "kube-node2" do |config|
    config.vm.box = "ubuntu/bionic64"
    config.vm.provider "virtualbox" do |vb|
      vb.name = "kube-node2"
      vb.cpus = 2
      vb.memory = 3072
    end
    config.vm.hostname = "kube-node2"
    config.vm.network "private_network", ip: "192.168.56.22"
    config.disksize.size = "30GB"
  end
  config.vm.define "kube-node3" do |config|
    config.vm.box = "ubuntu/bionic64"
    config.vm.provider "virtualbox" do |vb|
     vb.name = "kube-node3"
      vb.cpus = 2
      vb.memory = 3072
    end
    config.vm.hostname = "kube-node3"
    config.vm.network "private_network", ip: "192.168.56.23"
    config.disksize.size = "30GB"
  end

  # Hostmanager plugin
  config.hostmanager.enabled = true
  config.hostmanager.manage_guest = true

  # Enable SSH Password Authentication
  config.vm.provision "shell", inline: <<-SHELL
    sed -i 's/ChallengeResponseAuthentication no/ChallengeResponseAuthentication yes/g' /etc/ssh/sshd_config
    sed -i 's/archive.ubuntu.com/ftp.daum.net/g' /etc/apt/sources.list
    sed -i 's/security.ubuntu.com/ftp.daum.net/g' /etc/apt/sources.list
    systemctl restart ssh
  SHELL
end
~~~