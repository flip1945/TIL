# KMP는 왜 KMP일까 (Bronze 2)

### 문제 설명

KMP 알고리즘이 KMP인 이유는 이를 만든 사람의 성이 Knuth, Morris, Prett이기 때문이다. 이렇게 알고리즘에는 발견한 사람의 성을 따서 이름을 붙이는 경우가 많다.   

또 다른 예로, 유명한 비대칭 암호화 알고리즘 RSA는 이를 만든 사람의 이름이 Rivest, Shamir, Adleman이다.   

사람들은 이렇게 사람 성이 들어간 알고리즘을 두 가지 형태로 부른다.   

* 첫 번째는 성을 모두 쓰고, 이를 하이픈(-)으로 이어 붙인 것이다. 예를 들면, Knuth-Morris-Pratt이다. 이것을 긴 형태라고 부른다.

* 두 번째로 짧은 형태는 만든 사람의 성의 첫 글자만 따서 부르는 것이다. 예를 들면, KMP이다.

동혁이는 매일매일 자신이 한 일을 모두 메모장에 적어놓는다. 잠을 자기 전에, 오늘 하루 무엇을 했는지 되새겨 보는 것으로 하루를 마감한다.   

하루는 이 메모를 보던 중, 지금까지 긴 형태와 짧은 형태를 섞어서 적어 놓은 것을 발견했다.   

이렇게 긴 형태로 하루 일을 기록하다가는 메모장 가격이 부담되어 파산될 것이 뻔하기 때문에, 앞으로는 짧은 형태로 기록하려고 한다.   

긴 형태의 알고리즘 이름이 주어졌을 때, 이를 짧은 형태로 바꾸어 출력하는 프로그램을 작성하시오.

---

#### 입력

입력은 한 줄로 이루어져 있고, 최대 100글자의 영어 알파벳 대문자, 소문자, 그리고 하이픈 ('-', 아스키코드 45)로만 이루어져 있다. 첫 번째 글자는 항상 대문자이다. 그리고, 하이픈 뒤에는 반드시 대문자이다. 그 외의 모든 문자는 모두 소문자이다. 

---

#### 출력

첫 줄에 짧은 형태 이름을 출력한다.

---
#### 예제 입력 1

~~~
Knuth-Morris-Pratt
~~~

#### 예제 출력 1

~~~
KMP
~~~

#### 예제 입력 2

~~~
Mirko-Slavko
~~~

#### 예제 출력 2

~~~
MS
~~~

#### 예제 입력 3

~~~
Pasko-Patak
~~~

#### 예제 출력 3

~~~
PP
~~~

[출처](https://www.acmicpc.net/problem/2902)

---

### 문제풀이

이번 문제는 구현 문제입니다.   

-로 구분된 문자열이 입력으로 들어오는 데, 구분된 문자열의 첫번째 문자만 출력하면 정답을 맞출 수 있습니다.   

이 문제는 예외처리를 해줘야 하는 부분이 없어서 쉬웠습니다.   

예외처리를 하는 부분이 있다면 더 재밌는 문제가 됐을 것 같습니다.   

---

#### 나의 풀이

~~~python
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        StringTokenizer st = new StringTokenizer(sc.nextLine(), "-");
        StringBuilder sb = new StringBuilder();
		
        while (st.hasMoreTokens()) {
            sb.append(st.nextToken().substring(0, 1));
        }
        System.out.println(sb);
    }
}
~~~
