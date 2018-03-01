---
layout: post
title: "Hadoop ECO System을 이용한 DW/BI 구축기"
tags: [Hadoop, Spark, SparkSteaming, Kafka, DW, BI, Hive, HBase, Zeppelin, Durid, Imply, Elasticsearch, Logstash, Beats, ELK]
comments: true
---

## 왜 Hadoop ECO System을 이용하였는가?

이전 회사에서 했던 방식대로 Microsoft SQL Server를 이용하여 DW를 설계하고 적당한 BI 솔루션을 선택하여 DW/BI 구축 프로젝트를 마무리 할 수도 있었지만, 데이터혁신팀이 처한 상황은 이랬다.

****

- Microsoft SQL Server 에서 MariaDB로 서비스 이전 계획
- Opensource 적극도입
- 향후 기계학습을 위한 인프라 기반 마련

****

먼저, 예전부터 도입하고 싶었던 Elastic Stack를 도입하기로 하였다. 그 이유는

****

- Apache Lucene 기반의 빠른 데이터검색
- 유료 솔루션 버금가는 시각화 솔루션
- 뛰어난 ETL 솔루션
- 무엇보다 풍부한 reference와 그나마 쉽게 구축할 수 있다는 점

****

## 간단하게 Elastic Stack 구축

가장 기본적인 구성으로 구축을 진행하였다. MSSQL 또는 MariaDB에 있는 데이터를 Beats 와 Logstash를 이용하여 Elasticsearch로 전송을 하고 Kibana로 시각화를 하였다.


<img src="{{ '/images/20180301/20180301_04.png' }}" alt="">

간단하지만 그 과정은 간단하지 않았던 Elastic Stack를 구축해서 여러가지로 활용을 하였다. 교육시간을 채우지 않고 부정으로 수료하는 사람들 찾는다던지 어떤 교육이 현재 가장 인기가 좋은지 B2C 결제가 얼마나 일어나는지 실 시간으로 시각화를 하고 데이터베이스 모니터링 까지 다방면으로 활용을 하였다.

<img src="{{ '/images/20180301/20180301_05.png' }}" alt="">




<img src="{{ '/images/20180301/20180301_01.png' }}" alt="">
