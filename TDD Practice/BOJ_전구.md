# 전구 (Bronze 2)

### 문제 설명

N개의 전구가 있고 맨 왼쪽에 있는 전구를 첫 번째라고 하자. 전구의 상태는 두 가지가 있으며 이를 숫자로 표현한다.

1은 전구가 켜져 있는 상태를 의미하고, 0은 전구가 꺼져 있는 상태를 의미한다.

전구를 제어하는 명령어가 1번부터 4번까지 4개가 있다. 아래 표는 각 명령어에 대한 설명이다.

- 1번 명령어 [i x] $(1 \le i \le N, 0 \le x \le 1)$ i번째 전구의 상태를 x로 변경한다.
- 2번 명령어 [l r] $(1 \le l \le r \le N)$ l번째부터 r번째까지의 전구의 상태를 변경한다. (켜져있는 전구는 끄고, 꺼져있는 전구는 킨다.)
- 3번 명령어 [l r] $(1 \le l \le r \le N)$ l번째부터 r번째까지의 전구를 끈다.
- 4번 명령어 [l r] $(1 \le l \le r \le N)$ l번째부터 r번째까지의 전구를 킨다.

주어지는 명령어를 다 수행한 결과 전구는 어떤 상태인지 알아보자.

---

#### 입력

첫 번째 줄에 전구의 개수 $N$와 입력되는 명령어의 개수 $M$이 주어진다.

두 번째 줄에는 $N$개의 전구가 현재 어떤 상태 $s$인지 주어진다. ($0$은 꺼져있는 상태, $1$은 켜져있는 상태)

 $3$ 번째 줄부터 $M + 2$ 번째 줄까지 세 개의 정수 $a, b, c$가 들어온다.

 $a$는 $a$번째 명령어를 의미하고 $b, c$는 $a$가 1인 경우는 각각 $i, x$를 의미하고 $a$가 $2, 3, 4$ 중 하나면 각각 $l, r$을 의미한다.

---

#### 출력

모든 명령어를 수행한 후 전구가 어떤 상태인지 출력한다.

---

#### 예제 입력 1

~~~
8 3
0 0 0 0 0 0 0 0
1 2 1
1 4 1
2 2 4
~~~

#### 예제 출력 1

~~~
0 0 1 0 0 0 0 0
~~~

출처 : https://www.acmicpc.net/problem/21918

---

### 문제풀이

이 문제는 구현 문제입니다. 

간단한 구현 문제로 TDD를 사용해 문제를 풀었습니다.

---

#### 나의 풀이

##### 풀이
~~~java
import java.util.List;
import java.util.Scanner;
import java.util.stream.Collectors;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        List<Integer> nm = convertToIntList(List.of(scanner.nextLine().split(" ")));
        List<Integer> lamps = convertToIntList(List.of(scanner.nextLine().split(" ")));

        for (int i = 0; i < nm.get(1); i++) {
            List<Integer> commands = convertToIntList(List.of(scanner.nextLine().split(" ")));
            runCommand(lamps, commands.get(0), commands.get(1), commands.get(2));
        }

        lamps.forEach(lamp -> System.out.print(lamp + " "));
    }

    private static List<Integer> convertToIntList(List<String> strings) {
        return strings.stream().map(Integer::parseInt).collect(Collectors.toList());
    }

    private static void runCommand(List<Integer> lamps, int command, int firstArgument, int secondArgument) {
        switch (command) {
            case 1: runFirstCommand(lamps, firstArgument, secondArgument); break;
            case 2: runSecondCommand(lamps, firstArgument, secondArgument); break;
            case 3: runThirdCommand(lamps, firstArgument, secondArgument); break;
            case 4: runFourthCommand(lamps, firstArgument, secondArgument); break;
        }
    }

    private static void runFirstCommand(List<Integer> lamps, int i, int x) {
        lamps.set(getLampIndex(i), x);
    }

    private static void runSecondCommand(List<Integer> lamps, int l, int r) {
        for (int i = l; i <= r; i++) {
            lamps.set(getLampIndex(i), lamps.get(getLampIndex(i)) ^ 1);
        }
    }

    private static void runThirdCommand(List<Integer> lamps, int l, int r) {
        for (int i = l; i <= r; i++) {
            lamps.set(getLampIndex(i), 0);
        }
    }

    private static void runFourthCommand(List<Integer> lamps, int l, int r) {
        for (int i = l; i <= r; i++) {
            lamps.set(getLampIndex(i), 1);
        }
    }

    private static int getLampIndex(int index) {
        return index - 1;
    }
}
~~~

##### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.Arrays;
import java.util.List;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void firstCommandTest() {
        int i, x;

        i = 1;
        x = 1;
        List<Integer> lamps1 = Arrays.asList(0, 0, 0, 0);
        runFirstCommand(lamps1, i, x);
        assertEquals(List.of(1, 0, 0, 0), lamps1);

        i = 2;
        x = 1;
        List<Integer> lamps2 = Arrays.asList(0, 0, 0, 0);
        runFirstCommand(lamps2, i, x);
        assertEquals(List.of(0, 1, 0, 0), lamps2);

        i = 2;
        x = 1;
        List<Integer> lamps3 = Arrays.asList(1, 1, 1, 1);
        runFirstCommand(lamps3, i, x);
        assertEquals(List.of(1, 1, 1, 1), lamps3);

        i = 4;
        x = 0;
        List<Integer> lamps4 = Arrays.asList(1, 1, 1, 1);
        runFirstCommand(lamps4, i, x);
        assertEquals(List.of(1, 1, 1, 0), lamps4);
    }

    @Test
    void secondCommandTest() {
        int l, r;

        l = 1;
        r = 1;
        List<Integer> lamps1 = Arrays.asList(0, 0, 0, 0);
        runSecondCommand(lamps1, l, r);
        assertEquals(List.of(1, 0, 0, 0), lamps1);

        l = 1;
        r = 2;
        List<Integer> lamps2 = Arrays.asList(0, 0, 0, 0);
        runSecondCommand(lamps2, l, r);
        assertEquals(List.of(1, 1, 0, 0), lamps2);

        l = 2;
        r = 4;
        List<Integer> lamps3 = Arrays.asList(1, 1, 1, 1);
        runSecondCommand(lamps3, l, r);
        assertEquals(List.of(1, 0, 0, 0), lamps3);
    }

    @Test
    void thirdCommandTest() {
        int l, r;

        l = 1;
        r = 1;
        List<Integer> lamps1 = Arrays.asList(1, 0, 0, 0);
        runThirdCommand(lamps1, l, r);
        assertEquals(List.of(0, 0, 0, 0), lamps1);

        l = 1;
        r = 2;
        List<Integer> lamps2 = Arrays.asList(1, 1, 1, 1);
        runThirdCommand(lamps2, l, r);
        assertEquals(List.of(0, 0, 1, 1), lamps2);

        l = 2;
        r = 4;
        List<Integer> lamps3 = Arrays.asList(1, 1, 1, 1);
        runThirdCommand(lamps3, l, r);
        assertEquals(List.of(1, 0, 0, 0), lamps3);

        l = 1;
        r = 4;
        List<Integer> lamps4 = Arrays.asList(0, 0, 0, 0);
        runThirdCommand(lamps4, l, r);
        assertEquals(List.of(0, 0, 0, 0), lamps4);
    }

    @Test
    void fourthCommandTest() {
        int l, r;

        l = 1;
        r = 1;
        List<Integer> lamps1 = Arrays.asList(0, 0, 0, 0);
        runFourthCommand(lamps1, l, r);
        assertEquals(List.of(1, 0, 0, 0), lamps1);

        l = 1;
        r = 2;
        List<Integer> lamps2 = Arrays.asList(0, 0, 0, 0);
        runFourthCommand(lamps2, l, r);
        assertEquals(List.of(1, 1, 0, 0), lamps2);

        l = 2;
        r = 4;
        List<Integer> lamps3 = Arrays.asList(0, 0, 0, 0);
        runFourthCommand(lamps3, l, r);
        assertEquals(List.of(0, 1, 1, 1), lamps3);

        l = 1;
        r = 4;
        List<Integer> lamps4 = Arrays.asList(1, 1, 1, 1);
        runFourthCommand(lamps4, l, r);
        assertEquals(List.of(1, 1, 1, 1), lamps4);
    }

    private void runFirstCommand(List<Integer> lamps, int i, int x) {
        lamps.set(getLampIndex(i), x);
    }

    private void runSecondCommand(List<Integer> lamps, int l, int r) {
        for (int i = l; i <= r; i++) {
            lamps.set(getLampIndex(i), lamps.get(getLampIndex(i)) ^ 1);
        }
    }

    private void runThirdCommand(List<Integer> lamps, int l, int r) {
        for (int i = l; i <= r; i++) {
            lamps.set(getLampIndex(i), 0);
        }
    }

    private void runFourthCommand(List<Integer> lamps, int l, int r) {
        for (int i = l; i <= r; i++) {
            lamps.set(getLampIndex(i), 1);
        }
    }

    private int getLampIndex(int index) {
        return index - 1;
    }
}
~~~
