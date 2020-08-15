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
## Kubernetes 설치

## 구성환경

OS

- Ubuntu 18.04.5 LTS

Program Version

- VirtualBox 6.1.12 r139181 (Qt5.9.5)

- Vagrant 2.2.9

> RAM은 16G 이상이 되어야 구성하는데 불편함이 없습니다

## 설치

VirtualBox


![스크린샷, 2020-08-15 23-47-09](https://user-images.githubusercontent.com/51220344/90314831-a6159a00-df51-11ea-9f3d-95c15c0cc3b2.png)

VirtualBox 홈페이지에서 6.1.12 버전을 다운받습니다

![스크린샷, 2020-08-15 23-46-20](https://user-images.githubusercontent.com/51220344/90314812-8bdbbc00-df51-11ea-8172-4dcc877d20dd.png)

Linux distributions을 선택하고

![스크린샷, 2020-08-15 23-48-22](https://user-images.githubusercontent.com/51220344/90314858-d1988480-df51-11ea-945d-129edd1772a1.png)

VirtualBox Extension Pack도 같이 설치해줍니다

Vagrantfile

![스크린샷, 2020-08-15 23-49-41](https://user-images.githubusercontent.com/51220344/90314873-ff7dc900-df51-11ea-90d0-8476c038ba96.png)

Vagrant 홈페이지에서 Vagrant 2.2.9버전 DEBIAN으로 설치해줍니다

![스크린샷, 2020-08-15 23-51-00](https://user-images.githubusercontent.com/51220344/90314899-2fc56780-df52-11ea-8107-6daea01766ec.png)

아래 명령어로 Vagrant가 잘 설치되었는지 확인합니다

~~~bash
vagrant version
~~~

Vagrant Box는 우분투 공식 18.04 LST 빌드를 활용합니다

![스크린샷, 2020-08-15 23-53-23](https://user-images.githubusercontent.com/51220344/90314958-83d04c00-df52-11ea-8d31-df4125e17ffb.png)

~~~bash
vagrant box add ubuntu/bionic64
~~~

Vagrant Plugin도 설치합니다

~~~bash
vagrant plugin install vagrant-hostmanager
vagrant plugin install vagrant-disksize
~~~

Log가 저장될 공간을 준비하고 그 안에 `Vagrantfile`이름으로 설정 파일을 다음과 같이 준비합니다

1개의 마스터 노드와 3개의 노드로 구성되어있습니다

~~~bash
mkdir ~/vagrant
cd vagrant
vi Vagrantfile
~~~

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

> RAM이 16G이하라면 위 Vagrantfile에서 각 노드 `vb.memory`를 최소 2024(2G)로만 적당히 변경해줍니다

~~~bash
vagrant up
~~~

![스크린샷, 2020-08-16 00-00-56](https://user-images.githubusercontent.com/51220344/90315092-95662380-df53-11ea-9000-addde8f04614.png)

Vagrantfile이 준비된 공간에서 Vagrant를 실행합니다

~~~bash
vagrant status
~~~

![스크린샷, 2020-08-16 00-03-31](https://user-images.githubusercontent.com/51220344/90315133-ee35bc00-df53-11ea-91f0-0e82acfe9d82.png)

구성한 노드들이 잘 작동하는지 확인합니다

~~~bash
vagrant ssh kube-master1
~~~

![스크린샷, 2020-08-16 00-08-00](https://user-images.githubusercontent.com/51220344/90315204-8f247700-df54-11ea-92ac-a74c236e49cc.png)

Master 노드에 SSH 접속합니다

~~~bash
ssh-keygen
ssh-copy-id vagrant@kube-node1
ssh-copy-id vagrant@kube-node2
ssh-copy-id vagrant@kube-node3
ssh-copy-id vagrant@localhost 
~~~

각 노드간 통신을 위해 공개키를 만들어 배포합니다

~~~bash
sudo update
sudo apt install python3 python3-pip git
git clone --single-branch --branch v2.12.3 https://github.com/kubernetes-sigs/kubespray.git
~~~

파이썬과 깃을 설치하고 Kubernetes Github에서 kubespray 디렉토리를 가져옵니다

~~~bash
sudo pip3 install -r requirements.txt
cp -rfp inventory/sample inventory/mycluster
~~~

requirements를 설치합니다 설치되는 내용은 다읍과 같습니다

- Ansible 2.7.112

- Jinja2 2.10.1

/home/vagrant/kubespray/inventory/mycluster 디렉토리의 inventory.ini파일을 다음과 같이 수정합니다

~~~bash
[all]  
kube-master1	ansible_host=192.168.56.11 ip=192.168.56.11 ansible_connection=local
kube-node1      ansible_host=192.168.56.21 ip=192.168.56.21
kube-node2 	    ansible_host=192.168.56.22 ip=192.168.56.22
kube-node3 	    ansible_host=192.168.56.23 ip=192.168.56.23

[all:vars]  
ansible_python_interpreter=/usr/bin/python3

[kube-master]  
kube-master1 

[etcd]  
kube-master1  

[kube-node]  
kube-node1  
kube-node2
kube-node3  

[calico-rr]  

[k8s-cluster:children]  
kube-master  
kube-node  
calico-rr
~~~

![스크린샷, 2020-08-16 00-22-21](https://user-images.githubusercontent.com/51220344/90315472-d3187b80-df56-11ea-9f8b-107b559c2fde.png)

![스크린샷, 2020-08-16 00-22-45](https://user-images.githubusercontent.com/51220344/90315476-df043d80-df56-11ea-96dd-a57f856a77bb.png)

/home/vagrant/kubespray/inventory/mycluster/group_vars/k8s-cluster 디렉토리의 addons.yml파일의 `metric server`와 `nginx` 부분을 true로 변경합니다

Ansible Playbook 명령으로 진행합니다

~~~bash
ansible-playbook -i inventory/mycluster/inventory.ini cluster.yml --become
~~~

![스크린샷, 2020-08-16 00-43-32](https://user-images.githubusercontent.com/51220344/90315837-85e9d900-df59-11ea-8042-43cde4bc1bb6.png)

다음과같이 모든 노드가 정상적으로 구성되면 설치가 끝납니다

> kubectl명령어가 포트 및 권한문제로 인해 사용이 불가하면 다음과 같이 명령어를 입력합니다

~~~bash
mkdir -p $HOME/.kube 
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config 
~~~