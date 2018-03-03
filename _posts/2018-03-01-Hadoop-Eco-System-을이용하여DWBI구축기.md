---
layout: post
title: "Hadoop ECO System을 이용한 DW/BI 구축기 - (1)"
tags: [Hadoop, Spark, SparkSteaming, Kafka, DW, BI, Hive, HBase, Zeppelin, Durid, Imply, Elasticsearch, Logstash, Beats, ELK]
comments: true
---

****

<!-- TOC depthFrom:1 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [1. 왜 Hadoop ECO System을 이용하였는가?](#1-hadoop-eco-system-)
- [2. 데이터분석을 위한 BigData Infra 구축기](#2-bigdata-infra-)
	- [2.1. 진입장벽이 낮은 Elastic Stack 먼저 구축을 해보자](#21-elastic-stack-)
	- [2.2. Zeppelin으로 간단하게 데이터통계를 보자](#22-zeppelin-)

<!-- /TOC -->

****

# 1. 왜 Hadoop ECO System을 이용하였는가?

이전 회사에서 했던 방식대로 Microsoft SQL Server를 이용하여 DW를 설계하고 적당한 BI 솔루션을 선택하여 DW/BI 구축 프로젝트를 마무리 할 수도 있었지만, 데이터혁신팀이 처한 상황은 쉽지 않았습니다.

	- Microsoft SQL Server 에서 MariaDB로 서비스 이전 계획
	- Opensource 적극도입
	- 향후 기계학습을 위한 인프라 기반 마련

회사에서도 원하고 우리도 하고 싶었으니 아무말 안하고 고고싱 ~~

****

# 2. 데이터분석을 위한 BigData Infra 구축기

먼저, 예전부터 도입하고 싶었던 Elastic Stack를 도입하기로 하였습니다. 그 이유는

	- Apache Lucene 기반의 빠른 데이터검색
	- 유료 솔루션 버금가는 시각화 솔루션
	- 뛰어난 ETL 솔루션
	- 무엇보다 풍부한 reference와 그나마 쉽게 구축할 수 있다는 점

## 2.1. 진입장벽이 낮은 Elastic Stack 먼저 구축을 해보자

Elasticsearch는 가장 기본적인 구성으로 구축을 진행하였습니다. MSSQL 또는 MariaDB에 있는 데이터를 Beats 와 Logstash를 이용하여 Elasticsearch로 전송을 하고 Kibana로 시각화를 하였습니다. 중간 중간에 쓸데없는 기교를 부렸습니다만.. ^^;; 여튼..

<img src="{{ '/images/20180301/20180301_04.png' }}" alt="">

간단하지만 그 과정은 간단하지 않았지만 Elastic Stack를 구축해서 여러가지로 잘 활용흘 하였습니다. 교육시간을 채우지 않고 부정으로 수료하는 사람들 찾는다던지 어떤 교육이 현재 가장 인기가 좋은지 B2C 결제가 얼마나 일어나는지 실 시간으로 시각화를 하고 데이터베이스 모니터링 까지 다방면으로 활용을 하고 있습니다.

<img src="{{ '/images/20180301/20180301_05.png' }}" alt="">

데이터혁신팀에서는 궁극적으로는 데이터분석 또는 대쉬보드를 현업에서 직접하기를 원했지만, 현업에서 받아드리는 Kibana는 "어렵다" 였습니다. 현업에서 원활하게 사용하려면 Kibana 교육이 필요했습니다. 그리고 시계열분석도 필요했지만, 현업에서 쉽게 기초통계를 볼 수 있는 무엇인가도 필요했고 시각화도 되면 좋을 것 같은 것을 찾게 되었습니다.

****

## 2.2. Zeppelin으로 간단하게 데이터통계를 보자

현업들이 어려워하는 Kibana보다 더 쉽고 항상 수시로 요청하는 데이터요청결과값들을 미리 만들어 놓고 조건값만 바꿔서 조회하고 원하는 결과를 얻을 수 있으면 좋지 않을까? 그런게 있을까? 원하는 100%의 기능은 아니지만 Apache Zeppelin이라는 솔루션이 적합해 보였으며, Apache Zeppelin 또한 진입장벽이 높지 않아 큰 어려움 없이 도입을 했고, Elastic Stack에서 권한 관리를 하려면 x-pack라는 별도의 솔루션을 도입해야 하는고 이 x-pack은 유료이지만, Apache Zeppelin 이 녀석은 기가 막히게 무료로 사용자에 권한 관리까지 관리를 할 수 있었습니다.

<img src="{{ '/images/20180301/20180301_06.png' }}" alt="">

Zeppelin 도입은 꽤나 성공적이였습니다.
많은 팀들이 효과적으로 Zeppelin을 사용을 하였지만, 역시 현업에서 요구하는 단 한가지 부분을 충족시키지 못했습니다.

    - 데이터를 엑셀로 다운로드 할 수 있나요?

Zeppelin R&D 결과 어느정도 CSV로 다운로드 가능하지만 설정값에 지정해 놓은 데이터크기보다 크면 데이터를 짜르고 해당 설정값에는 최고치가 있어서 그 이상 설정이 되지 않았습니다. 다른곳에서는 Zeppelin 커스텀을 통하여 각자의 방법으로 해결을 한것 같았으나 아쉽게도 데이터혁신팀에는 개발자가 없어 CSV로 다운로드 받는 부분은 포기해야 했습니다.

****

여러가지 아쉬움과 성취감을 뒤로 한채 본격적으로 Hadoop ECO System을 이용한 DW/BI 구축기를 다음 포스팅에서 소개를 해 드릴까 합니다. 대단한거 하나 없습니다. 다른곳에서 대단하신 분들이 이미 다 한것을 한번 해본거 뿐이 없고 그냥 우리도 이렇게 했다? 그냥 그 정도 입니다. ^^;

감사합니다.
