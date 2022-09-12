## Kruskal MST 알고리즘
- 그래프에서 최소 비용 트리를 구하는 알고리즘으로, 탐욕적인 방법(greedy method)이다.

<br/>

> 탐욕적인 방법(greedy method)이란?
>
> 
>    - 주요 알고리즘 설계 기법
>    - **각 단계에서 최선의 답을 선택하는 과정을 반복함으로써 최종적인 해답에 도달한다.**
>    - 탐욕적인 방법은 항상 최적의 해답을 주는지 검증이 필요하다.

<br/>

### 알고리즘 동작 과정
1. 그래프의 간선들을 가중치의 오름차순으로 정렬되어 있다고 가정한다.
2. 정렬된 간선 중에서 사이클을 형성하지 않는 간선을 현재 MST 집합에 추가한다.
    - 만약 사이클을 형성하면 그 간선은 제외해야 한다.

<br/>

### 사이클 생성 여부를 확인하는 방법
- MST에 추가하고자 하는 간선의 양끝 정점이 같은 집합에 속해 있는지를 먼저 검사해야 한다.
- `union-find` 알고리즘을 이용

<br/>

### Union-find 알고리즘이란?
- 설명할 내용이 많아 [링크](https://gmlwjd9405.github.io/2018/08/31/algorithm-union-find.html)를 첨부합니다.


<br/>

### Kruskal 알고리즘의 시간 복잡도
- union-find 알고리즘을 이용하면 Kruskal 알고리즘의 시간 복잡도는 간선들을 정렬하는 시간에 좌우된다.
  - 즉, 간선 e개를 퀵 정렬과 같은 효율적인 알고리즘으로 정렬한다면
Kruskal 알고리즘의 시간 복잡도는 $O(elog2e)$ 이 된다.
- Prim 알고리즘의 시간 복잡도가 $O(n^2)$ 이므로 비교하면?

<br/>

> Kruskal : 그래프 내에 적은 숫자의 간선만을 가지는 ‘희소 그래프(Sparse Graph)’의 경우 적합
>
> Prim : 그래프에 간선이 많이 존재하는 ‘밀집 그래프(Dense Graph)’ 의 경우 적합


<br/>

---
### 관련 문제 풀이
- [백준 #1197 MST](https://github.com/2dongyeop/baekjoon/tree/main/src/MST)

<br/>

### Reference
- [union-find](https://gmlwjd9405.github.io/2018/08/31/algorithm-union-find.html)