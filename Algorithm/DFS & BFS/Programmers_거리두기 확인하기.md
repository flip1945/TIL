# 거리두기 확인하기 (Level 2)

### 문제 설명

개발자를 희망하는 죠르디가 카카오에 면접을 보러 왔습니다.   

코로나 바이러스 감염 예방을 위해 응시자들은 거리를 둬서 대기를 해야하는데 개발 직군 면접인 만큼   
아래와 같은 규칙으로 대기실에 거리를 두고 앉도록 안내하고 있습니다.   

> 1. 대기실은 5개이며, 각 대기실은 5x5 크기입니다.
> 2. 거리두기를 위하여 응시자들 끼리는 맨해튼 거리가 2 이하로 앉지 말아 주세요.
> 3. 단 응시자가 앉아있는 자리 사이가 파티션으로 막혀 있을 경우에는 허용합니다.

예를 들어,   

|<img src="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/8c056cac-ec8f-435c-a49a-8125df055c5e/PXP.png">|<img src="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/d611f66e-f9c4-4433-91ce-02887657fe7f/PX_XP.png">|<img src="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/ed707158-0511-457b-9e1a-7dbf34a776a5/PX_OP.png">|
|:-:|:-:|:-:|
|위 그림처럼 자리 사이에 파티션이 존재한다면 맨해튼 거리가 2여도 거리두기를 지킨 것입니다.	|위 그림처럼 파티션을 사이에 두고 앉은 경우도 거리두기를 지킨 것입니다.	|위 그림처럼 자리 사이가 맨해튼 거리 2이고 사이에 빈 테이블이 있는 경우는 거리두기를 지키지 않은 것입니다.|
|<img src="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/4c548421-1c32-4947-af9e-a45c61501bc4/P.png">|<img src="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/ce799a38-668a-4038-b32f-c515b8701262/O.png">|<img src="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/91e8f98b-baeb-4f81-8cb6-5bafebebdcc7/X.png">|
|응시자가 앉아있는 자리(P)를 의미합니다.	|빈 테이블(O)을 의미합니다.	|파티션(X)을 의미합니다.|

5개의 대기실을 본 죠르디는 각 대기실에서 응시자들이 거리두기를 잘 기키고 있는지 알고 싶어졌습니다. 자리에 앉아있는 응시자들의 정보와 대기실 구조를 대기실별로 담은 2차원 문자열 배열 places가 매개변수로 주어집니다. 각 대기실별로 거리두기를 지키고 있으면 1을, 한 명이라도 지키지 않고 있으면 0을 배열에 담아 return 하도록 solution 함수를 완성해 주세요.   

---

#### 제한사항

* places의 행 길이(대기실 개수) = 5
    * places의 각 행은 하나의 대기실 구조를 나타냅니다.

* places의 열 길이(대기실 세로 길이) = 5

* places의 원소는 P,O,X로 이루어진 문자열입니다.
    * places 원소의 길이(대기실 가로 길이) = 5
    * P는 응시자가 앉아있는 자리를 의미합니다.
    * O는 빈 테이블을 의미합니다.
    * X는 파티션을 의미합니다.

* 입력으로 주어지는 5개 대기실의 크기는 모두 5x5 입니다.

* return 값 형식
    * 1차원 정수 배열에 5개의 원소를 담아서 return 합니다.
    * places에 담겨 있는 5개 대기실의 순서대로, 거리두기 준수 여부를 차례대로 배열에 담습니다.
    * 각 대기실 별로 모든 응시자가 거리두기를 지키고 있으면 1을, 한 명이라도 지키지 않고 있으면 0을 담습니다.

---

#### 입출력 예

|places|	result|
|-|-|
|\[\["POOOP", "OXXOX", "OPXPX", "OOXOX", "POXXP"], \["POOPX", "OXPXP", "PXXXO", "OXXXO", "OOOPP"], \["PXOPX", "OXOXP", "OXPOX", "OXXOP", "PXPOX"], \["OOOXX", "XOOOX", "OOOXX", "OXOOX", "OOOOO"], \["PXPXP", "XPXPX", "PXPXP", "XPXPX", "PXPXP"]]|	\[1, 0, 1, 1, 1]|

