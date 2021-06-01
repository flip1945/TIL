# \[3차] 방금그곡(Level 2)

### 문제 설명

라디오를 자주 듣는 네오는 라디오에서 방금 나왔던 음악이 무슨 음악인지 궁금해질 때가 많다. 그럴 때 네오는 다음 포털의 '방금그곡' 서비스를 이용하곤 한다. 방금그곡에서는 TV, 라디오 등에서 나온 음악에 관해 제목 등의 정보를 제공하는 서비스이다.   

네오는 자신이 기억한 멜로디를 가지고 방금그곡을 이용해 음악을 찾는다. 그런데 라디오 방송에서는 한 음악을 반복해서 재생할 때도 있어서 네오가 기억하고 있는 멜로디는 음악 끝부분과 처음 부분이 이어서 재생된 멜로디일 수도 있다. 반대로, 한 음악을 중간에 끊을 경우 원본 음악에는 네오가 기억한 멜로디가 들어있다 해도 그 곡이 네오가 들은 곡이 아닐 수도 있다. 그렇기 때문에 네오는 기억한 멜로디를 재생 시간과 제공된 악보를 직접 보면서 비교하려고 한다. 다음과 같은 가정을 할 때 네오가 찾으려는 음악의 제목을 구하여라.   

* 방금그곡 서비스에서는 음악 제목, 재생이 시작되고 끝난 시각, 악보를 제공한다.

* 네오가 기억한 멜로디와 악보에 사용되는 음은 C, C#, D, D#, E, F, F#, G, G#, A, A#, B 12개이다.

* 각 음은 1분에 1개씩 재생된다. 음악은 반드시 처음부터 재생되며 음악 길이보다 재생된 시간이 길 때는 음악이 끊김 없이 처음부터 반복해서 재생된다. 음악 길이보다 재생된 시간이 짧을 때는 처음부터 재생 시간만큼만 재생된다.

* 음악이 00:00를 넘겨서까지 재생되는 일은 없다.

* 조건이 일치하는 음악이 여러 개일 때에는 라디오에서 재생된 시간이 제일 긴 음악 제목을 반환한다. 재생된 시간도 같을 경우 먼저 입력된 음악 제목을 반환한다.

* 조건이 일치하는 음악이 없을 때에는 “(None)”을 반환한다.

---

#### 입력 형식

입력으로 네오가 기억한 멜로디를 담은 문자열 m과 방송된 곡의 정보를 담고 있는 배열 musicinfos가 주어진다.   

* m은 음 1개 이상 1439개 이하로 구성되어 있다.

* musicinfos는 100개 이하의 곡 정보를 담고 있는 배열로, 각각의 곡 정보는 음악이 시작한 시각, 끝난 시각, 음악 제목, 악보 정보가 ','로 구분된 문자열이다.
    * 음악의 시작 시각과 끝난 시각은 24시간 HH:MM 형식이다.
    * 음악 제목은 ',' 이외의 출력 가능한 문자로 표현된 길이 1 이상 64 이하의 문자열이다.
    * 악보 정보는 음 1개 이상 1439개 이하로 구성되어 있다.

#### 출력 형식

조건과 일치하는 음악 제목을 출력한다.

---

#### 입출력 예시

|m|	musicinfos|	answer|
|-|-|-|
|"ABCDEFG"|	\["12:00,12:14,HELLO,CDEFGAB", "13:00,13:05,WORLD,ABCDEF"]	|"HELLO"|
|"CC#BCC#BCC#BCC#B"|	\["03:00,03:30,FOO,CC#B", "04:00,04:08,BAR,CC#BCC#BCC#B"]	|"FOO"|
|"ABC"|	\["12:00,12:14,HELLO,C#DEFGAB", "13:00,13:05,WORLD,ABCDEF"]	|"WORLD"|

#### 설명

첫 번째 예시에서 HELLO는 길이가 7분이지만 12:00부터 12:14까지 재생되었으므로 실제로 CDEFGABCDEFGAB로 재생되었고, 이 중에 기억한 멜로디인 ABCDEFG가 들어있다.   

세 번째 예시에서 HELLO는 C#DEFGABC#DEFGAB로, WORLD는 ABCDE로 재생되었다. HELLO 안에 있는 ABC#은 기억한 멜로디인 ABC와 일치하지 않고, WORLD 안에 있는 ABC가 기억한 멜로디와 일치한다.   

[출처](https://programmers.co.kr/learn/courses/30/lessons/17683)

---

### 문제풀이

이 문제는 문자열 완전 탐색 문제입니다.   

부분 문자열이 전체 문자열 안에 포함되는 지 확인하는 문제입니다.   

그런데, 악보에는 # 이 있어서 # 부분을 따로 처리해 줘야 합니다.   

저는 # 부분을 처리하기 위해 모든 음을 리스트로 분리했습니다.   

리스트로 변경하면 비교가 어려울 수도 있지만 python에서 리스트 간의 비교도 지원하기 때문에 어렵지 않게 문제를 풀 수 있었습니다.   

다른 분들의 풀이를 보니 저처럼 리스트로 나누는 것보다는 # 이 붙은 문자를 소문자로 바꾸는 방식으로 푸신 분이 더 많았습니다.   

저도 그 방법이 더 깔끔하다고 생각해서 다음에는 그런 방식으로 문제를 풀어야 겠습니다.   

---

#### 나의 풀이

~~~python
def solution(m, musicinfos):
    answer = []
    m = split_music(m)
    # 입력 순서를 저장하기 위한 변수를 초기화
    cnt = 0
    # 모든 곡의 정보를 탐색
    for musicinfo in musicinfos:
        # 음악이 시작된 분, 종료된 분, 음악 제목, 악보를 초기화
        start, end, title, notes = musicinfo.split(",")
        # 음악이 재생된 분을 초기화
        time = get_music_min(start, end)
        # 악보를 음으로 쪼갠 리스트로 변환
        notes = split_music(notes)
        # 악보가 반복될 숫자를 정의
        n = time//len(notes) + 1
        # 반복 재생된 악보에서 재생된 시간까지만 잘라서 content에 넣음
        content = (notes*n)[:time]
        # 들은 멜로디가 악보에 포함되는지 탐색
        for i in range(len(content) - len(m) + 1):
            # 들은 멜로디가 현재 악보에 포함되었다면
            if m == content[i:i+len(m)]:
                cnt += 1
                # 결과에 (현재 곡의 제목, 입력된 순서, 제목)을 저장
                answer.append((time, cnt, title))
                break
    # 저장된 결과들을 주어진 조건에 따라 정렬
    answer.sort(key = lambda x : (x[0], -x[1]))
    # 가장 적합한 결과를 반환
    return answer[-1][-1] if answer else "(None)"
# 모든 음을 분리하는 함수
def split_music(music):
    result = []
    for i in range(len(music)-1):
        if music[i] == "#":
            continue
        elif music[i+1] == "#":
            result.append(music[i]+"#")
        else:
            result.append(music[i])
    return result + ([music[-1]] if music[-1] != "#" else [])
# 음악이 재생된 분을 구하는 함수
def get_music_min(start, end):
    start_min = int(start[:2]) * 60 + int(start[3:])
    end_min = int(end[:2]) * 60 + int(end[3:])
    return end_min - start_min
~~~

#### 다른 사람의 풀이

~~~python
def solution(skill, skill_trees):
    answer = 0

    for skills in skill_trees:
        skill_list = list(skill)

        for s in skills:
            if s in skill:
                if s != skill_list.pop(0):
                    break
        else:
            answer += 1

    return answer
~~~
