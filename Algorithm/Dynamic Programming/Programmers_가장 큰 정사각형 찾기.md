# 가장 큰 정사각형 찾기(Level 2)

### 문제 설명

1와 0로 채워진 표(board)가 있습니다. 표 1칸은 1 x 1 의 정사각형으로 이루어져 있습니다. 표에서 1로 이루어진 가장 큰 정사각형을 찾아 넓이를 return 하는 solution 함수를 완성해 주세요. (단, 정사각형이란 축에 평행한 정사각형을 말합니다.)   

예를 들어   

|1|	2|	3|	4|
|-|-|-|-|
|0|	1|	1|	1|
|1|	1|	1|	1|
|1|	1|	1|	1|
|0|	0|	1|	0|

가 있다면 가장 큰 정사각형은    

|1|	2|	3|	4|
|-|-|-|-|
|0|	`1`|	`1`|	`1`|
|1|	`1`|	`1`|	`1`|
|1|	`1`|	`1`|	`1`|
|0|	0|1|	0|

가 되며 넓이는 9가 되므로 9를 반환해 주면 됩니다.

---

#### 제한사항

* 표(board)는 2차원 배열로 주어집니다.

* 표(board)의 행(row)의 크기 : 1,000 이하의 자연수

* 표(board)의 열(column)의 크기 : 1,000 이하의 자연수

* 표(board)의 값은 1또는 0으로만 이루어져 있습니다.

---

#### 입출력 예

|board|	answer|
|-|-|
|\[\[0,1,1,1],\[1,1,1,1],\[1,1,1,1],\[0,0,1,0]]	9|
|\[\[0,0,1,1],\[1,1,1,1]]	4|

---

#### 입출력 예 설명

##### 입출력 예 #1

위의 예시와 같습니다.

##### 입출력 예 #2

| 0 | 0 | `1` | `1` |   
| 1 | 1 | `1` | `1` |   

로 가장 큰 정사각형의 넓이는 4가 되므로 4를 return합니다.   

---

### 문제풀이

이 문제는 2차원 배열 내에서 가장 큰 정사각형을 구하는 문제입니다.   

저는 완전 탐색으로 문제를 풀 수 있을 것이라고 생각해서 완전 탐색으로 풀었는데, 효율성 테스트 1개만 통과하고 나머지는 통과하지 못했습니다.   

다른 방법으로 풀어야 했는데, 방법이 떠오르지 않아서 질문하기에서 힌트를 보게됐습니다.   

정말 보고 싶진 않았지만 이대로 가면 시간이 너무 오래 걸릴것 같아서 힌트를 보니, Dynamic Programming으로 푸는 문제라는 것을 알게됐습니다.   

DP 문제라는 것을 알게 되니 어떻게 풀어야 하는지 떠올랐습니다.   

이 문제의 핵심은 현재 위치의 좌표의 사각형의 최대 크기는 왼쪽 대각선 위의 사각형 크기에서 1을 더한 값이 됩니다.   

자세한 설명은 아래 블로그에서 잘 설명해주셔서 블로그 주소를 첨부합니다.   

</br>

다른 사람의 해설 : https://minnnne.tistory.com/16

---

#### 나의 풀이

~~~python
def solution(board):
    dp = [[board[row][col] for col in range(len(board[0]))] for row in range(len(board))]

    for row in range(1, len(board)):
        for col in range(1, len(board[0])):
            if not dp[row][col]:
                continue
            dp[row][col] = min(dp[row-1][col-1] + 1, dp[row-1][col] + 1, dp[row][col-1] + 1)
            
    return max([max(d) for d in dp])**2
~~~

#### 다른 사람의 풀이

~~~python
def solution(board):
    answer = 0
    for i in range(1, len(board)) :
        for j in range(1, len(board[0])) :
            if board[i][j] >= 1 :
                board[i][j] += min(board[i-1][j-1],board[i][j-1],board[i-1][j])

    for i in board :
        if answer < max(i) : answer = max(i)
    return answer * answer
~~~
