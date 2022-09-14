# Kruskal MST 알고리즘
- 그래프에서 최소 비용 트리를 구하는 알고리즘이다.

<br/>

## 알고리즘 동작 과정
1. 시작 정점에서부터 출발하여 신장 트리 집합을 단계적으로 확장해 나간다.
    - 시작 단계에서는 시작 정점만이 신장 트리 집합에 포함된다.
    - 신장 트리 집합에 인접한 정점 중, 최저 간선으로 연결된 정점 선택하여 신장 트리 집합에 추가한다.
2. 이 과정을 신장 트리 집합이 `n-1`개의 간선을 가질 때까지 반복한다.

<br/>


<img src="https://github.com/2dongyeop/TIL/blob/main/Algorithm/image/prim.png" width = 500/>

<br/>

### 💡 prim 알고리즘의 특징
- kruskal(간선 기반으로 동작)과 다르게 정점 기반으로 동작한다.
- 이전 단계에서 만들어진 신장 트리를 확장해 나간다.

<br/>

### 💡 prim 알고리즘의 시간 복잡도
- 주 반복문이 정점의 수 n만큼 반복하고, 내부 반복문이 n번 반복하므로
    - Prim의 알고리즘은 O(n^2)의 복잡도를 가진다.
- 희박한 그래프
    - O(e * log e)인 Kruskal의 알고리즘이 유리하다.
- 밀집한 그래프
    - O(n^2)인 Prim의 알고리즘이 유리하다.

<br/>

> Kruskal : 그래프 내에 적은 숫자의 간선만을 가지는 ‘희소 그래프(Sparse Graph)’의 경우 적합
>
> Prim : 그래프에 간선이 많이 존재하는 ‘밀집 그래프(Dense Graph)’ 의 경우 적합


<br/>

---
### 관련 문제 풀이
- [백준 #1197 MST](https://github.com/2dongyeop/baekjoon/tree/main/src/MST)