# 사과나무 (Gold 5)

### 문제 설명

이하는 최근 사과나무 씨앗을 구매하여 농장 뒷뜰에 일렬로 
$1$번부터 
$N$번까지 심었다. 이 나무들의 초기 높이는 모두 
$0$이다.

사과나무를 무럭무럭 키우기 위해 이하는 물뿌리개 
$2$개를 준비했다. 한 물뿌리개는 나무 하나를 
$1$만큼 성장시키고, 다른 물뿌리개는 나무 하나를 
$2$만큼 성장시킨다. 이 물뿌리개들은 동시에 사용해야 하며, 물뿌리개를 나무가 없는 토양에 사용할 수는 없다. 두 물뿌리개를 한 나무에 사용하여 
$3$만큼 키울 수도 있다.

물뿌리개 관리 시스템을 전부 프로그래밍한 이하는 이제 사과나무를 키워보려고 했다. 그 순간, 갊자가 놀러와서 각 사과나무의 높이가 이런 배치가 되었으면 좋겠다고 말했다. 이제 이하는 약간 걱정이 되기 시작했는데, 갊자가 알려준 사과나무의 배치를 이 프로그램 상으로 만들어내지 못할 수도 있기 때문이다.

이하는 이제 프로그램을 다시 수정하느라 바쁘기 때문에, 두 물뿌리개를 이용해 갊자가 알려준 사과나무의 배치를 만들 수 있는지의 여부를 판단하는 과정은 여러분의 몫이 되었다.

---

#### 입력

첫 번째 줄에는 자연수 
$N$이 주어진다. (1 <= N <= 100000) 이는 이하가 뒷뜰에 심은 사과나무의 개수를 뜻한다.

두 번째 줄에는 
$N$개의 정수 
$h_1,h_2,\cdots,h_N$이 공백으로 구분되어 주어진다. (0 <= $h_i$ <= 10000) 
$h_i$는 갊자가 바라는 
$i$번째 나무의 높이이다.

---

#### 출력

첫 번째 줄에 모든 나무가 갊자가 바라는 높이가 되도록 물뿌리개를 통해 만들 수 있으면 “YES”를, 아니면 “NO”를 따옴표를 제외하고 출력한다.

---

출처 : https://www.acmicpc.net/problem/19539

---

### 문제풀이

이번 문제는 그리디 문제입니다.

쉽게 풀이가 떠오르지 않았는데 풀고 나니 그렇게 어렵지는 않은 문제였습니다.

이 문제를 풀 때 주의해야 할 점이 있는데, 그 요소는 다음과 같습니다.

1. 매일매일 2개의 물뿌리개(1과 2의 물뿌리개)를 낭비 없이 사용해야 합니다. 이 때문에 사과나무의 총합이 **3의 배수**가 아니면 정답이 아니게 됩니다.
2. 물뿌리게를 사용하는 **전체 일수(사과나무의 총합 / 3)** 에서 2의 물뿌리개를 사용할 수 있는 일수가 중요합니다.
3. 예를 들어 2의 물뿌리개를 사용할 수 있는 일수가 4일이고 전체 일수가 4일보다 작거나 같다면 정답이지만 2의 물뿌리개를 사용할 수 있는 횟수가 전체 일수 보다 적다면 2의 물뿌리개를 처리할 수 없어는 날이 생겨서 정답이 될 수 없습니다.
4. 1의 물뿌리개는 중요하지 않은 게 2의 물뿌리개 횟수만 맞춘다면 자동으로 맞출 수 있기 때문에 따로 고려하지 않아도 됩니다.

---

#### 나의 풀이

~~~python
input()
h = list(map(int, input().split()))
s = sum(h)

two = 0
for i in h:
    two += i // 2

if s % 3 == 0:
    if two >= s // 3:
        print("YES")
    else:
        print("NO")
else:
    print("NO")

~~~
