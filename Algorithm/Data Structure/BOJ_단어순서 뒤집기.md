# 단어순서 뒤집기 (Bronze 1)

### 문제

스페이스로 띄어쓰기 된 단어들의 리스트가 주어질때, 단어들을 반대 순서로 뒤집어라. 각 라인은 w개의 영단어로 이루어져 있으며, 총 L개의 알파벳을 가진다. 각 행은 알파벳과 스페이스로만 이루어져 있다. 단어 사이에는 하나의 스페이스만 들어간다.   

---

#### 입력

첫 행은 N이며, 전체 케이스의 개수이다.   

N개의 케이스들이 이어지는데, 각 케이스는 스페이스로 띄어진 단어들이다. 스페이스는 라인의 처음과 끝에는 나타나지 않는다. N과 L은 다음 범위를 가진다.   

* N = 5

* 1 ≤ L ≤ 25

---

#### 출력

각 케이스에 대해서, 케이스 번호가 x일때  "Case #x: " 를 출력한 후 그 후에 이어서 단어들을 반대 순서로 출력한다.

---

#### 예제 입력 1
~~~
3
this is a test
foobar
all your base
~~~

#### 예제 출력 1
~~~
Case #1: test a is this
Case #2: foobar
Case #3: base your all
~~~

[출처](https://www.acmicpc.net/problem/12605)

---

### 문제풀이

이 문제는 단어의 순서를 뒤집는 문제입니다.   

이 문제를 풀 때, 저는 java로 풀어서 stack을 이용했지만 굳이 stack을 사용하지 않고도 문제를 풀 수 있습니다.   

StringTokenizer를 이용해 단어를 분리하고 분리한 단어를 stack에 넣어서 FILO 구조로 출력할 수 있도록 구현했습니다.   

이렇게 하면 단어 순서를 뒤집어서 출력할 수 있습니다.   

---

#### 나의 풀이

~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        Stack<String> stack = new Stack<>();
        int n = Integer.parseInt(sc.nextLine());

        for (int i = 1; i <= n; i++) {
            StringTokenizer st = new StringTokenizer(sc.nextLine(), " ");
            StringBuilder sb = new StringBuilder();

            while (st.hasMoreTokens()) {
                stack.push(st.nextToken());
            }

            while (!stack.empty()) {
                sb.append(stack.pop() + " ");
            }

            System.out.println("Case #" + i + ": " + sb);
        }
    }
}
~~~
