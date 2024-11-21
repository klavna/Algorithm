# 집합

## 문제설명
비어있는 공집합 S가 주어졌을 때, 아래 연산을 수행하는 프로그램을 작성하시오.

add x: S에 x를 추가한다. (1 ≤ x ≤ 20) S에 x가 이미 있는 경우에는 연산을 무시한다.
remove x: S에서 x를 제거한다. (1 ≤ x ≤ 20) S에 x가 없는 경우에는 연산을 무시한다.
check x: S에 x가 있으면 1을, 없으면 0을 출력한다. (1 ≤ x ≤ 20)
toggle x: S에 x가 있으면 x를 제거하고, 없으면 x를 추가한다. (1 ≤ x ≤ 20)
all: S를 {1, 2, ..., 20} 으로 바꾼다.
empty: S를 공집합으로 바꾼다.

### 입력
첫째 줄에 수행해야 하는 연산의 수 M (1 ≤ M ≤ 3,000,000)이 주어진다.

둘째 줄부터 M개의 줄에 수행해야 하는 연산이 한 줄에 하나씩 주어진다

### 출력
check 연산이 주어질때마다, 결과를 출력한다.

## 문제풀이
### 접근법
빠른 연산을위하여 비트 연산으로 수행하였다

### 코드
```python
import sys

input = sys.stdin.readline

class Set:
    def __init__(self):
        self.S = [False] *21
    
    def add(self, x):
        self.S[x] = True

    def remove(self, x):
        self.S[x] = False
    
    def check(self, x):
        if self.S[x]:
            return 1
        return 0 
    
    def toggle(self, x):
        self.S[x] = not self.S[x]

    def all(self):
        self.S = [True] *21

    def empty(self):
        self.S = [False] *21

set = Set()
M = int(input())
result=[]
for i in range(M):
    line = input()
    if line[0:2] == 'al' or line[0:2] == 'em':
        com = line.strip()
    else:
        com, num = line.split()
        

    if com == 'add':
        set.add(int(num))
    elif com == 'remove':
        set.remove(int(num))
    elif com == 'check':
        print(set.check(int(num)))
    elif com == 'toggle':
        set.toggle(int(num))
    elif com == 'all':
        set.all()
    else:
        set.empty()
```
