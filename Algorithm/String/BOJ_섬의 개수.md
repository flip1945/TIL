# 파일 탐색기 (Gold 2)

### 문제

Windows의 파일 탐색기를 보면 파일이 정렬된 방식이 보통의 정렬 방식과는 다른 것을 알 수 있다.

보통 문자열을 정렬할 때는 맨 앞부터 한 글자씩 비교하다가 어느 한쪽이 끝나거나 일치하지 않는 글자가 있으면 그 위치의 문자를 비교한 결과가 문자열 전체를 비교한 결과가 된다. 한편 파일 탐색기는 여러 자리의 수를 한 글자로 취급해서 비교하는데, 이 때문에 "str12ing"과 "str123ing" 중 후자가 아니라 전자가 앞에 온다. 이러한 정렬 방식을 종종 "natural sort"라고 하기도 한다.

여러 개의 문자열이 주어지면 natural sort 방식으로 정렬한 결과를 출력하는 프로그램을 작성해 보자. 이 문제에서 구현할 알고리즘은 다음을 만족해야 한다.

1. 문자열은 알파벳 대소문자와 숫자로만 이루어져 있다.
2. 숫자열이 알파벳보다 앞에 오고, 알파벳끼리는 AaBbCc...XxYyZz의 순서를 따른다.
3. 문자열을 비교하는 중 숫자가 있을 경우 그 다음에 오는 숫자를 최대한 많이 묶어 한 단위로 비교한다. 예를 들어 "a12bc345"는 "a", "12", "b", "c", "345"의 다섯 단위로 이루어져 있다.
4. 숫자열끼리는 십진법으로 읽어서 더 작은 것이 앞에 온다. 이때 예제 2에서와 같이 값이 263을 초과할 수 있다.
5. 같은 값을 가지는 숫자열일 경우 앞에 따라붙는 0의 개수가 적은 것이 앞에 온다.

---

#### 입력

첫 줄에 문자열의 개수 N(2 ≤ N ≤ 10,000)이 주어진다. 그 다음 N줄에 정렬할 문자열이 한 줄에 하나씩 주어진다.

모든 문자열의 길이는 100 이하이며, 알파벳 대소문자와 숫자로만 이루어져 있다.

---

#### 출력

N줄에 걸쳐 문제에서 설명한 대로 문자열을 정렬한 결과를 출력한다.

출처 : https://www.acmicpc.net/problem/20210

---

### 문제풀이

이번 문제는 문자열 정렬 문제입니다.

특정한 조건에 따라 문자열을 정렬해야 하는 문제입니다.

문제 풀이 자체는 빨리 떠올릴 수 있었지만 시간 초과 라던가 문자열의 길이가 다를 때 어떻게 처리해야 하는 지를 인지하지 못해서 정답까지는 조금 오래 걸려습니다.

if elif 문 덩어리로 문제를 풀어서 조금 부끄럽긴 한데 다른 분들도 대부분 풀이가 길어서 이정도만 하고 넘어가야겠습니다.

---

#### 나의 풀이

~~~python
import re
from functools import cmp_to_key
s = [list(filter(None, re.split('(\d+|[a-zA-Z])', input()))) for _ in range(int(input()))]


def compare(x_list, y_list):
    for i in range(min(len(x_list), len(y_list))):
        x = x_list[i]
        y = y_list[i]
        if x.isdigit() and y.isdigit():
            if int(x) < int(y):
                return -1
            elif int(x) > int(y):
                return 1
            else:
                if len(x) < len(y):
                    return -1
                elif len(x) > len(y):
                    return 1
        elif x.isalpha() and y.isalpha():
            if ((x.isupper() and y.islower()) or (x.islower() and y.isupper())) and (x.upper() != y.upper()):
                if ord(x.upper()) < ord(y.upper()):
                    return -1
                elif ord(x.upper()) > ord(y.upper()):
                    return 1
            else:
                if ord(x) < ord(y):
                    return -1
                elif ord(x) > ord(y):
                    return 1
        else:
            if x.isdigit() and y.isalpha():
                return -1
            elif x.isalpha() and y.isdigit():
                return 1
    if len(x_list) < len(y_list):
        return -1
    elif len(x_list) > len(y_list):
        return 1
    return 0


for c in sorted(s, key=cmp_to_key(compare)):
    print("".join(c))

~~~
