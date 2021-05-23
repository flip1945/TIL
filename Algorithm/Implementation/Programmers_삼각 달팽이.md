# 삼각 달팽이(Level 2)

### 문제 설명

정수 n이 매개변수로 주어집니다. 다음 그림과 같이 밑변의 길이와 높이가 n인 삼각형에서 맨 위 꼭짓점부터 반시계 방향으로 달팽이 채우기를 진행한 후, 첫 행부터 마지막 행까지 모두 순서대로 합친 새로운 배열을 return 하도록 solution 함수를 완성해주세요.   

</br>

<img src = "https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/e1e53b93-dcdf-446f-b47f-e8ec1292a5e0/examples.png" width = 800>

---

#### 제한사항

* n은 1 이상 1,000 이하입니다.

---

#### 입출력 예

|n|	result|
|-|-|
|4|	\[1,2,9,3,10,8,4,5,6,7]|
|5|	\[1,2,12,3,13,11,4,14,15,10,5,6,7,8,9]|
|6|	\[1,2,15,3,16,14,4,17,21,13,5,18,19,20,12,6,7,8,9,10,11]|

#### 입출력 예 설명

##### 입출력 예 #1

* 문제 예시와 같습니다.

##### 입출력 예 #2

* 문제 예시와 같습니다.

##### 입출력 예 #3

* 문제 예시와 같습니다.

[출처](https://programmers.co.kr/learn/courses/30/lessons/68645)

---

### 문제풀이

이 문제는 별다른 알고리즘을 사용하지 않는 구현문제입니다.   

삼각형을 테두리부터 채워나가서 안쪽까지 채우는 문제입니다.   

로직을 짜기가 까다로웠는데, 다음과 같이 구현했습니다.   

1. 삼각형의 테두리를 채우는 부분을 3가지로 나눠서 구성

2. 안쪽의 빈 부분을 새로운 삼각형으로 해서 

---

#### 나의 풀이

~~~python
def solution(n):
    answer = [[0] * i for i in range(1, n + 1)]
    end = sum([len(i) for i in answer]) + 1
    num = 1
    row = 0
    col = 0
    
    while num < end:
        
        if n == 1:
            answer[row][col] = num
            num += 1
            
        if num == end:
            break
        
        for i in range(n-1):
            if num == end:
                break
            answer[i + row][col] = num
            num += 1

        for i in range(n-1):
            if num == end:
                break
            answer[n+row-1][i + col] = num
            num += 1

        for i in range(n-1):
            if num == end:
                break
            answer[(n+row-1)-i][-(1+col)] = num
            num += 1
            
        n -= 3
        row += 2
        col += 1
    
    return [a for ans in answer for a in ans]
~~~

---

#### 다른 사람의 풀이

~~~python
def solution(n):
    dx=[0,1,-1];dy=[1,0,-1]
    b=[[0]*i for i in range(1,n+1)]
    x,y=0,0;num=1;d=0
    while num<=(n+1)*n//2:
        b[y][x]=num
        ny=y+dy[d];nx=x+dx[d];num+=1
        if 0<=ny<n and 0<=nx<=ny and b[ny][nx]==0:y,x=ny,nx
        else:d=(d+1)%3;y+=dy[d];x+=dx[d]
    return sum(b,[])
~~~

~~~python
from itertools import chain as SEX
def solution(n):
    [row, col, cnt] = [-1, 0, 1]
    board = [[None] * i for i in range(1, n+1)]
    for i in range(n):
        for _ in range(n-i):
            if i % 3 == 0:
                row += 1
            elif i % 3 == 1:
                col += 1
            else:
                row -= 1
                col -= 1
            board[row][col] = cnt
            cnt += 1
    return list(SEX(*board))
~~~
