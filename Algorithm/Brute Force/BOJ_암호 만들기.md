# 암호 만들기 (Gold 5)

### 문제 설명

바로 어제 최백준 조교가 방 열쇠를 주머니에 넣은 채 깜빡하고 서울로 가 버리는 황당한 상황에 직면한 조교들은, 702호에 새로운 보안 시스템을 설치하기로 하였다. 이 보안 시스템은 열쇠가 아닌 암호로 동작하게 되어 있는 시스템이다.

암호는 서로 다른 L개의 알파벳 소문자들로 구성되며 최소 한 개의 모음(a, e, i, o, u)과 최소 두 개의 자음으로 구성되어 있다고 알려져 있다. 또한 정렬된 문자열을 선호하는 조교들의 성향으로 미루어 보아 암호를 이루는 알파벳이 암호에서 증가하는 순서로 배열되었을 것이라고 추측된다. 즉, abc는 가능성이 있는 암호이지만 bac는 그렇지 않다.

새 보안 시스템에서 조교들이 암호로 사용했을 법한 문자의 종류는 C가지가 있다고 한다. 이 알파벳을 입수한 민식, 영식 형제는 조교들의 방에 침투하기 위해 암호를 추측해 보려고 한다. C개의 문자들이 모두 주어졌을 때, 가능성 있는 암호들을 모두 구하는 프로그램을 작성하시오.

---

#### 입력

첫째 줄에 두 정수 L, C가 주어진다. (3 ≤ L ≤ C ≤ 15) 다음 줄에는 C개의 문자들이 공백으로 구분되어 주어진다. 주어지는 문자들은 알파벳 소문자이며, 중복되는 것은 없다.  

---

#### 출력

각 줄에 하나씩, 사전식으로 가능성 있는 암호를 모두 출력한다.

---
#### 예제 입력 1

~~~
4 6
a t c i s w
~~~

#### 예제 출력 1

~~~
acis
acit
aciw
acst
acsw
actw
aist
aisw
aitw
astw
cist
cisw
citw
istw
~~~

출처 : https://www.acmicpc.net/problem/1759

---

### 문제풀이

이번 문제는 완전 탐색 문제입니다.   

백트래킹을 사용해 문제의 정답을 구할 수 있습니다.

오름차순으로 정렬한 문자열에서 만들 수 있는 모든 조합을 구하고,

정답으로 출력할 수 있는지 확인합니다.

가능성 있는 암호라면 바로 출력하고 아니라면 무시합니다.

python에 내장돼있는 combinations 함수를 이용하면 더 쉽게 풀 수 있지만

저는 연습을 위해 직접 구현해서 문제를 풀었습니다.

---

#### 나의 풀이

~~~python
l, c = map(int, input().split())
char = sorted(list(input().split()))

def back_tracking(index, cur, answer):
    if index == l:
        if check_password(answer):
            print(''.join(answer))
        return

    for i in range(cur, c):
        answer.append(char[i])
        back_tracking(index + 1, i + 1,  answer)
        answer.pop()

def check_password(password):
    vowel = ['a', 'e', 'i', 'o', 'u']
    vowel_cnt, consonant_cnt = 0, 0
    for p in password:
        if p in vowel:
            vowel_cnt += 1
        else:
            consonant_cnt += 1
        if vowel_cnt > 0 and consonant_cnt > 1:
            return True
    return False

back_tracking(0, 0, [])
~~~

---

#### 다른 사람의 풀이

출처 : https://www.acmicpc.net/source/24102146

~~~python
from itertools import combinations
n, m=map(int,input().split())
l=input().split()
l.sort()
for i in list(map(''.join, combinations(l, n))):
	cnt=0
	for j in i:
		if j in 'aeiou':
			cnt+=1
	if cnt>0 and n-cnt>1:
		print(i)
~~~
