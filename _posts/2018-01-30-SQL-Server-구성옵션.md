---
layout: post
title: "SQL Server 구성옵션"
author: bumjun.park
tags: [SQL Server 구성옵션, sp_configure]
comments: true
---



## 1. SQL Server 구성옵션이란

### SQL Server 구성옵션
**[SQL Server 구성옵션](https://docs.microsoft.com/ko-kr/sql/database-engine/configure-windows/server-configuration-options-sql-server)** 이란 SQL Server Management Studio 리소스를 관리하고 최적화하도록 설정하는 옵션    
 SQL Server 또는 sp_configure 시스템 저장프로시저를 통해 구성옵션 변경   
 * 설정범위
> 서버 수준의 설정을 표시하거나 변경하려면 SP_CONFIGURE  
데이터 베이스 스준의 설정을 변경하려면 ALTER DATABASE   
현재 사용자 세션에만 적용되는 설정을 변경하려면 SET

* Syntax for SQL Server
> sp_configure [ [@configname = ]'option_name'[,[@configvalue=]'value']]

sp_configure 명령어를 통해 현재 SQL Server에 설정되어 있는 설정값과 최소값, 최대값에 대한 값을 확인할 수 있음  
기본적으로 모든 사용자에게 주어지는 옵션만 확인이 가능하지만
**sp_configure "show advanced options"** 명령어를 사용하게 되면 시스템 저장 프로시저 고급옵션을 설정하게 되어 모든 설정값을 확인할 수 있음

* 결과의미  


| name | minimum | maximum | config_value | run_value |
|:-----|:----:|-----:|:-----|:----:|-----:|
| 구성옵션의 이름  | 구성옵션의 최소값  | 구성옵션의 최대값  | sp_configure를 사용하여 설정한 구성 옵션 값  | 현재 실행중인 구성 옵션에 있는 값  |


####  실행 중인 구성 값 업데이트(RECONFIGURE)

sp_configure 시스템 저장 프로시저로 변경한 구성 옵션의 현재 구성 값을 업데이트하는 옵션  
구성 설정에서 서버를 중지하고 다시 시작하지 않아도 될 경우 현재 실행 중인 값이 업데이트 되도록 지정
새 구성값에서 유효하지 않은 값이나 권장되지 않는 값을 확인  
변경하고자 하는 옵션에 따라 현재 구성 값을 업데이트하기 위해선 서버를 중지하고 다시 시작해야 하기 떄문에 항상 업데이트를 하진 않음


##### Reconfigure & Rconfigure with override   

Reconfigure는 유효하지 않은 값인지 확인해주는 옵션이고 Reconfigure with override를 설정하고자 하는 값이 유효한 범위의 값인지 확인하지 않고 설정하는 옵션

* Syntax for SQL Server
> sp_configure reconfigure [with override]

##### sp_configure 사용권한

매개 변수 없이 또는 첫 번째 매개변수만 사용하여 sp_configure를 실행할 수 있는 권한은 기본적으로 모든 사용자에게 부여됨

구성옵션을 변경하거나 reconfigure문을 실행하는두 매개 변수를 사용하여 sp_configure를 실행하려면 ALTER SETTINGS 서버 수준 권한이 있어야 함

sysadmin 및 serveradmin 고정 서버 역할은 ALTER SETTINGS 권한을 암시적으로 보유





## 2. SQL Server 주요 옵션


#### 1. Adhoc Distributed Queris(임시 분산 쿼리 서버 구성 옵션)
```
1. 기본적으로 SQL Server에선 openrowset 및 opendatasource를 사용하는 임시 분산 쿼리를 허용하지 않음
2. Adhoc Distributed Queris 를 '0' 으로 설정하면 임시 엑세스 허용안함, '1' 로 설정하면 임시 엑세스 허용
3. 임시 분산 쿼리에선 openrowset 및 opendatasource 함수를 사용하여 OLE DB를 사용하는 원격 데이터 원본에 연결
```

#### 2. Agent  XPs
```
1. SQL Server 에이전트 확장 저장 프로시저의 사용여부를 설정하는 옵션
2. SQL Server 에이전트는 SQL Server에서 작업이라고 하는 일정이 지정된 관리 태스크를 실행하는 서비스
3. '0'으로 설정하면 SQL Server 에이전트 확장 저장프로시저 사용 불가, '1'으로 설정하면 사용가능

```

#### 3. Fill Factor(%)

```
1. 인덱스 채우기 비율을 설정하는 옵션
2. 채우기 비율에 따라 각 리프 수준 페이지에서 데이터로 채울 공간의 비율이 결정되므로 나머지 부분을 이후 인덱스
증가에 대비한 여유 공간으로 확보
3. 채우기 비율 값을 적절히 선택하면 기본 테이블에 데이터가 추가될 때 인덱스 확장을 위한 충분한 공간을
제공함으로써 페이지 분할 가능성을 줄일 수 있음
4. 가득찬 인덱스 페이지에 새 행이 추가되면 데이터베이스 엔진에서 행의 절반 정도를 새 페이지로 옮겨 새 행을
위한 공간을 만들게 되는 것이 '페이지분할'
```

#### 4. Index Create memory(KB)
```
1. 인덱스 생성 메모리 옵션은 인덱스를 만들 때 정렬 작업을 위해 처음으로 할당되는 최대 메모리 양을 제어하는 옵션
2. 기본 값은 '0'(자체구성)
3. 추후 인덱스 생성에 메모리가 더 필요하고 해당 메모리를 사용할 수 있는 경우 서버가 이 옵션의 설정 값을 초과하여 메모리를 사용하게 됨
4. 추가 메모리를 사용할 수 없는 경우엔 이미 할당된 메모리를 계쏙 사용하여 인덱스 생성
5. 인덱스 생성 메모리 옵션은 자체 구성되므로 대부분 조정이 필요하지 않지만 인덱스를 만드는데 문제가 발생하면 이 옵션의 값을 변경해야 함
```

#### 5. Max degree of parallelism
```
1. 병렬 계획 실행에 사용할 프로세서 수를 제한하는 구성 옵션
2. Affinity mask에서 가능한 프로세서 수가 적으면 정상 동작하지 않을 수 있음
3. SQL Server에서 정상 동작하기 위해선 SMP(대칭적 다중 처리) 컴퓨터와 같이 둘 이상의 마이크로프로세서 또는 CPU가 있는 컴퓨터에서만 병렬 처리가 가능
4. '0'으로 설정하게 되면 MAX 값인 64개의 프로세서로 실행가능
5. 사용자 사이의 리소스를 관리하기 위해서 적절하게 사용하면 여유있는 프로세서를 다른 트랜잭션에서 사용할 수 있음으로써 효율적으로 사용 가능
```

#### 6. MAX/MIN Server Memory(MB)
```
1. SQL Server는 메모리를 동적으로 사용할 수 있지만, 메모리 옵션을 수동으로 설정하고 SQL Server에 액세스 할 수 있는 메모리 양을 제한할 수 있음
2. 메모리 양을 설정하기 전에 운영체제, max server memory setting에 의해 제어 되지 않는 메모리에 한해서 할당가능
```
#### 7. Max worker threads
```
1. SQL Server 프로세스에 사용할 수 있는 작업자 스레드 수를 구성
2. SQL Server는 운영 체제의 네이티브 스레드 서비스를 사용하여 하나 이상의 스레드가 SQL Serve에서 지원하는 각 네트워크를 동시에 지원하고 또 다른 스레드가 데이터베이스 검사점을 처리하고 스레드 풀이 모든 사용자를 처리
3. 기본 값인 '0'은 SQL Server를 시작할 때  작업자 스레드 수가 자동으로 구성됨
4. 기본 설정은 대부분 시스템에 적합하지만 시스템 구성에 따라 max worker threads를 특정한 값으로 설정하면 성능이 향상될 수 있음
```
#### 8. Min memory per query
```
1. 쿼리 실행을 위해 할당할 최소 메모리 용량(KB)을 지정(기본 1024KB)
2. 단일 쿼리에서 수신할 최소 메모리 양을 지정할 수 있으면 hash작업이나 sorting 작업을 할 경우 쿼리가 많은 메모리를 가져오게 되므로 크게 설정하면 이런 쿼리의 성능은 개선될 수 있지만 최소 메모리 확보시간이나 query wait 서버 구성 옵션에 지정된 값이 초과될 때까지 쿼리가 대기할 수도 있고 리소스의 경합이 심해질 가능성이 있음
3. index create memory 옵션보다 우선 적용
```
#### 9. Ole automation procedures(OLE 자동화 저장프로시저)
```
1. SQL Server는 Transact-SQL 일괄 처리 내에서 OLE 자동화 개체를 사용할 수 있는 시스템 저장 프로시저를 지원
2. 기본적으로 SQL Server는 서버의 보안 구성 중 OLE 자동화 저장프로시저가 설정이 해제되어 있기 때문에 OLE 자동화 저장 프로시저에 대한 액세스를 차단
```
#### 10. Optimize for ad hoc workloads
```
1. SQL Server 2008부터 사용
2. 여러 개의 일회용 임시 일괄 처리를 포함하는 작업에서 계획 캐시의 효율성을 높이는 데 사용
3. 이 옵션을 설정하면 데이터베이스 엔진이 일괄 처리가 처음으로 컴파일 되었을 때 전체 컴파일 된 계획 대신 계획 캐시에 포함된 작은 컴파일 된 계획 stub을 저장
4. 결과적으로 계획 캐시의 사이즈가 커지는 문제를 완화
```
#### 11. User connections
```
1. SQL Server 인스턴스에 허용되는 최대 동시 사용자 연결수를 지정
2. 기본값은 32,767(동시에 최대값)
```
#### 12. User options
```
1. 모든 사용자에 대한 기본값을 지정
2. 중간 또는 지연된 제약 조건 검사, 경고제어, Null 처리 제어 등을 지정
```
#### 13. xp_cmdshell
```
1. xp_cmdshell 확장 저장 프로시저의 실행을 제어하는 구성 옵션

```
