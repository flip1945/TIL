# 칸토어 집합 (Silver 3)

### 문제

칸토어 집합은 0과 1사이의 실수로 이루어진 집합으로, 구간 [0, 1]에서 시작해서 각 구간을 3등분하여 가운데 구간을 반복적으로 제외하는 방식으로 만든다.

전체 집합이 유한이라고 가정하고, 다음과 같은 과정을 통해서 칸토어 집합의 근사를 만들어보자.

1. -가 3N개 있는 문자열에서 시작한다.

2. 문자열을 3등분 한 뒤, 가운데 문자열을 공백으로 바꾼다. 이렇게 하면, 선(문자열) 2개가 남는다.

3. 이제 각 선(문자열)을 3등분 하고, 가운데 문자열을 공백으로 바꾼다. 이 과정은 모든 선의 길이가 1일때 까지 계속 한다.

예를 들어, N=3인 경우, 길이가 27인 문자열로 시작한다.

~~~
---------------------------
~~~

여기서 가운데 문자열을 공백으로 바꾼다.

~~~
---------         ---------
~~~

남은 두 선의 가운데 문자열을 공백으로 바꾼다.

~~~
---   ---         ---   ---
~~~

한번 더

~~~
- -   - -         - -   - -
~~~

모든 선의 길이가 1이면 멈춘다. N이 주어졌을 때, 마지막 과정이 끝난 후 결과를 출력하는 프로그램을 작성하시오.

---

#### 입력

입력을 여러 줄로 이루어져 있다. 각 줄에 N이 주어진다. 파일의 끝에서 입력을 멈춘다. N은 0보다 크거나 같고, 12보다 작거나 같은 정수이다.

---

#### 출력

입력으로 주어진 N에 대해서, 해당하는 칸토어 집합의 근사를 출력한다.

출처 : https://www.acmicpc.net/problem/4779

---

### 문제풀이

이번 문제는 분할 정복 문제입니다.

가운데 자리의 문자를 재귀적으로 지워나가면 문제를 해결할 수 있습니다.

System.out.print로 문제를 풀었다가 시간 초과가 나서 StringBuilder로 바꾸고 나서 통과할 수 있었습니다.

---

#### 나의 풀이

~~~java
import java.io.*;
import java.util.stream.*;

public class Main {
    static boolean[] line;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String str;
        StringBuilder answer = new StringBuilder();
        while ((str = br.readLine()) != null) {
            int n = Integer.parseInt(str);
            int length = (int) Math.pow(3, n);
            line = new boolean[length];
            
            cantorSet(0, length, length);

            IntStream.range(0, length)
                    .mapToObj(i -> line[i] ? " ":"-")
                    .forEach(answer::append);
            answer.append("\n");
        }
        System.out.println(answer);
    }

    static void cantorSet(int start, int end, int length) {
        if (length == 1) {
            return;
        }
        int nextLength = length / 3;
        removeCenterSet(start, end, nextLength);
        cantorSet(start, start + nextLength, nextLength);
        cantorSet(start + nextLength, end - nextLength, nextLength);
        cantorSet(end - nextLength, end, nextLength);
    }

    static void removeCenterSet(int start, int end, int nextLength) {
        for (int i = start + nextLength; i < end - nextLength; i++) {
            line[i] = true;
        }
    }
}

~~~

---

#### 다른 사람의 풀이

https://www.acmicpc.net/source/43656178
