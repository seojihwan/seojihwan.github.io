---
title: "BFS와 DFS with python"
date: 2020-05-14 12:00:00
category: 'Algorithm'
draft: false
---


### BFS (Breadth First Search)

- 시작 노드와의 거리에 따른 탐색방법

#### 구현

```python
from collections import deque

def bfs(graph, root):
    q = deque([root])
    visited = []
    while q:
        n = q.popleft()
        if n not in visited:
            visited.append(n)
            if n in graph.keys():
                q += sorted(list((graph[n] - set(visited))))
    return visited
```

list를 이용할경우 pop연산에 있어서 O(n)의 시간이 필요하기 때문에
(deque) double ended queue를 import하여 popleft를 사용하였다.
set연산에서 '-'가 지원되어 방문했던 노드를 제거할 수 있고,
최종적으로 방문한 순서대로 노드들이 visited라는 list에 저장된다.

### DFS (Depth First Search)

- 시작 노드에서 깊이에 따른 탐색방법

#### 구현

```python
def dfs(graph, root):
    q = deque([root])
    visited = []
    while q:
        n = q.pop()
        if n not in visited:
            visited.append(n)
            if n in graph.keys():
                q += sorted(list((graph[n] - set(visited))), reversed=True)
    return visited
```

bfs와 유사하지만, popleft가 아닌 pop을 통해, 깊이 탐색이 이루어진다.
pop연산은 마지막 원소를 제거하고 return하기 때문에, 큐에 노드를 추가할 때
거꾸로 추가를 해주면, 순차적인 깊이탐색이 이루어질 수 있다.
