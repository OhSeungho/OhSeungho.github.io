---
title: "local repository 생성"
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
description: local repository 생성
article_tag1: VirtualBox
article_tag2: Cent OS
article_section: VM
meta_keywords: VirtualBox, CentOS, VM, 가상머신
last_modified_at: 2020-07-07T00:00:00+00:00
---
## local repository

## 0. createrepo 설치

~~~
yum install createrepo -y
~~~

## 1. rpm 디렉토리 생성

~~~
mkdir /cowsay-local
mv [cowsay rpm file] /[cowsay-local]
~~~

<u>기본 파일들은 백업해두기</u>

## 2. repository 파일 생성

~~~
vi /etc/yum.repos.d/[생성할 repository].repo
[cowsay-local]
name=cowsay local repository generates ASCII pictures of cow with a message
baseurl=http://192.168.56.100/cowsaylocal
# ssh 연결 IP Address
enabled=1 # 
gpgcheck=0 # 
~~~

## 3. repository 생성

~~~
createrepo /cowsaylocal
~~~

## 4. 동작 확인

~~~
yum install cowsaylocal
~~~