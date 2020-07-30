---
title: "CentOS 7.8 LAMP 설치"
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
description: LAMP
article_tag1: VirtualBox
article_tag2: Cent OS
article_section: WordPress
meta_keywords: WordPress, LAMP
last_modified_at: 2020-07-17T00:00:00+00:00
---
## WordPress 설치를 위한 CentOS 7.8 LAMP 설치

WordPress는 템플릿 시스템을 활용하는 세계 최대의 오픈 소스 저작물 관리 시스템 입니다

이 WordPress를 활용하기 위해 LAMP(Linux, Apache, MariaDB, PHP)가 설치되어야 합니다

LAMP 말고도 MAMP, FAMP 등이 있지만 가장 보편적인 LAMP로 설치를 진행하겠습니다

## 1. Linux

CentOS Version 확인은

~~~bash
cat /etc/os-release
~~~

위 명령으로 확인할 수 있습니다

![캡처01](https://user-images.githubusercontent.com/51220344/87903955-39b19300-ca98-11ea-90c7-74391f770683.PNG)

Update를 진행합니다

~~~bash
yum update
~~~

## 2. Apache

Apache를 설치하고, 실행 및 적용합니다

~~~bash
yum install -y httpd
systemctl start httpd
systemctl enable httpd
systemctl status httpd
~~~

![캡처02](https://user-images.githubusercontent.com/51220344/87904172-b3498100-ca98-11ea-8423-839cc9988a88.PNG)

이제 방화벽에 새로운 Zone을 생성하고 서비스를 등록합니다

~~~bash
firewall-cmd --permanent --new-zone=wordpress
firewall-cmd --set-default-zone=wordpress
firewall-cmd --permanent --zone=wordpress --add-service=http
firewall-cmd --permanent --zone=wordpress --add-service=https
firewall-cmd --permanent --zone=wordpress --add-service=ssh
firewall-cmd --reload
~~~

서비스가 재대로 설정되었는지 확인해줍니다

![캡처03](https://user-images.githubusercontent.com/51220344/87904360-1804db80-ca99-11ea-92c1-48708d88b0d8.PNG)

방화벽 설정이 끝나면 Apache의 Testing Page에 들어가 확인해봅니다

![캡처04](https://user-images.githubusercontent.com/51220344/87905984-b5adda00-ca9c-11ea-8fc5-1b8b1cdf028d.PNG)

## 3. MariaDB

이제 CentOS는 기본적으로 MySQL대신 MariaDB를 사용합니다

MySQL이 오라클에 인수되면서 기존 개발자가 나와서 호환되게 만든 것이 MariaDB 입니다

예전에는 WordPress를 설치할때 MySQL을 썻지만 이제는 오픈소스인 MariaDB를 사용합니다

MariaDB를 설치하고, 세팅합니다

~~~bash
yum install mariadb-server mariadb
systemctl start mariadb
systemctl enable mariadb
systemctl enable mariadb
~~~

![캡처05](https://user-images.githubusercontent.com/51220344/87904678-cad53980-ca99-11ea-92b3-ba32e94e209a.PNG)

MariaDB Secure 설정을 진행합니다

~~~bash
mysql_secure_installation
~~~

- Enter current password for root (enter for none):

Enter로 넘어갑니다(방금 설치했기 때문에 패스워드가 없습니다)

- Set root password? [Y/n]

Y로 root 계정의 패스워드를 설정합니다

- New password: <br>Re-enter new password: 

새 root 패스워드를 설정합니다

- Remove anonymous users? [Y/n]

Y / 인증된 계정만 접속 가능하게 하는지

- Disallow root login remotely? [Y/n]

Y / MariaDB root 계정이 외부접속을 막는지

- Remove test database and access to it? [Y/n]

Y / Test용 DB를 삭제하는지

- Reload privilege tables now? [Y/n]

Y / 설정을 바로 적용하는지

이렇게 MariaDB 설치가 끝납니다

## 4. PHP

PHP 설치를 진행합니다

보통은 `yum install php php-mysql`로 간편하게 설치할 수 있지만

PHP 5.4 Version이 설치됩니다

![캡처06](https://user-images.githubusercontent.com/51220344/87905239-120ffa00-ca9b-11ea-8302-ef007ef96e41.PNG)

본 포스트에서는 7.2 Version으로 설치를 진행합니다

epel 저장소와 Webtatic 저장소를 추가합니다

~~~bash
yum install epel-release
rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
~~~

이제 PHP 7.2 Version과 라이브러리를 설치합니다

~~~bash
yum install mod_php72w php72w-cli
yum install php72w-bcmath php72w-gd php72w-mbstring php72w-mysqlnd php72w-pear php72w-xml php72w-xmlrpc php72w-process
~~~

`php -v` 명령으로 설치된 Version을 확인할 수 있습니다

![캡처07](https://user-images.githubusercontent.com/51220344/87905230-09b7bf00-ca9b-11ea-9847-c99fc96a0972.PNG)

PHP를 시작하고, 등록합니다

~~~bash
systemctl restart httpd
systemctl enable httpd
~~~

설치된 PHP를 확인하기 위해 Apache 기본 디렉토리 `/var/www/html/`에 `info.php` 샘플 파일을 생성하고, Apache 접근 권한을 줍니다

~~~bash
vim /var/www/html/info.php
chown -R apache: /var/www/html/info.php
~~~

다음과 같이 파일을 작성합니다

![캡처08](https://user-images.githubusercontent.com/51220344/87905415-792dae80-ca9b-11ea-9b8f-cba76b7a4343.PNG)

브라우저에서 `http://*Server IP*/info.php`로 확인해 봅니다

![캡처09](https://user-images.githubusercontent.com/51220344/87906006-bf374200-ca9c-11ea-858e-e2cde8956d28.PNG)

## 5. phpMyAdmin

phpMyAdmin은 Apache, PHP와 연동해 MySQL(MariaDB)를 편하게 관리하는 툴입니다

phpMyAdmin을 설치합니다

~~~bash
yum install phpmyadmin
~~~

config 파일을 수정하여 접근하려는 IP를 추가합니다

~~~bash
vim /etc/httpd/conf.d/phpMyAdmin.conf
~~~

![캡처11](https://user-images.githubusercontent.com/51220344/87905740-3b7d5580-ca9c-11ea-8372-17cb9ef45e3f.PNG)

> 

브라우저에서 `*Server IP*/phpmyadmin`으로 접속하면

![캡처12](https://user-images.githubusercontent.com/51220344/87906021-c8c0aa00-ca9c-11ea-8bba-304713b82d5f.PNG)

다음과 같은 로그인 화면이 나옵니다

앞서 설정해준 계정과 패스워드를 입력하면

![캡처13](https://user-images.githubusercontent.com/51220344/87906033-d2e2a880-ca9c-11ea-9921-4f51c3e25f06.PNG)

phpMyAdmin에 접속이 됩니다

> 다음 포스트에서 WordPress 설치를 진행하겠습니다