본 문제는 프로그래머스에서 탐욕법으로 분류된 문제입니다. 하지만 완전탐색에 해당한다고 생각해 완전탐색으로 분류했습니다.

이 문제가 탐욕법(그리디 알고리즘)이 아닌 이유는 오른쪽이나 왼쪽으로 순차적으로 탐색하면 정답이 아니기 때문입니다.

가장 최선의 선택만 반복하면 되는 것이 아니라 오른쪽으로 탐색 했다가 다시 왼쪽으로 탐색해야 하는 케이스들이 존재하기 때문에 그리디 알고리즘으로는 해결할 수 없습니다.

심지어 **"ABBAAAAAB"** 케이스는 왼쪽을 먼저 탐색했다가 오른쪽으로 탐색해야 하는 케이스인데, 문제에서는 빠져있습니다.

저도 문제는 풀었지만 **"ABBAAAAAB"** 케이스는 염두해두지 않았기 때문에 완전히 풀게되면 왼쪽부터 탐색하는 방식을 추가해야 합니다.

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
