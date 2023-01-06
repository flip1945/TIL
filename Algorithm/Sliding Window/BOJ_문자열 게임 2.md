# 문자열 게임 2 (Gold 5)

### 문제

작년에 이어 새로운 문자열 게임이 있다. 게임의 진행 방식은 아래와 같다.

1. 알파벳 소문자로 이루어진 문자열 W가 주어진다.
2. 양의 정수 K가 주어진다.
3. 어떤 문자를 정확히 K개를 포함하는 가장 짧은 연속 문자열의 길이를 구한다.
4. 어떤 문자를 정확히 K개를 포함하고, 문자열의 첫 번째와 마지막 글자가 해당 문자로 같은 가장 긴 연속 문자열의 길이를 구한다.

위와 같은 방식으로 게임을 T회 진행한다.

---

#### 입력

문자열 게임의 수 T가 주어진다. (1 ≤ T ≤ 100)

다음 줄부터 2개의 줄 동안 문자열 W와 정수 K가 주어진다. (1 ≤ K ≤ |W| ≤ 10,000) 

---

#### 출력

T개의 줄 동안 문자열 게임의 3번과 4번에서 구한 연속 문자열의 길이를 공백을 사이에 두고 출력한다.

만약 만족하는 연속 문자열이 없을 시 -1을 출력한다.

출처 : https://www.acmicpc.net/problem/20437

---

### 문제풀이

이번 문제는 슬라이딩 윈도우 문제입니다.

처음에 푼 풀이는 슬라이딩 윈도우 알고리즘을 사용하긴 했지만 시간 초과가 나서 문제 풀이법을 찾아봤습니다.

찾아본 결과 저는 모든 문자열을 탐색하면서 정답을 찾았지만 시간 초과가 나지 않기 위해서는 각 문자들이 등장하는 위치를 파악해서 문제를 풀어야 합니다.

자세한 설명은 해당 블로그(https://ansohxxn.github.io/boj/20437/)를 참고했습니다.

---

#### 나의 풀이

~~~python
for _ in range(int(input())):
    w = input()
    k = int(input())
    alpha = [[] for _ in range(26)]

    for i in range(len(w)):
        alpha[ord(w[i]) - 97].append(i)

    s = int(10**9)
    l = 0
    for positions in alpha:
        if len(positions) >= k:
            window_size = k - 1
            for j in range(len(positions) - window_size):
                s = min(positions[j+window_size] - positions[j] + 1, s)
                l = max(positions[j+window_size] - positions[j] + 1, l)

    if l == 0:
        print(-1)
        continue
    print(s, l)
~~~
