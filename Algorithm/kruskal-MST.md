# Kruskal MST 알고리즘
- 그래프에서 최소 비용 트리를 구하는 알고리즘으로, 탐욕적인 방법(greedy method)이다.

<br/>

> 탐욕적인 방법(greedy method)이란?
>
> 
>    - 주요 알고리즘 설계 기법
>    - **각 단계에서 최선의 답을 선택하는 과정을 반복함으로써 최종적인 해답에 도달한다.**
>    - 탐욕적인 방법은 항상 최적의 해답을 주는지 검증이 필요하다.

<br/>


<img src="https://github.com/2dongyeop/TIL/blob/main/Algorithm/image/kruskal.png" width = 500/>

<br/>

## 알고리즘 동작 과정
1. 그래프의 간선들을 가중치의 오름차순으로 정렬되어 있다고 가정한다.
2. 정렬된 간선 중에서 사이클을 형성하지 않는 간선을 현재 MST 집합에 추가한다.
    - 만약 사이클을 형성하면 그 간선은 제외해야 한다.

<br/>

### 💡 사이클 생성 여부를 확인하는 방법
- MST에 추가하고자 하는 간선의 양끝 정점이 같은 집합에 속해 있는지를 먼저 검사해야 한다.
- `union-find` 알고리즘을 이용

<br/>

### 💡 Union-find 알고리즘이란?
- Disjoint Set을 표현할 때 사용하는 알고리즘
  - Disjoint Set이란?
    - 서로 중복되지 않는 부분 집합들 로 나눠진 원소들에 대한 정보를 저장하고 조작하는 자료구조로, 서로소의 집합으로 볼 수 있다.

<br/>

### 💡 union-find의 연산
- `make-set(x)`
  - 초기화
  - x를 유일한 원소로 하는 새로운 집합을 만든다.
- `union(x, y)`
  - 합하기
  - x가 속한 집합과 y가 속한 집합을 합친다. 즉, x와 y가 속한 두 집합을 합치는 연산
- `find(x)`
  - 찾기
  - x가 속한 집합의 대표값(루트 노드 값)을 반환한다. 즉, x가 어떤 집합에 속해 있는지 찾는 연산

<br/>

### 💡 union-find의 기본적인 구현
```C
int root[MAX_SIZE];
for (int i = 0; i < MAX_SIZE; i++)
    parent[i] = i;

/* find(x): 재귀 이용 */
int find(int x) {
    // 루트 노드는 부모 노드 번호로 자기 자신을 가진다.
    if (root[x] == x) {
        return x;
    } else {
        // 각 노드의 부모 노드를 찾아 올라간다.
        return find(root[x]);
    }
}

/* union(x, y) */
void union(int x, int y){
    // 각 원소가 속한 트리의 루트 노드를 찾는다.
    x = find(x);
    y = find(y);

    root[y] = x;
}
```




<br/>

### 💡 Kruskal 알고리즘의 시간 복잡도
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