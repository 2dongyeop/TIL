
## 💡 Join이란?

> 두 개 이상의 테이블이나 데이터베이스를 연결하여 데이터를 검색하는 방법이다.

<br/>

<br/>

## 💡Join의 필요성

관계형 데이터베이스의 구조적 특징으로 정규화를 수행하면 의미 있는 데이터의 집합으로 테이블이 구성되고, 각 테이블끼리는 관계(Relationship) 를 갖게 된다.

<br/>

이와 같은 특징으로 관계형 데이터베이스는 저장 공간이 효율성이 향상되게 된다.

<br/>

다른 한편으로는 **서로 관계있는 데이터가 여러 테이블로 나뉘어 저장**되므로, **각 테이블에 저장된 데이터를 효과적으로 검색하기 위해 조인이 필요**하다!

<br/>

<br/>

## 💡 Join 종류

### 논리적 조인

`cross join` : 대상이 되는 두 테이블 간의 카테이션곱을 통하여 데이터를 도출하는 방식

`inner join` : 대상이 되는 두 테이블 간의 교집합을 도출하는 방식

`outer join` : 대상이 되는 두 테이블 간의 합집합을 도출하는 방식

<br/>

<br/>

### 물리적 조인

`Nested Loops join` 

→ NL 조인 같은 경우에는 구동 테이블이 있고 내부 테이블이 존재하는데 구동 테이블을 기준으로 내부 테이블을 반복적으로 탐색해주는 방식

→ 인덱스를 이용한 조인 방식이므로 인덱스 구성에 따른 성능 차이가 심하다.

<br/>

`Sorted Merge join` :

NL 조인을 개선한 방법으로,조인에 참여하는 ***두 집합을 조인 키(칼럼) 순서대로 정렬(sort) 한 다음에 병합(merge) 하는 조인 방식***

NL 조인은 구동 테이블을 기준으로 내부 테이블을 반복적으로 탐색하는데 반해, 소트 머지 조인은 처음에 두 테이블을 조인 키를 통해 정렬을 수행하고 비교하므로 반복적인 탐색이 줄어들게 된다.

<br/>

`Hash join` :

소트 머지 조인에서 두 테이블을 조인하기 전에 sort 하는 연산과정이 필요했는데 hash join은 그러한 연산과정도 필요하지 않다!

해시 조인은 빌드 단계와 probe 단계가 존재하는데 빌드 단계에서는 작은 쪽 테이블을 스캔하여 해시 테이블을 생성한다.

그 후 probe 단계로 넘어가서 큰 쪽 테이블을 스캔하여 만들어진 해시 테이블을 탐색하면서 조인을 수행한다.


<br/>

<img src="https://github.com/2dongyeop/TIL/blob/main/Database/image/join.png" width = 500/>