~~~python
# 입력 조건 : n = 전체 학생 수, lost = 도난 당한 학생들의 리스트 (중복 X), reserve = 여벌을 가져온 학생들의 리스트
# 출력 조건 : 수업을 들을 수 있는 총 학생 수

def solution(n, lost, reserve):
    students = reserve_chk(lost_students(n, sorted(lost)), sorted(reserve))
    return sum(students[1:])


def lost_students(n, lost):
    return [0 if i in lost else 1 for i in range(n + 1)]


def self_reserve(students, reserve):
    _reserve = []
    for i in reserve:
        if students[i] == 0:
            students[i] = 1
            continue
        _reserve.append(i)
    return students, _reserve


def reserve_chk(students, reserve):
    students, reserve = self_reserve(students, reserve)

    for i in reserve:
        for j in [-1, 1]:
            if i + j >= len(students):
                continue
            if students[i + j] == 0:
                students[i + j] = 1
                break
    return students


if __name__ == "__main__":
    reserve_chk([1, 1, 0, 1, 0, 1], [1, 3, 5])
~~~
