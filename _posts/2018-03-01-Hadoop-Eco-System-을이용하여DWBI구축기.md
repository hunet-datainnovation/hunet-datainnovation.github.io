---
layout: post
title: "Hadoop ECO System을 이용한 DW/BI 구축기"
tags: [Hadoop, Spark, SparkSteaming, Kafka, DW, BI, Hive, HBase, Zeppelin, Durid, Imply, Elasticsearch, Logstash, Beats, ELK]
comments: true
---

****

<!-- TOC depthFrom:1 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [1. 왜 Hadoop ECO System을 이용하였는가?](#1-hadoop-eco-system-)
- [2. 데이터분석을 위한 BigData Infra 구축기](#2-bigdata-infra-)
	- [2.1. 진입장벽이 낮은 Elastic Stack 먼저 구축을 해보자](#21-elastic-stack-)
		- [2.1.1. Elastic Stack 문제점](#211-elastic-stack-)
	- [2.2. Zeppelin으로 간단하게 데이터통계를 보자](#22-zeppelin-)
		- [2.2.1. Zeppelin이 총족하지 못한 한가지](#221-zeppelin-)
	- [3. 본격적인 Hadoop ECO System을 구축하자.](#3-hadoop-eco-system-)
		- [3.1. Cloudera를 선택하다.](#31-cloudera-)
- [내용 작성중...](#-)

<!-- /TOC -->

****

# 1. 왜 Hadoop ECO System을 이용하였는가?

이전 회사에서 했던 방식대로 Microsoft SQL Server를 이용하여 DW를 설계하고 적당한 BI 솔루션을 선택하여 DW/BI 구축 프로젝트를 마무리 할 수도 있었지만, 데이터혁신팀이 처한 상황은 이랬다.

	- Microsoft SQL Server 에서 MariaDB로 서비스 이전 계획
	- Opensource 적극도입
	- 향후 기계학습을 위한 인프라 기반 마련

회사에서도 원하고 우리도 하고 싶었으니 아무말 안하고 고고싱 ~~

****

# 2. 데이터분석을 위한 BigData Infra 구축기

먼저, 예전부터 도입하고 싶었던 Elastic Stack를 도입하기로 하였다. 그 이유는

	- Apache Lucene 기반의 빠른 데이터검색
	- 유료 솔루션 버금가는 시각화 솔루션
	- 뛰어난 ETL 솔루션
	- 무엇보다 풍부한 reference와 그나마 쉽게 구축할 수 있다는 점

## 2.1. 진입장벽이 낮은 Elastic Stack 먼저 구축을 해보자

가장 기본적인 구성으로 구축을 진행하였다. MSSQL 또는 MariaDB에 있는 데이터를 Beats 와 Logstash를 이용하여 Elasticsearch로 전송을 하고 Kibana로 시각화를 하였다.

<img src="{{ '/images/20180301/20180301_04.png' }}" alt="">

간단하지만 그 과정은 간단하지 않았던 Elastic Stack를 구축해서 여러가지로 활용을 하였다. 교육시간을 채우지 않고 부정으로 수료하는 사람들 찾는다던지 어떤 교육이 현재 가장 인기가 좋은지 B2C 결제가 얼마나 일어나는지 실 시간으로 시각화를 하고 데이터베이스 모니터링 까지 다방면으로 활용을 하였다.

<img src="{{ '/images/20180301/20180301_05.png' }}" alt="">

궁극적으로는 데이터분석 또는 대쉬보드를 현업에서 직접하기를 원했지만, 현업에서는 Kibana를 사용하기에는 다소 무리가 있었다. 주기적인 교육이 필요했다. 그리고 시계열분석도 필요했지만, 기초통계를 볼 수 있는 무엇인가도 필요했고 시각화도 되면 좋을 것 같은 것을 찾게 되었다.

### 2.1.1. Elastic Stack 문제점

Elastic Stack 자체는 너무나도 훌륭한 빅데이터 인프라 분석도구 이다.  
진입장벽이 낮은 구축, 손쉬운 ETL, 기가막힌 시각화 도구까지 최고의 솔루션이다. 하지만, IT전공자가 아닌 현업의 반응은 조금은 달랐다.

    - Kibana 가 어려워요.

그렇다. Kibana나 자체가 현업분들에게는 조금은 낯설고 어려웠던 것이다.  
다른 무언가가 필요했다.

****

## 2.2. Zeppelin으로 간단하게 데이터통계를 보자

현업들이 어려워하는 Kibana보다 더 쉽고 항상 수시로 요청하는 데이터요청결과값들을 미리 만들어 놓고 조건값만 바꿔서 조회하고 원하는 결과를 얻을 수 있으면 좋지 않을까? 그런게 있을까? 원하는 100%의 기능은 아니지만 Apache Zeppelin이라는 솔루션이 적합해 보였으며, Apache Zeppelin 또한 진입장벽이 높지 않아 큰 어려움 없이 도입을 했고, Elastic Stack에서 권한 관리를 하려면 x-pack라는 별도의 솔루션을 도입해야 하는고 이 x-pack은 유료이지만, Apache Zeppelin 이 녀석은 기가 막히게 무료로 사용자에 권한 관리까지 관리를 할 수 있었다.

<img src="{{ '/images/20180301/20180301_06.png' }}" alt="">

Zeppelin 도입은 성공적이였다.  
많은 팀들이 효과적으로 Zeppelin을 사용하였다. 하지만, 역시 현업에서 요구하는 단 한가지 부분을 충족시키지 못했다.

### 2.2.1. Zeppelin이 총족하지 못한 한가지

    - Excel 로 다운받고 싶은데 데이터가 짤리네요?

그렇다. Zeppelin 다운로드 기능은 MAX값을 넘기면 데이터를 짜른다. 다른곳에서는 Zeppelin 커스텀을 통하여 각자의 방법으로 해결을 한것 같았으나 아쉽게도 데이터혁신팀에는 개발자가 없어 CSV로 다운로드 받는 부분은 포기해야 했다.

****

## 3. 본격적인 Hadoop ECO System을 구축하자.

본격적으로 Hadoop ECO System를 구축하기 위하여 준비를 하기 시작했지만, 쉽지 않았다.  
모든 오픈소스가 그러하듯이 접근장벽이 높았다. 개념도 쉽지 않았고 솔루션 하나하나가 너무나도 어려웠다. 많은 유료 동영상강의 유/무료 오프라인교육 그리고 수 많은 블로그 검색을 통하여 하나하나 구축해 나가기 시작했다.

### 3.1. Cloudera를 선택하다.

처음에 Hadoop을 바이너리로 설치를 하고 설정파일을 관리하려 했다. 하지만, 설치를 하고 R&D하는 과정임에도 불구하고 손이 너무 많이 갔다. 이렇게 손이 많이 가는데 어떻게 운영을하지 무언가 관리툴이 필요하다고 생각을 했고, 분명 누군가 친철하게 만들어 놓은것이 있을거라는걸 믿어 의심하지 않았다. 구글링 하는 도중 Cloudera라는 솔루션이 있는것을 알게 되었고 Cloudera는 Hadoop의 아버지 더그커팅이 몸 담고 있는 그룹이라는 것도 알게 되었다. 지인찬스를 통하여 Cloudera 설치 레퍼런스를 전달 받고 구축을 하게 되었다.

<img src="{{ '/images/20180301/cloudera-logo.png' }}" alt="">






# 내용 작성중...
