# 그래프

## 개념 정의

<img src="https://github.com/2dongyeop/TIL/blob/main/Data-Structure/image/graph.png" width = 500/>

- 단순히 노드(N, node)와 그 노드를 연결하는 간선(E, edge)을 하나로 모아 놓은 자료 구조
  - 즉, 연결되어 있는 객체 간의 관계를 표현할 수 있는 자료 구조이다.
  - cf) 트리 또한 그래프이며, 그 중 사이클이 허용되지 않는 그래프를 말한다.


<br/>

###  그래프의 특징
- 그래프는 네트워크 모델이기에 2개 이상의 경로가 가능하다.
  - 즉, 노드들 사이에 무방향/방향에서 양방향 경로를 가질 수 있다.
- 루트 노드라는 개념과 부모-자식 관계라는 개념이 없다.
- 그래프의 순회(탐색)는 DFS나 BFS로 이루어진다.
- 그래프는 크게 방향 그래프와 무방향 그래프가 있다.


<br/>

## 그래프 관련 용어 정리
- ### 방향 그래프와 무방향 그래프
  - 방향 그래프(Directed Graph) : 간선에 방향성이 포함되어 있는 그래프
    ```
    V = {1, 2, 3, 4, 5, 6}
    E = {(1, 4), (2,1), (3, 4), (3, 4), (5, 6)}
    (u, v) = vertex u에서 vertex v로 가는 edge 
    ```

    <br/>

  - 무방향 그래프(Undirected Graph) : 말 그대로 정점과 간선의 연결관계에 있어서 방향성이 없는 그래프
    ```
    V = {1, 2, 3, 4, 5, 6}
    E = {(1, 4), (2,1), (3, 4), (3, 4), (5, 6)}
    (u, v) = vertex u와 vertex v를 연결하는 edge
    ```

    <br/>

  - Degree
    - 무방향 그래프에서 각 정점(Vertex)에 연결된 Edge 의 개수를 말한다.
    - 방향 그래프에서는 간선에 방향성이 존재하기 때문에 Degree 가 두 개로 나뉘게 된다. 
    - 각 정점으로부터 나가는 간선의 개수를 Outdegree 라 하고, 들어오는 간선의 개수를 Indegree 라 한다.

    <br/>

  - 가중치 그래프(Weight Graph)와 부분 그래프(Sub Graph)
    - 가중치 그래프 : 간선에 가중치 정보를 두어서 구성한 그래프
    - 비가중치 그래프 : 모든 간선의 가중치가 동일한 그래프
    - 부분 그래프 : 본래의 그래프의 일부 정점 및 간선으로 이루어진 그래프

<br/>

## 그래프를 구현하는 두 가지 방법
- ### 인접 행렬 (adjacent matrix) : 정방 행렬을 사용하는 방법
  
  <img src="https://github.com/2dongyeop/TIL/blob/main/Data-Structure/image/graph-matrix.png" width = 500/>

  - 해당하는 위치의 value 값을 통해서 vertex 간의 연결 관계를 O(1) 으로 파악할 수 있다. 
  - 밀집 그래프를 표현할 때 적절할 방법이다.

<br/>

- ### 인접 리스트 (adjacent list) : 연결 리스트를 사용하는 방법

  <img src="https://github.com/2dongyeop/TIL/blob/main/Data-Structure/image/graph-list.png" width = 500/>
  
  - vertex 의 adjacent list 를 확인해봐야 하므로 vertex 간 연결되어있는지 확인하는데 오래 걸린다. 
  - 희소 그래프를 표현하는데 적당한 방법이다.


<br/>

## 그래프 탐색 알고리즘
- 그래프는 정점의 구성 뿐만 아니라 간선의 연결에도 규칙이 존재하지 않기 때문에 탐색이 복잡하다. 
- 따라서 그래프의 모든 정점을 탐색하기 위한 방법은 다음의 두 가지 알고리즘을 기반으로 한다.

<br/>

- ### 깊이 우선 탐색 (Depth First Search: DFS)
  - 그래프 상에 존재하는 임의의 한 정점으로부터 연결되어 있는 한 정점으로만 나아간다라는 방법을 우선으로 탐색한다. 
  - 연결할 수 있는 정점이 있을 때까지 계속 연결한다.
  -  더이상 연결되지 않은 정점이 없으면 바로 그 전 단계의 정점으로 돌아간다.

<br/>

- ### 너비 우선 탐색 (Breadth First Search: BFS)
  - 그래프 상에 존재하는 임의의 한 정점으로부터 연결되어 있는 모든 정점으로 나아간다. 
  - Tree 에서의 Level Order Traversal 형식으로 진행되는 것이다. 
  - BFS 에서는 자료구조로 Queue 를 사용한다. 연락을 취할 정점의 순서를 기록하기 위한 것이다. 

<br/>

> 자세한 설명은 따로 정리한 [링크](https://github.com/2dongyeop/TIL/blob/main/Algorithm/DFS-BFS.md)를 첨부한다.


<br/>

## 최소 비용 알고리즘
- ### Kruskal Algorithm
  - 초기화 작업으로 edge 없이 vertex 들만으로 그래프를 구성한다. 
  - 그리고 weight 가 제일 작은 edge 부터 검토한다. 
    - 그러기 위해선 Edge Set 을 오름차순으로 정렬해야 한다. 
  - 그리고 가장 작은 weight 에 해당하는 edge 를 추가하는데 추가할 때 그래프에 cycle 이 생기지 않는 경우에만 추가한다. 

<br/>

- ### Prim Algorithm
  - 초기화 과정에서 한 개의 vertex 로 이루어진 초기 그래프 A 를 구성한다. 
  - 그리고 그래프 A 내부에 있는 vertex 로부터 외부에 있는 vertex 사이의 edge 를 연결하는데 그 중 가장 작은 weight 의 edge 를 통해 연결되는 vertex 를 추가한다. 
  - 어떤 vertex 건 간에 상관없이 edge 의 weight 를 기준으로 연결하는 것이다. 
  - 이렇게 연결된 vertex 는 그래프 A 에 포함된다. 