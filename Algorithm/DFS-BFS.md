# DFS & BFS
- 두 알고리즘 모두 그래프 알고리즘으로, 문제를 풀 때 상당히 많이 사용한다.

<br/>

## DFS
- 깊이 우선 탐색이며 DFS(Depth-First Search)라고 부른다.
- 루트 노드 혹은 임의 노드에서 다음 브랜치로 넘어가기 전에, 해당 브랜치를 모두 탐색하는 방법
- 스택 or 재귀함수를 통해 구현한다.
- 주로 모든 경로를 방문해야 할 경우 사용에 적합하다.

<br/>

### DFS의 특징
- 루트 노드에서 시작해서 다음 분기로 넘어가기 전에 해당 분기를 완벽하게 탐색한다.
- 넓게 탐색하기 전에 깊게 탐색하는 것이다.
- 예를 들어, 미로를 탐색할 때 한 방향으로 갈 수 있을 때까지 계속 가다가 더 이상 갈 수 없게 되면 다시 가장 가까운 갈림길로 돌아와서 이곳으로부터 다른 방향으로 다시 탐색을 진행하는 방법과 유사하다.

<br/>

### DFS 과정
<img src="https://github.com/2dongyeop/TIL/blob/main/Algorithm/image/DFS.png" width = 500/>

<br/>

## BFS
- 너비 우선 탐색이라 하며 BFS(Breadth-First Search)라 부른다.
- 루트 노드 또는 임의 노드에서 인접한 노드부터 먼저 탐색하는 방법
- 해당 노드의 주변부터 탐색해야하기 때문에 큐를 통해 구현한다. 
- 최소 비용(즉, 모든 곳을 탐색하는 것보다 최소 비용이 우선일 때)에 적합

<br/>

### BFS의 특징
- 루트 노드 혹은 임의의 노드에서 시작해 인접한 노드를 먼저 탐색하는 방법.
- 시작 정점으로부터 가까운 정점을 먼저 방문하고 멀리 떨어져 있는 정점을 나중에 방문하는 순회 방법이다.
  - 즉, 깊게 탐색하기 전에 넓게 탐색하는 것이다.

<br/>

### BFS 특징

<img src="https://github.com/2dongyeop/TIL/blob/main/Algorithm/image/BFS.png" width = 500/>