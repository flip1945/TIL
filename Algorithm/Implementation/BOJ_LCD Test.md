# LCD Test(Silver 2)

### 문제 설명

지민이는 새로운 컴퓨터를 샀다. 하지만 새로운 컴퓨터에 사은품으로 온 LC-디스플레이 모니터가 잘 안나오는 것이다. 지민이의 친한 친구인 지환이는 지민이의 새로운 모니터를 위해 테스트 할 수 있는 프로그램을 만들기로 하였다.   

---

#### 입력

첫째 줄에 두 개의 정수 s와 n이 들어온다. (1 ≤ s ≤ 10, 0 ≤ n ≤ 9,999,999,999)이다. n은 LCD 모니터에 나타내야 할 수 이며, s는 크기이다.       

---

#### 출력

길이가 s인 '-'와 '|'를 이용해서 출력해야 한다. 각 숫자는 모두 s+2의 가로와 2s+3의 세로로 이루어 진다. 나머지는 공백으로 채워야 한다. 각 숫자의 사이에는 공백이 한 칸 있어야 한다.

---
#### 예제 입력 1

~~~
2 1234567890
~~~

#### 예제 출력 1

~~~
      --   --        --   --   --   --   --   --  
   |    |    | |  | |    |       | |  | |  | |  | 
   |    |    | |  | |    |       | |  | |  | |  | 
      --   --   --   --   --        --   --       
   | |       |    |    | |  |    | |  |    | |  | 
   | |       |    |    | |  |    | |  |    | |  | 
      --   --        --   --        --   --   --  
~~~

[출처](https://www.acmicpc.net/problem/2290)

---

### 문제풀이

이번 문제는 구현 문제입니다.   

난이도는 쉬운 문제 였지만, 저는 푸는 방법을 오래 고민했던 문제였습니다.   

배열을 만들어서 쉽게 풀 수 있는 방법이 있을까 고민했는데, 너무 시간을 오래 쓰는 바람에 if문 여러개를 사용해서 문제를 풀게 됐습니다.   

문제를 풀고 다른 사람들의 풀이를 보니 다른 분들은 아래의 코드처럼 깔끔하게 풀었습니다.    

---

#### 나의 풀이

~~~python
s, n = input().split()
s = int(s)
lcd = [[] for i in range(s * 2 + 3)]

for i in range(len(lcd)):
    for num in n:
        if i == 0:
            if num in ["1", "4"]:
                lcd[i].append(' ' + ' ' * s + '  ')
            else:
                lcd[i].append(' ' + '-' * s + '  ')
        elif 0 < i < 1 + s:
            if num in ["1", "2", "3", "7"]:
                lcd[i].append(' ' * (s + 1) + '| ')
            elif num in ["5", "6"]:
                lcd[i].append('|' + ' ' * (s + 1) + ' ')
            else:
                lcd[i].append('|' + ' ' * s + '| ')
        elif i == 1 + s:
            if num in ["1", "7", "0"]:
                lcd[i].append(' ' + ' ' * s + '  ')
            else:
                lcd[i].append(' ' + '-' * s + '  ')
        elif 1 + s < i <= (s * 2) + 1:
            if num in ["1", "3", "4", "5", "7", "9"]:
                lcd[i].append(' ' * (s + 1) + '| ')
            elif num in ["2"]:
                lcd[i].append('|' + ' ' * (s + 1) + ' ')
            else:
                lcd[i].append('|' + ' ' * s + '| ')
        else:
            if num in ["1", "4", "7"]:
                lcd[i].append(' ' + ' ' * s + '  ')
            else:
                lcd[i].append(' ' + '-' * s + '  ')
    print("".join(lcd[i]))
~~~

#### 다른 사람의 풀이

~~~python
n,k = input().split()
n = int(n)

p = [
	"- -- -----",
	"|| | | |||| |  |||||",
	"  ----- --",
	"|| ||  | | ||| ||| |",
	"- -- -- --"
]

l = [[] for _ in '.....']

for u in k:
	u = int(u)
	for i in range(0, 5, 2):
		l[i].append(" %s " % (p[i][u] * n))
	for i in range(1, 5, 2):
		l[i].append("%s%s%s" % (p[i][u*2], " "*n, p[i][u*2+1]))
for i in range(5):
	l[i] = ' '.join(l[i])
print(l[0])
for i in range(n):
	print(l[1])
print(l[2])
for i in range(n):
	print(l[3])
print(l[4])
~~~
