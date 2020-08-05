---
title: "GCP Wordpress"
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
categories:
- GCP
toc: true
toc_sticky: true
toc_label: 목차
description: LAMP
article_tag1: GCP
article_tag2: Cent OS
article_section: WordPress
meta_keywords: WordPress, LAMP
last_modified_at: 2020-07-31T00:00:00+00:00
---
## GCP Wordpress 설치하기

GCP에서 Wordpress를 사용해 봅시다

WordPress는 무료 오픈소스 콘텐츠 관리 시스템(CMS)이며 세계 최대의 자체 호스팅 블로그 도구입니다

> GCP에서는 사실 클릭 한 번으로 WordPress의 단일 인스턴스를 Compute Engine에 배포할 수 있습니다

<https://cloud.google.com/wordpress?hl=ko>

## 1. VM 인스턴스 생성

Wordpress 설치를 위한 VM 인스턴스를 새로 하나 만들어줍시다

![01_instance](https://user-images.githubusercontent.com/51220344/89009617-dbae6680-d347-11ea-9d0c-0fecc12609ce.PNG)

리전과 머신 구성 등 적당히 설정해주시고 이번 포스트에서는 `CentOS 7`을 이용하겠습니다

![02_instance](https://user-images.githubusercontent.com/51220344/89009725-26c87980-d348-11ea-802f-09710d21bc66.PNG)

Cloud API 엑세스 허용과 HTTP, HTTPS 트래픽 허용을 체크해줍니다

![03_instance](https://user-images.githubusercontent.com/51220344/89009756-3cd63a00-d348-11ea-870c-81631ff93c7e.PNG)

외부 IP는 고정으로 새로 하나 할당해주고 마무리합니다

![04_instance](https://user-images.githubusercontent.com/51220344/89009784-4cee1980-d348-11ea-8be1-cbd3ce6e2fc9.PNG)

이제 인스턴스가 생성되었으니 SSH 연결로 접속합니다

## 2. LAMP 구성

Wordpress 구성을 위한 Apache, MySQL(MariaDB), PHP를 설치합니다

- Apache

다음 명령으로 업데이트와 httpd 설치를 진행합니다

![05_httpd](https://user-images.githubusercontent.com/51220344/89009836-68592480-d348-11ea-864e-692524bc73b3.PNG)

~~~bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
~~~

![06_httpd](https://user-images.githubusercontent.com/51220344/89010046-d30a6000-d348-11ea-924c-a3c950a278cc.PNG)

VM 인스턴스 외부 IP에서 httpd가 잘 동작하는지 확인해줍니다

- MySQL

VM 인스턴스에 MariaDB를 설치합니다

~~~bash
yum install mariadb mariadb-server
~~~

GCP의 SQL을 Wordpress Server VM 인스턴스에 Proxy 구성 연결하여 사용합니다

![08_sql](https://user-images.githubusercontent.com/51220344/89010398-7d828300-d349-11ea-9bdf-bf6d4060df7a.PNG)

MySQL을 선택하여 리전을 VM 인스턴스와 동일한 SQL 인스턴스를 생성합니다

![09_sql](https://user-images.githubusercontent.com/51220344/89010471-9854f780-d349-11ea-8f7f-21b2a95a3ae9.PNG)

생성한 SQL 인스턴스에 Wordpress에서 사용할 DB를 만들어줍니다

- PHP

다음 명령으로 PHP를 설치합니다

~~~bash
yum install epel-release
rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
yum install mod_php72w php72w-cli
yum install php72w-bcmath php72w-gd php72w-mbstring php72w-mysqlnd php72w-pear php72w-xml php72w-xmlrpc php7
2w-process
php -v
~~~

![07_php](https://user-images.githubusercontent.com/51220344/89010160-03ea9500-d349-11ea-8ded-bd23b30204e3.PNG)

PHP가 7.2 버전으로 설치되었는지 확인합니다

## 3. Wordpress 설치

다음 명령으로 Wordpress를 설치합니다

~~~bash
wget "http://wordpress.org/latest.tar.gz"
tar -xvzf latest.tar.gz -C /var/www/html
chown -R apache: /var/www/html/wordpress
~~~

![10_wp](https://user-images.githubusercontent.com/51220344/89010532-bb7fa700-d349-11ea-8fbb-afd178b5fb57.PNG)

`/var/www/html` 경로에 Wordpress가 준비되었는지 확인합니다

이제 가상 서버 설정을 진행합니다

~~~bash
vim /etc/httpd/conf/httpd.conf
~~~

![11_wp](https://user-images.githubusercontent.com/51220344/89010650-feda1580-d349-11ea-8309-d8491c1ffc93.PNG)

~~~bash
<VirtualHost *:80>
	ServerAdmin admin@wp.com
	DocumentRoot /var/www/html/wordpress
	ServerName wp.com
	ServerAlias www.wp.com
	ErrorLog /var/log/httpd/tecminttest-error-log
	CustomLog /var/log/httpd/tecminttest-acces-log common
</VirtualHost>
~~~

적당한 ServerAdmin, ServerName, ServerAlias를 지정해 줍니다

## 4. SQL 인스턴스 Proxy 연결 구성

먼저 SELinux를 일시정지 시켜줍니다

~~~bash
setenforce 0
~~~

LAMP 준비 과정에서 생성한 SQL 인스턴스를 Proxy 연결을 통해 Wordpress를 준비합니다

~~~bash
wget https://dl.google.com/cloudsql/cloud_sql_proxy.linux.amd64 -O cloud_sql_proxy && chmod +x cloud_sql_proxy
export SQL_CONNECTION=[SQL 인스턴스 연결 이름]
./cloud_sql_proxy -instances=$SQL_CONNECTION=tcp:3306 &
~~~

![11_wp_proxy](https://user-images.githubusercontent.com/51220344/89010831-4e204600-d34a-11ea-8569-da9da9c0a4b5.PNG)

`Ready for new connections`메세지가 출력되면 정상적인 구성이 된 것 입니다

![18_proxy_connect](https://user-images.githubusercontent.com/51220344/89011491-6349a480-d34b-11ea-8ef0-19929557b55b.PNG)

> Wordpress를 설정하면서 127.0.0.1 SQL 인스턴스와 연결을 하는것을 볼 수 있습니다

## 5. Wordpress 설정

이제 VM 인스턴스 외부 IP로 Wordpress 설정을 시작합니다

![12_wp](https://user-images.githubusercontent.com/51220344/89011130-d7377d00-d34a-11ea-8667-6977efe57967.PNG)

한국어를 선택합니다

![13_wp](https://user-images.githubusercontent.com/51220344/89011199-ed453d80-d34a-11ea-922d-1b784b90e429.PNG)

SQL 인스턴스 데이터베이스 이름과 사용자명, 암호를 넣어주고

> 데이터베이스 호스트를 Proxy 연결 구성했기 때문에 localhost인 127.0.0.1로 연결합니다

전송을 클릭합니다

![14_wp](https://user-images.githubusercontent.com/51220344/89011288-11088380-d34b-11ea-87ac-b12d83bc808a.PNG)

이제 준비가 마무리되갑니다

사이트 제목 등 설정을 진행합니다

![15_wp](https://user-images.githubusercontent.com/51220344/89011319-21206300-d34b-11ea-9654-f1507124d356.PNG)

로그인 클릭

![16_wp](https://user-images.githubusercontent.com/51220344/89011341-29789e00-d34b-11ea-9899-f8309a25385f.PNG)

아까 설정해줬던 사용자 계정과 암호로 로그인하면

![17_wp](https://user-images.githubusercontent.com/51220344/89011443-4e6d1100-d34b-11ea-87e0-ceb4ed5290e2.PNG)

Wordpress 대시보드로 접근이되며 설치가 마무리됩니다

끝