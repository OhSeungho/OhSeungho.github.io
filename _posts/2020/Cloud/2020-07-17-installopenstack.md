---
title: "CentOS 7.8 OpenStack 설치"
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
description: OpenStack
article_tag1: VirtualBox
article_tag2: Cent OS
article_section: SSH
meta_keywords: SSH, RSA
last_modified_at: 2020-07-17T00:00:00+00:00
---
## CentOS 7.8 PackStack활용 OpenStack 설치

## 1. VMware CentOS 7.8 minimal 설치

VMware에서 가상머신을 추가합니다

![캡처01](https://user-images.githubusercontent.com/51220344/87735502-a61a6100-c810-11ea-8c19-3ca99eeb88ab.PNG)

Guest OS 선택에서 `Linux`에 `CentOS 7 64-bit` Version을 선택합니다

![캡처02](https://user-images.githubusercontent.com/51220344/87735519-b3375000-c810-11ea-9309-957cf0a9383b.PNG)

적당한 이름을 넣어주고

![캡처03](https://user-images.githubusercontent.com/51220344/87735531-b92d3100-c810-11ea-9199-74821b7bba12.PNG)

Processor Configuration에서 `Number of processors:` 2개를 지정해 줍니다

![캡처04](https://user-images.githubusercontent.com/51220344/87735537-be8a7b80-c810-11ea-99ad-a6a3f865278f.PNG)

Memory는 환경에 따라 다르지만 8 GB 이상을 권장합니다

> OpenStack 설치 과정에서 Memory가 8GB 미만이면 설치가 안되는 경우도 있어 나중에 줄이더라도 지금은 8GB를 선택합니다

![캡처05](https://user-images.githubusercontent.com/51220344/87735543-c3e7c600-c810-11ea-8a77-aecc31f286a3.PNG)

적당한 디스크 사이즈를 정해줍니다

![캡처06](https://user-images.githubusercontent.com/51220344/87735549-c8ac7a00-c810-11ea-9651-4cab3f611103.PNG)

생성된 가상머신에서 `Edit virtual machine settings`를 클릭합니다

![캡처07](https://user-images.githubusercontent.com/51220344/87735560-ccd89780-c810-11ea-869e-ac34c79a8130.PNG)

`Processors` 탭에서 `Virtualize Intel VT-x/EPT or AMD-V/RVI`를 체크합니다

![캡처08](https://user-images.githubusercontent.com/51220344/87735570-d235e200-c810-11ea-81b9-9f0793a7474a.PNG)

CentOS 7.8 minimal ISO 파일을 넣어주고 가상머신을 실행합니다

![캡처09](https://user-images.githubusercontent.com/51220344/87735574-d661ff80-c810-11ea-9adc-c42c13d41580.PNG)

`Install CentOS 7` 엔터 후 잠시 기다립니다

![캡처10](https://user-images.githubusercontent.com/51220344/87735581-db26b380-c810-11ea-96b6-da5bb671d562.PNG)

설치 화면이 나오면 영어를 선택하고 Continue 클릭하면

![캡처11](https://user-images.githubusercontent.com/51220344/87737292-85083f00-c815-11ea-9682-bccb4ef1ebe5.PNG)

다음과 같은 화면이 나옵니다

![캡처12](https://user-images.githubusercontent.com/51220344/87737339-a832ee80-c815-11ea-8c36-350a576e0a00.PNG)

먼저 `DATE & TIME`에서 서울로 시간대를 변경해줍니다

![캡처13](https://user-images.githubusercontent.com/51220344/87737381-c698ea00-c815-11ea-8233-cc1ed1b53542.PNG)

`NETWORK & HOST NAME`에서 `ens33`를 오른쪽 버튼으로 켜줍니다

그 후 밑에 자동으로 설정된 IP 주소를 기억하고 `Configure...`를 클릭합니다

![캡처14](https://user-images.githubusercontent.com/51220344/87737392-cd276180-c815-11ea-8791-542b5087c709.PNG)

`IPv4 Settings` 탭으로 가서 Method를 `Manual`로 바꾸고 아까 기억해둔 IP 주소와 Gateway, DNS를 입력하고 `Save`를 누릅니다

![캡처15](https://user-images.githubusercontent.com/51220344/87737468-0364e100-c816-11ea-9842-87a805a0005f.PNG)

`DATE & TIME` 메뉴로 돌아와서 오른쪽 위 톱니바퀴 버튼으로 통신이 잘 되는지를 확인해줍니다

![캡처16](https://user-images.githubusercontent.com/51220344/87737529-2d1e0800-c816-11ea-8220-55be094802cc.PNG)

`INSTALLATION SOURCE` 메뉴에서 Done 클릭

![캡처17](https://user-images.githubusercontent.com/51220344/87737590-5474d500-c816-11ea-8e6c-65ea6abb35c2.PNG)

이제 `Begin Installation`을 클릭해 다음으로 넘어갑니다

![캡처18](https://user-images.githubusercontent.com/51220344/87737623-6a829580-c816-11ea-8c78-934dea7d7e15.PNG)

`ROOT PASSWORD`와 `USER CREATION`에서 적당한 설정을 해주고 기다립니다

![캡처19](https://user-images.githubusercontent.com/51220344/87737659-88e89100-c816-11ea-9bc4-7a76a2fe7fcb.PNG)

`Reboot` 버튼이 활성화 되면 클릭합니다

![캡처22](https://user-images.githubusercontent.com/51220344/87737696-a61d5f80-c816-11ea-8b70-0f15993b2eea.PNG)

이제 CentOS가 준비되었습니다

`vim, bash-completion` 등 사용하는 툴들을 설치해둡니다

![캡처23](https://user-images.githubusercontent.com/51220344/87737729-bc2b2000-c816-11ea-9204-009586a4b560.PNG)

## 2. OpenStack 설치 준비

> 과정 중간중간 재부팅과 꼼꼼한 설정, Snapshot을 권장합니다

설치된 CentOS에 root 유저로 로그인합니다

이제 `SELinux, firewalld, NetworkManager`를 비활성화 합니다

- SELinux 비활성화

~~~bash
setenforce 0
vim /etc/sysconfig/selinux
~~~

`SELINUX=disabled` 로 바꿔줍니다

![캡처24](https://user-images.githubusercontent.com/51220344/87738203-cc8fca80-c817-11ea-9620-c6d67f4c915c.PNG)

- firewalld 비활성화

~~~bash
systemctl stop firewalld
systemctl disable firewalld
~~~

- NetworkManager 비활성화

~~~bash
systemctl stop NetworkManager
systemctl disable NetworkManager
~~~

시스템을 한번 업데이트 해주고 종료합니다

~~~bash
yum update -y
systemctl poweroff
~~~

다시 `Edit virtual machine settings`로 돌아와서 `Network Adapter`를 추가합니다

`Bridged`를 선택하고 `Replicate physical network connection state`를 체크합니다

![캡처36](https://user-images.githubusercontent.com/51220344/87739024-bf73db00-c819-11ea-8e59-84be441d87ad.PNG)

가상머신을 실행하고 추가한 네트워크 설정을 진행합니다

~~~bash
cd /etc/sysconfig/network-scripts
~~~

![캡처33](https://user-images.githubusercontent.com/51220344/87738607-bf271000-c818-11ea-95c2-d862e88272dd.PNG)

`ifconfig` 명령으로 추가한 네트워크 카드의 디바이스 명을 확인합니다

```bash
ifconfig -a
```

브릿지 네트워크를 추가하기 위해 `ifcfg-br0`와 `ifcfg-ens37` 파일을 생성합니다

생성한 파일들을 다음과 같이 수정합니다

- ifcfg-br0

![캡처34](https://user-images.githubusercontent.com/51220344/87739238-580a5b00-c81a-11ea-99e5-9c11eb2f94ce.PNG)

- ifcfg-ens37

![캡처35](https://user-images.githubusercontent.com/51220344/87739262-65bfe080-c81a-11ea-8643-ebe012673a33.PNG)

> 기존 NAT 네트워크와 겹치지 않게 설정합니다

네트워크를 재시작 하고 연결이 잘 되는지를 확인힙니다

~~~bash
systemctl restart network
ifconfig -a
ping google.com
~~~

## 3. OpenStack PackStack 설치

다음 명령어로 현재 배포된 OpenStack 버전을 확인합니다

~~~bash
yum list centos-release-openstack-* 
~~~

![캡처25](https://user-images.githubusercontent.com/51220344/87739405-be8f7900-c81a-11ea-96a4-68a1664babe4.PNG)

본 포스트에서는 `queens` 버전으로 진행하겠습니다

다음 명령어로 OpenStack과 PackStack 설치를 진행합니다

~~~bash
yum -y install centos-release-openstack-queens
yum -y install openstack-packstack
yum -y update
~~~

> 여기까지 진행됬으면 꼭 스냅샷을 찍어두는 것을 권장힙니다

OpenStack 설치 옵션파일인 answer파일을 생성해서 옵션을 수정하고 설치할 수 있지만, 여러가지 복잡한 과정이 진행되야해서 이번 포스트는 `all-in-one` 옵션으로 진행하겠습니다

설치를 시작하고 기다립니다(디바이스 사양에 따라 최소 10분 이상 걸립니다)

~~~bash
packstack --allinone
~~~

![캡처29](https://user-images.githubusercontent.com/51220344/87739998-1bd7fa00-c81c-11ea-9de0-d4edfdeeb0de.PNG)

정상적으로 설치가 완료되면 다음과 같은 화면이 나옵니다

![캡처30](https://user-images.githubusercontent.com/51220344/87740087-4b870200-c81c-11ea-8640-175f5c91e983.PNG)


## 4. OpenStack Dashboard 확인

IP 주소로 OpenStack Dashboard에 접근할 수 있습니다

![캡처37](https://user-images.githubusercontent.com/51220344/87740636-a2410b80-c81d-11ea-9c91-086d02d1fbc4.PNG)

admin 계정으로 로그인을 진행합니다

`all-in-one` 옵션으로 진행했기 때문에 `keystonerc_admin` 파일에서 패스워드를 확인합니다

![캡처39](https://user-images.githubusercontent.com/51220344/87740579-7de52f00-c81d-11ea-874b-aeac192bdb54.PNG)

확인한 패스워드로 로그인을 합니다

![캡처38](https://user-images.githubusercontent.com/51220344/87740413-1929d480-c81d-11ea-877d-c35e92768036.PNG)

끝