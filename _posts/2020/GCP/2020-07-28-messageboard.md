---
title: "GCP dengonban 메시지보드"
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
article_tag1: VirtualBox
article_tag2: Cent OS
article_section: Message Board
meta_keywords: dengonban
last_modified_at: 2020-07-22T00:00:00+00:00
---
## GCP dengonban 메시지보드 생성하기

"프로그래머를 위한 Google Cloud Platform 입문"의 2장에서 사용한 샘플 애플리케이션을 활용하여 메시지보드를 생성해보겠습니다

## 1. 프로젝트 생성

GCP에서 새 프로젝트를 적절하게 생성해줍니다

![01_project](https://user-images.githubusercontent.com/51220344/88675075-84c94700-d125-11ea-9aea-56840c98ab67.PNG)

> 프로젝트 ID는 후에 변경할 수 없으므로 기억하기 쉬운걸로 작성해줍시다

이제 생성한 프로젝트를 선택하고 다음 단계로 넘어갑니다

![02_프로젝트선택](https://user-images.githubusercontent.com/51220344/88675196-a5919c80-d125-11ea-8153-1fc5c160ffa6.PNG)


## 2. VM 인스턴스 생성

새 프로젝트에서 메시지보드로 활용할 VM 인스턴스를 생성합니다

GCP 탐색 메뉴 - Computer Engine를 누르고 새 인스턴스를 생성합니다

![03_인스턴스생성](https://user-images.githubusercontent.com/51220344/88675262-b93d0300-d125-11ea-9d9b-3a681c2f6b88.PNG)

VM 인스턴스 화면에서 만들기를 누릅니다

![04_인스턴스1](https://user-images.githubusercontent.com/51220344/88675683-2355a800-d126-11ea-8b24-d2089ee58ef3.PNG)

다음과 같이 VM 인스턴스를 설정해줍니다

- 리전은 꼭 기억하고 이후 SQL 인스턴스에도 동일한 리전을 설정합니다

- 부팅 디스크는 Debian을 사용합니다

![04_인스턴스1 3](https://user-images.githubusercontent.com/51220344/88675579-07ea9d00-d126-11ea-934a-5786e70f8df7.PNG)

후에 Cloud SQL API를 사용하기위해 `모든 Cloud API에 대한 전체 엑세스 허용`을 선택합니다

![04_인스턴스1 2](https://user-images.githubusercontent.com/51220344/88675489-f608fa00-d125-11ea-8279-161182fa140a.PNG)

HTTP와 HTTPS 트래픽을 허용해줍니다

![05_네트워크](https://user-images.githubusercontent.com/51220344/88675938-729bd880-d126-11ea-8eb6-6f912eb74886.PNG)

네트워크 인터페이스 설정에서 내/외부 IP를 고정으로 설정해줍시다

![09_ssh](https://user-images.githubusercontent.com/51220344/88676244-cd353480-d126-11ea-8066-dfff1f62af9b.PNG)

생성된 VM 인스턴스에 SSH 접속합니다

다음 명령어로 기본 구성을 해줍니다

~~~bash
sudo apt-get -y update
sudo apt-get install git python-pip python-dev python-flask python-wtforms python-arrow  python-flask-sqlalc
hemy python-pymysql python-flaskext.wtf
pip install --upgrade setuptools
pip install -U pyasn1_modules
pip install --upgrade gcloud
~~~

![10_gcp설치](https://user-images.githubusercontent.com/51220344/88676364-edfd8a00-d126-11ea-96dd-87530c87f210.PNG)

> Gcloud 업그레이드 과정이 조금 오래 걸려서 기다려줍니다...

설치한 Git을 이용하여 `/home/계정이름` 디렉토리에 clone 명령을 실행합니다

~~~bash
git clone https://github.com/freelec/gcp-compute-engine
~~~

DB와 스토리지 연동을 하지 않는 v1 메시지보드를 설치하여 테스트해봅시다

app_v1 디렉토리의 설치 쉘스크립트를 실행합니다

~~~bash
cd gcp-compute-engine/app_v1
./install.sh
~~~

`dengonban` 메시지보드 서비스를 실행합니다

~~~bash
systemctl start dengonban.service
~~~

![14_v1test](https://user-images.githubusercontent.com/51220344/88678451-72510c80-d129-11ea-99bd-4303cc2daa5d.PNG)

VM 인스턴스의 외부IP로 접속하여 메시지보드가 정상적으로 작동하는지를 확인합니다

메시지보드가 정상적으로 작동하면 기본적인 VM 인스턴스 준비가 끝났습니다

## 3. SQL 인스턴스 생성

메시지보드에 연동할 SQL 인스턴스를 생성합니다

GCP 탐색 메뉴 - SQL를 누릅니다

![06_sql인스턴스](https://user-images.githubusercontent.com/51220344/88676056-9828e200-d126-11ea-9175-be7206d5af60.PNG)

인스턴스 만들기를 선택합니다

![07_sql인스턴스](https://user-images.githubusercontent.com/51220344/88676094-a37c0d80-d126-11ea-929f-2052a7785253.PNG)

데이터베이스 엔진 선택에서 `MySQL`을 선택합니다

![08_sql인스턴스](https://user-images.githubusercontent.com/51220344/88676137-b1ca2980-d126-11ea-9a56-864e720d91b5.PNG)

VM 인스턴스와 연동하기위해 인스턴스 ID를 `msg-db`로 설정하고 리전도 VM 인스턴스와 동일하게 설정합니다

![11_sql계정](https://user-images.githubusercontent.com/51220344/88676568-30bf6200-d127-11ea-9982-5d6644176229.PNG)

생성한 SQL 인스턴스에 `appuser`계정을 생성하고 비밀번호는 `pas4appuser`로 설정합니다

![21_finish](https://user-images.githubusercontent.com/51220344/88676849-8a279100-d127-11ea-96c3-c66f503891bc.PNG)

`message_db` 이름으로 데이터베이스도 만들어줍니다

![12_sqlssh](https://user-images.githubusercontent.com/51220344/88676649-50568a80-d127-11ea-9261-85de21d7df0b.PNG)

SQL 개요 화면에서 Cloud Shell을 접속할 수 있습니다

![13_sqlssh](https://user-images.githubusercontent.com/51220344/88677109-da9eee80-d127-11ea-89f5-887fde7a2c35.PNG)

Cloud Shell에서 다음과 같은 명령으로 생성한 유저에 DB권한을 부여합니다

~~~bash
grant all privileges on message_db.* to appuser@'%' identified by 'pas4appuser' with grant option;

flush privileges;
~~~

이제 SQL 인스턴스 준비가 끝났습니다

## 4. Storage 버킷 생성

메시지보드 VM 인스턴스와 연동할 Storage 버킷을 생성합니다

GCP 탐색 메뉴 - Storage 를 누르고 버킷 생성을 클릭합니다

![15_버킷](https://user-images.githubusercontent.com/51220344/88677227-fd310780-d127-11ea-8fc6-7c9faf4a8603.PNG)

버킷 이름은 `프로젝트 ID + imagestore`로 설정합니다

## 5. Cloud SQL API 활성화

이제 Cloud SQL API를 활성화합니다

제품 및 리소스 검색에서 Cloud SQL API를 검색합니다

![18_ cloudsqlapiPNG](https://user-images.githubusercontent.com/51220344/88677467-4a14de00-d128-11ea-9a20-c16351292f6b.PNG)

사용을 클릭해줍니다

## 6. Proxy 설정

다시 VM 인스턴스 SSH로 돌아와서 Proxy설정을 진행합니다

wget 명령으로 Cloud SQL Proxy파일을 받아옵니다

~~~bash
apt-get install wget
wget https://dl.google.com/cloudsql/cloud_sql_proxy.linux.amd64
~~~

이제 받아온 파일을 dengonban 설정에 맞게 디렉토리에 옮기고 권한도 바꿔줍니다

~~~bash
mkdir /opt/cloudsqlproxy
mv cloud_sql_proxy.linux.amd64 /opt/cloudsqlproxy/cloud_sql_proxy
chmod a+x /opt/cloudsqlproxy/cloud_sql_proxy
~~~

다음은 메시지보드에서 임시로 데이터를 받아 저장할 디렉토리를 생성합니다

~~~bash
mkdir /cloudsql
chmod 777 /cloudsql/
~~~

## 7. 메시지보드 설치 및 실행

이제 DB와 Storage를 연동하여 메시지보드를 설정하기 위해 `app_v3` 디렉토리로 접근합니다

생성한 SQL 인스턴스 연결 이름이 필요합니다

![16_db인스턴스 이름](https://user-images.githubusercontent.com/51220344/88679522-7af61280-d12a-11ea-9ee8-2a3846d4c0e1.PNG)

`cloudsqlproxy.service` 파일을 다음과 같이 수정합니다

![16_vimproxy](https://user-images.githubusercontent.com/51220344/88679321-497d4700-d12a-11ea-9aa8-6201557262fe.PNG)

- -instance=[SQL 인스턴스 연결 이름]

`app.py` 파일도 수정합니다

![17_vimapppy](https://user-images.githubusercontent.com/51220344/88679631-92cd9680-d12a-11ea-8f0f-3973c6d7bc0e.PNG)

- project_id = [프로젝트 ID]

- dbinstance = [SQL 인스턴스 연결 이름]

이제 쉘스크립트로 메시지보드를 설치합니다

~~~bash
./install.sh
~~~

Cloud SQL Proxy와 메시지보드를 실행하고 확인합니다

~~~bash
systemctl start cloudsqlproxy
systemctl start dengonban.service
systemctl status cloudsqlproxy.service
systemctl status dengonban.service
~~~

![19_ status](https://user-images.githubusercontent.com/51220344/88679992-ffe12c00-d12a-11ea-8b50-6fdca8d311df.PNG)


둘다 잘 작동하는걸 볼 수 있습니다

이제 VM 인스턴스 외부IP로 접속하면

![20_finish](https://user-images.githubusercontent.com/51220344/88680179-2bfcad00-d12b-11ea-87ac-94feae7d5f03.PNG)

SQL DB와 Storage가 연동된 메시지보드가 작동되는걸 볼 수 있습니다

끝