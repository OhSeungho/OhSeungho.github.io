---
title: "VirtualBox CentOS 설치"
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
description: VirtualBox를 활용하여 가상머신에 Cent OS 설치해보기
article_tag1: VirtualBox
article_tag2: Cent OS
article_section: VM
meta_keywords: VirtualBox, CentOS, VM, 가상머신
last_modified_at: 2020-06-28T00:00:00+00:00
---

CPU : AMD Ryzen 7 3750H with Radeon Vega Mobile Gfx

<u># AMD CPU 사용자는 Bios에서 SVM Mode를 활성화 해주셔야 됩니다.</u>

RAM : DDR4 PC4-21300 16GB

GPU : NVIDIA GeForce GTX 1660 Ti

OS : Windows 10

Windows 환경에서 가상머신을 사용해 Cent OS를 설치해 보고자 합니다.

## 1. VirtualBox 설치

아래의 링크를 통해 Oracle VM VirtualBox 설치 파일을 다운로드 합니다.

<https://www.virtualbox.org/wiki/Downloads>

![1_virtualbox](https://user-images.githubusercontent.com/51220344/85927725-e312c800-b8e2-11ea-8495-c0e16524d06d.PNG)
<u># 빨간 박스 "Windows hosts 클릭</u>

다운 받은 VirtualBox-xx.xx.xx-xxxxxx-Win.exe를 실행합니다.

![2_virtualbox](https://user-images.githubusercontent.com/51220344/85938763-083b2100-b94b-11ea-999f-3b643318df34.PNG)

딱히 건드릴 설정이 없어 기본으로 Next를 계속 눌러줍니다.

![3_virtualbox](https://user-images.githubusercontent.com/51220344/85938793-37519280-b94b-11ea-9df3-03dc1894fd44.PNG)

그럼 중간에 Warning page가 나오는데 가상머신 환경에서 Network를 사용하기 위함으로 Yes를 눌러주면 됩니다. 그럼 창이 하나 더 뜰텐데 마찬가지로 설치해 주시면 됩니다.

![4_virtualbox](https://user-images.githubusercontent.com/51220344/85938853-cfe81280-b94b-11ea-81c8-48b8afe6c0c0.PNG)

그럼 설치가 진행되면서...

![5_virtualbox](https://user-images.githubusercontent.com/51220344/85938859-db3b3e00-b94b-11ea-82cb-eb8573f06783.PNG)

정상적으로 설치가 완료됩니다.

## 2. Cent OS 다운로드

Cent OS는 "The Community Enterprise Operating System"의 약자로 Cent OS Project에서 RHEL(Red Hat Enterprise Linux)와 제휴를 맺고 개발한 리눅스 운영체제입니다. 이에 대한 자세한 설명은 외부에 다양하게 나와있으니 생략하겠습니다.

Cent OS 사용의 포인트는 거의 완벽하게 포킹한 RHEL를 무료로 쓸수 있다는 점 입니다. 때문에 다양한 기업체, 대학교 등의 서버에서 Cent OS로 많이 운영중에 있고, 후에 RHEL를 사용한다 해도 쉽게 적응할 수 있습니다.

아래의 링크를 통해 Oracle VM VirtualBox 설치 파일을 다운로드 합니다.

<https://www.centos.org>

![6_centos](https://user-images.githubusercontent.com/51220344/85939074-8a2c4980-b94d-11ea-92c2-88abb5202099.PNG)
<u># 빨간 박스 "Download" 클릭</u>

![7_centos](https://user-images.githubusercontent.com/51220344/85939201-7b926200-b94e-11ea-82c9-074a3ca950b7.PNG)
<u># 빨간 박스가 표시된 Cent OS Version 7(1908)과 x86_64 클릭</u>

![8_centos](https://user-images.githubusercontent.com/51220344/85939263-dd52cc00-b94e-11ea-89d0-6167dce3e7be.PNG)

현재 제공중인 mirror링크들입니다. 아무거나 선택하셔도 무방합니다. 저는 첫번째를 선택했습니다.

![9_centos](https://user-images.githubusercontent.com/51220344/85939320-3a4e8200-b94f-11ea-99ea-483c3e64deac.PNG)

이제 사용하실 활용도에 맞게 Cent OS를 선택하시면 됩니다. 여기서 저는 Minimal을 사용합니다.

![10_centos](https://user-images.githubusercontent.com/51220344/85939344-91eced80-b94f-11ea-8e67-f8f9efa5172f.PNG)

다운로드 끝.

## 3. VirtualBox 가상머신 만들기

이제 Oracle VM VirtualBox를 실행해 보겠습니다.

![11_vm](https://user-images.githubusercontent.com/51220344/85939378-eb551c80-b94f-11ea-9960-071a46d7766c.PNG)

Oracle VM VirtualBox의 기본 모습입니다. 이제 가상머신에 Cent OS를 설치해 보겠습니다.

![12_vm](https://user-images.githubusercontent.com/51220344/85939401-1a6b8e00-b950-11ea-8f49-a96dbaef9f6f.PNG)
<u># 빨간 박스가 표시된 "새로 만들기(N)" 클릭</u>

![13_vm](https://user-images.githubusercontent.com/51220344/85939421-60285680-b950-11ea-9075-a0a4067947e8.PNG)

원하는 이름과 디렉토리를 설정해 주시면 됩니다. 보통 이름에 자신이 설치하려는 운영체제가 들어가면 종류와 버전이 자동적으로 선택되지만, Cent OS를 설치하려면 <u>종류(T)에 "Linux"와 버전(V)에 "Red Hat (64-bit)"</u> 가 잘 선택되었는지 한번 확인해줍니다.

![14_vm](https://user-images.githubusercontent.com/51220344/85939501-06745c00-b951-11ea-934a-4556d40f3842.PNG)

가상 머신에 할당할 메모리의 크기를 선택할 수 있습니다. 저는 2048(2 GB)를 넣어줬습니다.

![15_vm](https://user-images.githubusercontent.com/51220344/85939520-302d8300-b951-11ea-9299-238942013730.PNG)

이번엔 디스크 드라이브 생성입니다. 일단 만들기를 클릭합니다.

![16_vm](https://user-images.githubusercontent.com/51220344/85939534-54895f80-b951-11ea-92e2-00d645a5ce95.PNG)

VDI를 선택하고 다음.

![17_vm](https://user-images.githubusercontent.com/51220344/85939542-69fe8980-b951-11ea-8c9a-e6e11c3cbc2e.PNG)

여기서 동적 할당과 고정 크기로 나뉩니다. 동적 할당은 VM에서 사용하는 만큼 유동적으로 드라이브에 데이터를 쓰는거고, 고정 크기는 말 그대로 드라이브에 고정적인 용량의 디스크 파일을 만들어 줍니다. 보통 디스크가 남아 돌지 않는 이상 동적 할당으로 선택해 줍니다.

![18_vm](https://user-images.githubusercontent.com/51220344/85939589-c82b6c80-b951-11ea-893a-7f96cea2488c.PNG)

디스크 크기를 설정해 줍니다. 저는 30GB 만큼 넣어줬습니다. 위에서 동적 할당으로 생성하였기 때문에 실제로 30GB를 차지하지는 않습니다.

![19_vm](https://user-images.githubusercontent.com/51220344/85939610-00cb4600-b952-11ea-92a8-145118d2d776.PNG)

이제 Cent OS를 설치할 가상머신이 준비되었습니다.

## 4. 가상머신에 Cent OS 설치 준비

준비된 가성머신에 다운받은 Cent OS 디스크를 넣기 전에 기본적인 설정을 해 줍니다.

![20_vm](https://user-images.githubusercontent.com/51220344/85939978-97990200-b954-11ea-970d-2af1b7457f6a.PNG)

설정(S)를 클릭.

![21_vm](https://user-images.githubusercontent.com/51220344/85940019-ea72b980-b954-11ea-9788-607df78c5251.PNG)

플로피 디스크를 쓸 일이 없기 때문에 시스템 메뉴에서 마더보드(M)-부팅 순서(B)에 플로피 디스크를 체크 해제해 줍니다.

- 이후 과정은 필요할 경우에 따라 설정해주시면 됩니다.

![22_vm](https://user-images.githubusercontent.com/51220344/85940166-c368b780-b955-11ea-811a-8e78d0ab0125.PNG)

네트워크 설정에서 어댑터 1을 NAT 네트워크로 변경해줍니다.

![23_vm](https://user-images.githubusercontent.com/51220344/85940204-f9a63700-b955-11ea-9e79-7296dca33489.PNG)

어댑터 2를 클릭하여 네트워크 어댑터 사용하기(E)를 체크한 후, 호스트 전용 어댑터로 변경해줍니다.

- 위 과정에서 오류가 날 경우(ex : Ubuntu OS)

![24_vm](https://user-images.githubusercontent.com/51220344/85940268-61f51880-b956-11ea-9798-4093808352cb.PNG)

VirtualBox 기본 환경설정(메인 page에서 Ctrl + G)에서 네트워크 -> 오른쪽 녹색 + 클릭 -> NatNetwork를 활성화 시켜 주면 해결됩니다.

![25_vm](https://user-images.githubusercontent.com/51220344/85940341-a4b6f080-b956-11ea-9840-4f8e1a078ffc.PNG)

이제 메인 page로 돌아와 저장소 -> [광학 드라이브]비어 있음을 클릭 -> 디스크 파일 선택을 클릭하여 2. Cent OS 다운로드 과정에서 다운로드한 디스크 파일을 선택해줍니다.

![26_vm](https://user-images.githubusercontent.com/51220344/85940416-33c40880-b957-11ea-89cf-e2d12f463548.PNG)

디스크 파일이 잘 들어간걸 확인했으면 시작(T)를 눌러 가상머신을 실행합니다.


## 5. Cent OS 설치

![27_vm](https://user-images.githubusercontent.com/51220344/85940444-71289600-b957-11ea-9166-478eeb47887b.PNG)

키보드 방향키로 맨위 Install CentOS 7를 선택하고 Enter키를 입력합니다.

![28_vm](https://user-images.githubusercontent.com/51220344/85940572-2fe4b600-b958-11ea-8733-f095921e26f2.PNG)

그럼 설치가 진행이 되면서...

![29_vm](https://user-images.githubusercontent.com/51220344/85940591-48ed6700-b958-11ea-9a17-d672f73b2757.PNG)

언어 선택창이 나옵니다. 일단은 English로 하고 Continue를 클릭합니다.

![30_vm](https://user-images.githubusercontent.com/51220344/85940674-b7322980-b958-11ea-90bf-6f6abbe58a26.PNG)

설치 메인 화면입니다. DATE & TIME을 클릭합니다.

![31_vm](https://user-images.githubusercontent.com/51220344/85940697-e5176e00-b958-11ea-9fe4-968225909ea0.PNG)

시간대를 설정해 줍니다. City에서 Seoul를 입력하거나, 지도에서 클릭해도 바꿀 수 있습니다. 다 됬다면 왼쪽 위 Done을 클릭합니다.

![30_1_vm](https://user-images.githubusercontent.com/51220344/85940729-260f8280-b959-11ea-8d37-3c26030fe66c.PNG)

일단 다른 설정들은 건들지 말고 Begin Installation을 클릭합니다.

![32_vm](https://user-images.githubusercontent.com/51220344/85940748-3d4e7000-b959-11ea-98cd-86b6f0630fc5.PNG)

이제 CONFIGURATION page가 나옵니다. 먼저 ROOT PASSWORD를 클릭합니다.

![33_vm](https://user-images.githubusercontent.com/51220344/85940768-6bcc4b00-b959-11ea-8c49-860557352ffd.PNG)

Root 계정의 암호를 입력하고 Done을 클릭합니다.

![34_vm](https://user-images.githubusercontent.com/51220344/85940790-93bbae80-b959-11ea-80d0-be66d5ff952b.PNG)

사용자 계정을 설정합니다. Full name을 입력하면 User name도 자동적으로 똑같이 설정이 됩니다. 이름과 암호를 입력하고 Done을 클릭합니다.

![35_vm](https://user-images.githubusercontent.com/51220344/85940807-b51c9a80-b959-11ea-9ab4-8608010a6531.PNG)

설치가 진행되는 동안 기다려줍니다...

![36_vm](https://user-images.githubusercontent.com/51220344/85940827-db423a80-b959-11ea-894d-57a7d30f071c.PNG)

오른쪽 아래에 Reboot 버튼이 활성화되면 클릭해줍니다. 이제 재부팅을 하게되면...

![37_vm](https://user-images.githubusercontent.com/51220344/85940834-eeeda100-b959-11ea-945b-da7f13b7e1e4.PNG)

VirtualBox에 Cent OS 설치가 됩니다.

끝.