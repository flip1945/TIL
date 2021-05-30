# 방문 길이(Level 2)

### 문제 설명

게임 캐릭터를 4가지 명령어를 통해 움직이려 합니다. 명령어는 다음과 같습니다.

* U: 위쪽으로 한 칸 가기

* D: 아래쪽으로 한 칸 가기

* R: 오른쪽으로 한 칸 가기

* L: 왼쪽으로 한 칸 가기

캐릭터는 좌표평면의 (0, 0) 위치에서 시작합니다. 좌표평면의 경계는 왼쪽 위(-5, 5), 왼쪽 아래(-5, -5), 오른쪽 위(5, 5), 오른쪽 아래(5, -5)로 이루어져 있습니다.   

<img src = "https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/ace0e7bc-9092-4b95-9bfb-3a55a2aa780e/%E1%84%87%E1%85%A1%E1%86%BC%E1%84%86%E1%85%AE%E1%86%AB%E1%84%80%E1%85%B5%E1%86%AF%E1%84%8B%E1%85%B51_qpp9l3.png">

예를 들어, "ULURRDLLU"로 명령했다면   

<img src = "https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/668c7458-e184-472d-9d32-f5d2acca759a/%E1%84%87%E1%85%A1%E1%86%BC%E1%84%86%E1%85%AE%E1%86%AB%E1%84%80%E1%85%B5%E1%86%AF%E1%84%8B%E1%85%B52_lezmdo.png">

* 1번 명령어부터 7번 명령어까지 다음과 같이 움직입니다.

<img src = "https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/08558e36-d667-4160-bfec-b754c78a7d85/%E1%84%87%E1%85%A1%E1%86%BC%E1%84%86%E1%85%AE%E1%86%AB%E1%84%80%E1%85%B5%E1%86%AF%E1%84%8B%E1%85%B53_sootjd.png">

* 8번 명령어부터 9번 명령어까지 다음과 같이 움직입니다.

<img src = "https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/a52af28e-5835-438b-9f40-5467ebf9bf03/%E1%84%87%E1%85%A1%E1%86%BC%E1%84%86%E1%85%AE%E1%86%AB%E1%84%80%E1%85%B5%E1%86%AF%E1%84%8B%E1%85%B54_hlpiej.png">

이때, 우리는 게임 캐릭터가 지나간 길 중 캐릭터가 처음 걸어본 길의 길이를 구하려고 합니다. 예를 들어 위의 예시에서 게임 캐릭터가 움직인 길이는 9이지만, 캐릭터가 처음 걸어본 길의 길이는 7이 됩니다. (8, 9번 명령어에서 움직인 길은 2, 3번 명령어에서 이미 거쳐 간 길입니다)   

단, 좌표평면의 경계를 넘어가는 명령어는 무시합니다.   

예를 들어, "LULLLLLLU"로 명령했다면   

<img src = "https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/f631f005-f8de-4392-a76c-a9ef64b6de08/%E1%84%87%E1%85%A1%E1%86%BC%E1%84%86%E1%85%AE%E1%86%AB%E1%84%80%E1%85%B5%E1%86%AF%E1%84%8B%E1%85%B55_nitjwj.png">

* 1번 명령어부터 6번 명령어대로 움직인 후, 7, 8번 명령어는 무시합니다. 다시 9번 명령어대로 움직입니다.

<img src = "https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/35e62f0a-43c6-4142-bec6-6d28fbc57216/%E1%84%87%E1%85%A1%E1%86%BC%E1%84%86%E1%85%AE%E1%86%AB%E1%84%80%E1%85%B5%E1%86%AF%E1%84%8B%E1%85%B56_nzhumd.png">

이때 캐릭터가 처음 걸어본 길의 길이는 7이 됩니다.   

명령어가 매개변수 dirs로 주어질 때, 게임 캐릭터가 처음 걸어본 길의 길이를 구하여 return 하는 solution 함수를 완성해 주세요.   

---

#### 제한사항

* dirs는 string형으로 주어지며, 'U', 'D', 'R', 'L' 이외에 문자는 주어지지 않습니다.

* dirs의 길이는 500 이하의 자연수입니다.

---

#### 입출력 예

|dirs|	answer|
|-|-|
|"ULURRDLLU"|	7|
|"LULLLLLLU"|	7|

#### 입출력 예 설명

##### 입출력 예 #1

* 문제 예시와 같습니다.

##### 입출력 예 #2

* 문제 예시와 같습니다.

[출처](https://programmers.co.kr/learn/courses/30/lessons/49994)

---

### 문제풀이

이 문제는 시뮬레이션 문제입니다.   

정해진 명령대로 이동시켜서 움직인 길의 개수를 정답으로 제출해야 합니다.    

이동한 점의 개수를 세는 것이 아니라 이동한 길의 개수를 알아야 하기 때문에 시작점의 좌표와 도착점의 좌표를 알아야 합니다.   

저는 딕셔너리를 만들어서 각 좌표에서 이동한 좌표의 정보를 저장하도록 했습니다.   

길에는 방향성이 없기 때문에 반대 방향으로 가는 것도 딕셔너리에 추가해야 합니다. 이렇게 해야 같은 길을 반대 방향에서 왔을 때 중복을 제거할 수 있습니다.   

그리고 좌표의 개수를 전부 더한 뒤 2로 나누면 정답이됩니다.   

---

#### 나의 풀이

~~~python
from collections import defaultdict

def solution(dirs):
    x = 0
    y = 0
    cmd = {"U": 0, "D": 1, "R": 2, "L": 3}
    way = [(0, 1), (0, -1), (1, 0), (-1, 0)]
    answer = defaultdict(set)
    
    for d in dirs:
        dx, dy = way[cmd[d]]
        # 좌표를 벗어나지 않는다면
        if -5 <= x + dx <= 5 and -5 <= y + dy <= 5:
            answer[f"{x} {y}"].add((x+dx, y+dy))
            answer[f"{x+dx} {y+dy}"].add((x, y))
            x, y = x+dx, y+dy
            
    return sum([len(ans) for ans in answer.values()]) // 2
~~~

---

#### 다른 사람의 풀이

~~~python
def solution(dirs):
    s = set()
    d = {'U': (0,1), 'D': (0, -1), 'R': (1, 0), 'L': (-1, 0)}
    x, y = 0, 0
    for i in dirs:
        nx, ny = x + d[i][0], y + d[i][1]
        if -5 <= nx <= 5 and -5 <= ny <= 5:
            s.add((x,y,nx,ny))
            s.add((nx,ny,x,y))
            x, y = nx, ny
    return len(s)//2
~~~