#### 입출력 예 설명

##### 입출력 예#1

첫 번째 대기실

|No.|	0|	1|	2|	3|	4|
|-|-|-|-|-|-|
|0|	P|	O|	O|	O|	P|
|1|	O|	X|	X|	O|	X|
|2|	O|	P|	X|	P|	X|
|3|	O|	O|	X|	O|	X|
|4|	P|	O|	X|	X|	P|

* 모든 응시자가 거리두기를 지키고 있습니다.

##### 입출력 예#2

두 번째 대기실

|No.|	0|	1|	2|	3|	4|
|-|-|-|-|-|-|
|0|	P|	O|	O|	P|	X|
|1|	O|	X|	P|	X|	P|
|2|	P|	X|	X|	X|	O|
|3|	O|	X|	X|	X|	O|
|4|	O|	O|	O|	P|	P|

* (0, 0) 자리의 응시자와 (2, 0) 자리의 응시자가 거리두기를 지키고 있지 않습니다.

* (1, 2) 자리의 응시자와 (0, 3) 자리의 응시자가 거리두기를 지키고 있지 않습니다.

* (4, 3) 자리의 응시자와 (4, 4) 자리의 응시자가 거리두기를 지키고 있지 않습니다.

##### 입출력 예#3

세 번째 대기실

|No.|	0|	1|	2|	3|	4|
|-|-|-|-|-|-|
|0|	P|	X|	O|	P|	X|
|1|	O|	X|	O|	X|	P|
|2|	O|	X|	P|	O|	X|
|3|	O|	X|	X|	O|	P|
|4|	P|	X|	P|	O|	X|

* 모든 응시자가 거리두기를 지키고 있습니다.

##### 입출력 예#4

네 번째 대기실

|No.|	0|	1|	2|	3|	4|
|-|-|-|-|-|-|
|0|	O|	O|	O|	X|	X|
|1|	X|	O|	O|	O|	X|
|2|	O|	O|	O|	X|	X|
|3|	O|	X|	O|	O|	X|
|4|	O|	O|	O|	O|  O|

* 대기실에 응시자가 없으므로 거리두기를 지키고 있습니다.

##### 입출력 예#5

다섯 번째 대기실

|No.|	0|	1|	2|	3|	4|
|-|-|-|-|-|-|
|0|	P|	X|	P|	X|	P|
|1|	X|	P|	X|	P|	X|
|2|	P|	X|	P|	X|	P|
|3|	X|	P|	X|	P|	X|
|4|	P|	X|	P|	X|	P|

* 모든 응시자가 거리두기를 지키고 있습니다.

두 번째 대기실을 제외한 모든 대기실에서 거리두기가 지켜지고 있으므로, 배열 \[1, 0, 1, 1, 1]을 return 합니다.

[출처](https://programmers.co.kr/learn/courses/30/lessons/17687)

---

### 문제풀이

이 문제는 그래프 탐색 + 완전 탐색으로 풀 수 있는 문제입니다.   

탐색의 범위가 적기 때문에 모든 P 표시가 된 부분에서 bfs 탐색을 해서 거리두기를 위반한 적이 있는지 확인합니다.   

5 X 5 칸이기 때문에 모든 경우에서 탐색을 해도 풀리기 때문에 쉽게 풀었던 것 같습니다.

---

#### 나의 풀이

~~~python
from collections import deque

def solution(places):
    return [check_place(place) for place in places]

def check_place(place):
    for row in range(5):
        for col in range(5):
            if place[row][col] == "P" and bfs(place, row, col):
                return 0
    return 1

def bfs(place, row, col):
    queue = deque([(row, col, 0)])
    visited = [[False] * 5 for _ in range(5)]
    visited[row][col] = True
    while queue:
        x, y, cur = queue.popleft()

        for dx, dy in [(-1, 0), (1, 0), (0, -1), (0, 1)]:
            nx, ny = x + dx, y + dy

            if 0 <= nx < 5 and 0 <= ny < 5 and not visited[nx][ny] and place[nx][ny] != "X" and cur < 2:

                if place[nx][ny] == "P":
                    return True
                queue.append((nx, ny, cur + 1))
                visited[nx][ny] = True
    return False
~~~
