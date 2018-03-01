---
layout: post
title: "Hadoop ECO System을 이용한 DW/BI 구축기"
tags: [Hadoop, Spark, SparkSteaming, Kafka, DW, BI, Hive, HBase, Zeppelin, Durid, Imply, Elasticsearch, Logstash, Beats, ELK]
comments: true
---

# 왜 Hadoop ECO System을 이용하였는가?

이전 회사에서 했던 방식대로 Microsoft SQL Server를 이용하여 DW를 설계하고 적당한 BI 솔루션을 선택하여 DW/BI 구축 프로젝트를 마무리 할 수도 있었지만, 데이터혁신팀이 처한 상황은 이랬다.

****
- Microsoft SQL Server 에서 MariaDB로 서비스 이전 계획
- Opensource 적극도입
- 향후 기계학습을 위한 인프라 기반 마련
****

먼저, 예전부터 도입하고 싶었던 Elastic Stack를 도입하기로 하였다.
그 이유는

****
- Apache Lucene 기반의 빠른 데이터검색
- 유료 솔루션 버금가는 시각화 솔루션
- 뛰어난 ETL 솔루션
- 무엇보다 풍부한 reference와 그나마 쉽게 구축할 수 있다는 점
****
*
*
*
*




<img src="{{ '/images/20180301/20180301_01.png' }}" alt="">
