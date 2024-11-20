# 백준 2667(단지 번호 붙이기)
---
## 문제 설명
주어진 지도에서 1은 집이고, 0은 집이 없는 곳이다. 
지도를 가지고 연결된 집의 모임인 단지를 정의하고, 단지에 번호를 붙이려 한다. 여기서 연결되었다는 것은 어떤 집이 좌우, 혹은 아래위로 다른 집이 있는 경우를 말한다.

### 입력
첫 번째 줄에는 지도의 크기 N(정사각형이므로 가로와 세로의 크기는 같으며 5≤N≤25)이 입력되고, 그 다음 N줄에는 각각 N개의 자료(0혹은 1)가 입력된다.

### 출력
첫 번째 줄에는 총 단지수를 출력하시오. 그리고 각 단지내 집의 수를 오름차순으로 정렬하여 한 줄에 하나씩 출력하시오.

---
## 접근 방법
bfs를 이용하여 접근한 노드를 0으로 변경하고 가로세로를 확인하며 방문한다.

### 작성코드
```python
#그래프 이론(dfs or bfs)
from collections import deque

def bfs(graph,x,y):

    dx = [-1,1,0,0]
    dy = [0,0,-1,1]

    queue = deque()
    queue.append((x,y))
    graph[x][y] = 0  #탐색중인 위치 0으로 바꿔 다시 방문하지 않도록 함
    cnt = 1
    
    while queue:
        x,y = queue.popleft()

        for i in range(4):
            nx = x + dx[i]
            ny = y + dy[i]

            if nx < 0 or nx >= N or ny <0 or ny >= N:
                continue

            if graph[nx][ny] == 1:
                graph[nx][ny] = 0
                queue.append((nx,ny))
                cnt += 1
    return cnt
    


N = int(input())

graph = [list(map(int,input())) for _ in range(N)]

count = [bfs(graph, i, j) for i in range(N) for j in range(N) if graph[i][j] == 1]

count.sort()
print(len(count))

for i in range(len(count)):
    print(count[i])
```
