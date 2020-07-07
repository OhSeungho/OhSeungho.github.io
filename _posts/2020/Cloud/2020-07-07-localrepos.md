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

'''linux
yum install createrepo -y
'''

## 1. rpm 디렉토리 생성

'''linux
mkdir /cowsaylocal
mv [cowsay rpm file] /[cowsaylocal]
```

<u>기본 파일들은 백업해두기</u>

## 2. repository 파일 생성

```linux
vi /etc/yum.repos.d/[생성할 repository].repo
[cowsaylocal]
name=cowsay local repository generates ASCII pictures of cow with a message
baseurl=///cowsaylocal
# ssh로 연결해서 배포할 IP Address
enabled=1 # 
gpgcheck=0 # 
```

## 3. repository 생성

```linux
createrepo /cowsaylocal
```

## 4. 확인

```linux
yum repolist
```