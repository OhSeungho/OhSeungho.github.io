---
title: "WordPress 설치"
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
categories:
- WordPress
toc: true
toc_sticky: true
toc_label: 목차
description: WordPress
article_tag1: VirtualBox
article_tag2: Cent OS
article_section: WordPress
meta_keywords: WordPress
last_modified_at: 2020-07-17T00:00:00+00:00
---
## CentOS 7.8 WordPress 설치

LAMP가 준비되어있는 환경에서 WordPress 설치를 진행합니다

## 1. DB 설정

WordPress를 위한 MariaDB 구축을 진행합니다

~~~bash
mysql -u root -p
~~~

다음과 같은 화면이 진행됩니다

![캡처01](https://user-images.githubusercontent.com/51220344/87906815-59e45080-ca9e-11ea-82b2-85fd83927cf0.PNG)

WordPress에서 사용할 DB를 생성합니다

~~~bash
CREATE DATABASE wordpress CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
~~~

DB 계정, 패스워드를 생성하고, 종료합니다

~~~bash
GRANT ALL ON wordpress.* TO 'wordpressuser'@'localhost' IDENTIFIED BY 'tmdgh0701`;
exit
~~~

## 2. WordPress 설치

