---
title: "AWS EC2 Elastic IP"
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
categories:
- AWS
toc: true
toc_sticky: true
toc_label: 목차
description: LAMP
article_tag1: VirtualBox
article_tag2: Cent OS
article_section: WordPress
meta_keywords: WordPress, LAMP
last_modified_at: 2020-07-24T00:00:00+00:00
---
## AWS EC2 Elastic IP

Elastic IP Address는 동적 클라우드 컴퓨팅을 위해 고안된 정적 IPv4 주소입니다

Elastic IP Address는 AWS 계정과 연결됩니다

이를 이용하여 주소의 계정의 다른 인스턴스에 신속하게 다시 매핑하여 인스턴스나 소프트웨어의 오류를 마스킹할 수 있습니다

## Elastic IP 

Elastic IP의 기본 특성은 다음과 같습니다

- Elastic IP를 사용하려면 먼저 계정에 주소를 할당한 후 인스턴스 또는 네트워크 인터페이스와 연결합니다

## Elastic IP 할당

1. Amazon EC2 Console 접속

2. 탐색창에서 `Elastic IPs`를 선택합니다

3. Allocate Elastic IP address를 선택합니다

4. 사용할 범위에 따라 `범위`에서 VPC또는 EC2-Classic을 선택합니다

5. (VPC범위에만 해당)Public IPv4 address pool의 경우 다음 중 하나를 선택합니다

* Amazon의 IP 주소 풀 - IP 주소를 Amazon의 IP 주소 풀에 할당하는 경우