# 조이스틱 (Level 2)

### 문제 설명

조이스틱으로 알파벳 이름을 완성하세요. 맨 처음엔 A로만 이루어져 있습니다.
ex) 완성해야 하는 이름이 세 글자면 AAA, 네 글자면 AAAA

조이스틱을 각 방향으로 움직이면 아래와 같습니다.

~~~
▲ - 다음 알파벳
▼ - 이전 알파벳 (A에서 아래쪽으로 이동하면 Z로)
◀ - 커서를 왼쪽으로 이동 (첫 번째 위치에서 왼쪽으로 이동하면 마지막 문자에 커서)
▶ - 커서를 오른쪽으로 이동
~~~

예를 들어 아래의 방법으로 "JAZ"를 만들 수 있습니다.

~~~
- 첫 번째 위치에서 조이스틱을 위로 9번 조작하여 J를 완성합니다.
- 조이스틱을 왼쪽으로 1번 조작하여 커서를 마지막 문자 위치로 이동시킵니다.
- 마지막 위치에서 조이스틱을 아래로 1번 조작하여 Z를 완성합니다.
따라서 11번 이동시켜 "JAZ"를 만들 수 있고, 이때가 최소 이동입니다.
~~~

만들고자 하는 이름 name이 매개변수로 주어질 때, 이름에 대해 조이스틱 조작 횟수의 최솟값을 return 하도록 solution 함수를 만드세요.

---

#### 제한사항

* name은 알파벳 대문자로만 이루어져 있습니다.

* name의 길이는 1 이상 20 이하입니다.

---

#### 입출력 예

|name|	return|
|-|-|
|"JEROEN"|	56|
|"JAN"|	23|

출처 : https://programmers.co.kr/learn/courses/30/lessons/42860

---

### 문제 풀이

본 문제는 프로그래머스에서 탐욕법으로 분류된 문제입니다. 하지만 완전탐색에 해당한다고 생각해 완전탐색으로 분류했습니다.

이 문제가 탐욕법(그리디 알고리즘)이 아닌 이유는 오른쪽이나 왼쪽으로 순차적으로 탐색하면 정답이 아니기 때문입니다.

가장 최선의 선택만 반복하면 되는 것이 아니라 오른쪽으로 탐색 했다가 다시 왼쪽으로 탐색해야 하는 케이스들이 존재하기 때문에 그리디 알고리즘으로는 해결할 수 없습니다.

심지어 **"ABBAAAAAB"** 케이스는 왼쪽을 먼저 탐색했다가 오른쪽으로 탐색해야 하는 케이스인데, 문제에서는 빠져있습니다.

저도 문제는 풀었지만 **"ABBAAAAAB"** 케이스는 염두해두지 않았기 때문에 완전히 풀게되면 왼쪽부터 탐색하는 방식을 추가해야 합니다.

---

#### 나의 풀이

~~~python
def solution(name):
    name = list(name)
    min_result = int(1e9)

    for i in range(len(name)):
        result = 0
        s_list = ['A' for _ in range(len(name))]

        for j in range(i + 1):
            result += alphabet_count(name[j])
            s_list[j] = name[j]
            if s_list == name:
                min_result = min(result, min_result)
                break
            if j == i:
                continue
            result += 1

        if s_list == name:
            continue

        result += (i + 1)

        for k in range(len(name) - 1, i, -1):
            result += alphabet_count(name[k])
            s_list[k] = name[k]
            if s_list == name:
                min_result = min(result, min_result)
                break
            result += 1
    return min_result


def alphabet_count(character):
    return min(ord(character) - 65, 91 - ord(character))


if __name__ == "__main__":
    print(solution(list("JEROEN")))
    print(solution("JEROEN"))
    print(solution(list("BBA")))
~~~
