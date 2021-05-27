# \[1차] 프렌즈4블록(Level 2)

### 문제 설명

블라인드 공채를 통과한 신입 사원 라이언은 신규 게임 개발 업무를 맡게 되었다. 이번에 출시할 게임 제목은 "프렌즈4블록".   
같은 모양의 카카오프렌즈 블록이 2×2 형태로 4개가 붙어있을 경우 사라지면서 점수를 얻는 게임이다.   

<img src = "http://t1.kakaocdn.net/welcome2018/pang1.png">

만약 판이 위와 같이 주어질 경우, 라이언이 2×2로 배치된 7개 블록과 콘이 2×2로 배치된 4개 블록이 지워진다. 같은 블록은 여러 2×2에 포함될 수 있으며, 지워지는 조건에 만족하는 2×2 모양이 여러 개 있다면 한꺼번에 지워진다.   

<img src = "http://t1.kakaocdn.net/welcome2018/pang2.png">

블록이 지워진 후에 위에 있는 블록이 아래로 떨어져 빈 공간을 채우게 된다.   

<img src = "http://t1.kakaocdn.net/welcome2018/pang3.png">

만약 빈 공간을 채운 후에 다시 2×2 형태로 같은 모양의 블록이 모이면 다시 지워지고 떨어지고를 반복하게 된다.   

<img src = "http://t1.kakaocdn.net/welcome2018/pang4.png">

위 초기 배치를 문자로 표시하면 아래와 같다.   

~~~
TTTANT
RRFACC
RRRFCC
TRRRAA
TTMMMF
TMMTTJ
~~~

각 문자는 라이언(R), 무지(M), 어피치(A), 프로도(F), 네오(N), 튜브(T), 제이지(J), 콘(C)을 의미한다.   

입력으로 블록의 첫 배치가 주어졌을 때, 지워지는 블록은 모두 몇 개인지 판단하는 프로그램을 제작하라.   

---

#### 입력 형식

* 입력으로 판의 높이 m, 폭 n과 판의 배치 정보 board가 들어온다.

* 2 ≦ n, m ≦ 30

* board는 길이 n인 문자열 m개의 배열로 주어진다. 블록을 나타내는 문자는 대문자 A에서 Z가 사용된다.

#### 출력 형식

입력으로 주어진 판 정보를 가지고 몇 개의 블록이 지워질지 출력하라.   

---

#### 입출력 예제

|m|	n|	board|	answer|
|-|-|-|-|
|4|	5|	\["CCBDE", "AAADE", "AAABF", "CCBBF"]|	14|
|6|	6|	\["TTTANT", "RRFACC", "RRRFCC", "TRRRAA", "TTMMMF", "TMMTTJ"]|	15|

#### 예제에 대한 설명

* 입출력 예제 1의 경우, 첫 번째에는 A 블록 6개가 지워지고, 두 번째에는 B 블록 4개와 C 블록 4개가 지워져, 모두 14개의 블록이 지워진다.

* 입출력 예제 2는 본문 설명에 있는 그림을 옮긴 것이다. 11개와 4개의 블록이 차례로 지워지며, 모두 15개의 블록이 지워진다.  

[출처](https://programmers.co.kr/learn/courses/30/lessons/17679)

---

### 문제풀이

이번 문제는 퍼즐 게임과 비슷한 프로그램을 만드는 문제입니다.   

같은 모양이 2x2 형태로 모여있을 때, 그 모양들을 지워야합니다.   

지울 수 없을 때까지 모두 지우고, 지운 블록의 숫자가 몇 개인지 파악해야합니다.   

블록의 모양이 계속 변하고, 같은 모양이 있는 지 확인해야 하므로 완전 탐색을 사용해야 하는 것으로 보입니다.   

제한 사항을 보면 배열의 행, 열이 최대 30이하 이므로 완전 탐색으로 충분히 풀 수 있어보입니다.   

지운 블록이 자연스럽게 당겨지는 구조로 하기 위해서 행과 열을 뒤집어서 사용했습니다.   

그리고 지워야 하는 블록이 나오지 않을 때까지 완전 탐색을 반복해서 정답을 맞췄습니다.   

이 문제를 풀 때, 짧은 시간에 풀었는데, 한 번에 바로 통과해서 기분이 좋았습니다.   

한 달, 두 달 전에 저와 비교해서 실력이 늘었다는 것이 느껴지는 문제였습니다.   

---

#### 나의 풀이

~~~python
def solution(m, n, board):
    answer = 0
    # 행과 열을 뒤집어서 배열을 재설정
    board = list(map(list, zip(*reversed(board))))
    while True:
        # 지우기 리스트를 초기화
        del_list = [[] for _ in range(len(board))]
        for i in range(len(board)-1):
            for j in range(len(board[i])-1):
                cur = board[i][j]
                
                if j + 2 > len(board[i+1]):
                    continue
                
                # 지워야 하는 블록인 경우
                if cur == board[i][j+1] and cur == board[i+1][j] and cur == board[i+1][j+1]:
                    for k in range(2):
                        # 지워야 하는 블록을 지우기 리스트에 저장
                        del_list[i+k].append(j)
                        del_list[i+k].append(j+1)
        # 마크해 놓은 모양들을 제거
        for i in range(len(del_list)):
            for d in sorted(set(del_list[i]), reverse = True):
                board[i].pop(d)
                answer += 1
        # 지워야 하는 것이 없었다면 반복문을 종료 
        if not sum([len(d) for d in del_list]):
            break
        
    return answer
~~~
