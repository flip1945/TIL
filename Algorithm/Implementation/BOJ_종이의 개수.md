# 종이의 개수 (Silver 2)

### 문제

N×N크기의 행렬로 표현되는 종이가 있다. 종이의 각 칸에는 -1, 0, 1 중 하나가 저장되어 있다. 우리는 이 행렬을 다음과 같은 규칙에 따라 적절한 크기로 자르려고 한다.

1. 만약 종이가 모두 같은 수로 되어 있다면 이 종이를 그대로 사용한다.
2. (1)이 아닌 경우에는 종이를 같은 크기의 종이 9개로 자르고, 각각의 잘린 종이에 대해서 (1)의 과정을 반복한다.

이와 같이 종이를 잘랐을 때, -1로만 채워진 종이의 개수, 0으로만 채워진 종이의 개수, 1로만 채워진 종이의 개수를 구해내는 프로그램을 작성하시오.

---

#### 입력

첫째 줄에 N(1 ≤ N ≤ 37, N은 3k 꼴)이 주어진다. 다음 N개의 줄에는 N개의 정수로 행렬이 주어진다.

---

#### 출력

첫째 줄에 -1로만 채워진 종이의 개수를, 둘째 줄에 0으로만 채워진 종이의 개수를, 셋째 줄에 1로만 채워진 종이의 개수를 출력한다.

출처 : https://www.acmicpc.net/problem/1780

---

### 문제풀이

이번 문제는 구현 문제입니다.

이전에 비슷한 유형의 문제를 푼 적이 있는데, 기억이 나지 않아서 어렵게 풀었습니다.

전체 구역이 같은 숫자인지 확인하고 9가지 구역으로 다시 나눠서 재귀 호출을 하면 문제를 해결할 수 있습니다.

---

#### 나의 풀이

~~~python
import sys
input = sys.stdin.readline

n = int(input())
p = []
for _ in range(n):
    p.append(list(map(int, input().split())))
answer = {-1: 0, 0: 0, 1: 0}

def is_same_paper(paper):
    return len(set(i for line in paper for i in line)) == 1

def solution(current, current_size):
    if is_same_paper(current):
        answer[current[0][0]] += 1
        return

    size = current_size // 3
    for i in range(9):
        temp = []
        for j in range(size):
            temp.append(current[i // 3 * size + j][(i % 3 * size):(i % 3 * size)+size])
        solution(temp, size)

solution(p, n)
print(answer[-1])
print(answer[0])
print(answer[1])
~~~

#### 다른 사람의 풀이

https://www.acmicpc.net/source/30568800
