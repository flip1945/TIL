# 스위치 켜고 끄기 (Silver 4)

### 문제 설명

1부터 연속적으로 번호가 붙어있는 스위치들이 있다. 스위치는 켜져 있거나 꺼져있는 상태이다. <그림 1>에 스위치 8개의 상태가 표시되어 있다. ‘1’은 스위치가 켜져 있음을, ‘0’은 꺼져 있음을 나타낸다. 그리고 학생 몇 명을 뽑아서, 학생들에게 1 이상이고 스위치 개수 이하인 자연수를 하나씩 나누어주었다. 학생들은 자신의 성별과 받은 수에 따라 아래와 같은 방식으로 스위치를 조작하게 된다.

남학생은 스위치 번호가 자기가 받은 수의 배수이면, 그 스위치의 상태를 바꾼다. 즉, 스위치가 켜져 있으면 끄고, 꺼져 있으면 켠다. <그림 1>과 같은 상태에서 남학생이 3을 받았다면, 이 학생은 <그림 2>와 같이 3번, 6번 스위치의 상태를 바꾼다.

여학생은 자기가 받은 수와 같은 번호가 붙은 스위치를 중심으로 좌우가 대칭이면서 가장 많은 스위치를 포함하는 구간을 찾아서, 그 구간에 속한 스위치의 상태를 모두 바꾼다. 이때 구간에 속한 스위치 개수는 항상 홀수가 된다.

스위치 번호	①	②	③	④	⑤	⑥	⑦	⑧
스위치 상태	0	1	0	1	0	0	0	1
<그림 1>

예를 들어 <그림 2>에서 여학생이 3을 받았다면, 3번 스위치를 중심으로 2번, 4번 스위치의 상태가 같고 1번, 5번 스위치의 상태가 같으므로, <그림 3>과 같이 1번부터 5번까지 스위치의 상태를 모두 바꾼다. 만약 <그림 2>에서 여학생이 4를 받았다면, 3번, 5번 스위치의 상태가 서로 다르므로 4번 스위치의 상태만 바꾼다.

스위치 번호	①	②	③	④	⑤	⑥	⑦	⑧
스위치 상태	0	1	1	1	0	1	0	1
<그림 2>

스위치 번호	①	②	③	④	⑤	⑥	⑦	⑧
스위치 상태	1	0	0	0	1	1	0	1
<그림 3>

입력으로 스위치들의 처음 상태가 주어지고, 각 학생의 성별과 받은 수가 주어진다. 학생들은 입력되는 순서대로 자기의 성별과 받은 수에 따라 스위치의 상태를 바꾸었을 때, 스위치들의 마지막 상태를 출력하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/1244

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.IntStream;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        int[] lights = IntStream.range(0, n)
                .map(i -> scanner.nextInt())
                .toArray();

        int countOfStudents = scanner.nextInt();
        for (int i = 0; i < countOfStudents; i++) {
            int gender = scanner.nextInt();
            int number = scanner.nextInt();

            if (gender == 1) {
                executeMaleStudentCommand(lights, number);
            } else {
                executeFemaleStudentCommand(lights, number);
            }
        }

        for (int i = 0; i < n; i++) {
            if (i != 0 && i % 20 == 0) {
                System.out.println();
            }
            System.out.print(lights[i] + " ");
        }
    }

    static void executeMaleStudentCommand(int[] lights, int number) {
        for (int i = number - 1; i < lights.length; i += number) {
            lights[i] = lights[i] ^ 1;
        }
    }

    static void executeFemaleStudentCommand(int[] lights, int number) {
        int start = number - 1;
        int end = number - 1;
        while (start > 0 && end < lights.length - 1) {
            start--;
            end++;
            if (lights[start] != lights[end]) {
                start++;
                end--;
                break;
            }
        }

        for (int i = start; i <= end; i++) {
            lights[i] = lights[i] ^ 1;
        }
    }
}


~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.*;

public class MainTest {

    @Test
    void executeMaleStudentCommandTest() {
        int[] lights1 = new int[]{0, 0, 0};
        executeMaleStudentCommand(lights1, 1);
        assertArrayEquals(new int[]{1, 1, 1}, lights1);

        int[] lights2 = new int[]{1, 1 ,1};
        executeMaleStudentCommand(lights2, 1);
        assertArrayEquals(new int[]{0, 0, 0}, lights2);

        int[] lights3 = new int[]{1, 1, 1, 1};
        executeMaleStudentCommand(lights3, 2);
        assertArrayEquals(new int[]{1, 0, 1, 0}, lights3);

        int[] lights4 = new int[]{1, 1, 1, 1, 1, 1, 1};
        executeMaleStudentCommand(lights4, 3);
        assertArrayEquals(new int[]{1, 1, 0, 1, 1, 0, 1}, lights4);

        int[] lights5 = new int[]{0, 0, 0, 0};
        executeMaleStudentCommand(lights5, 1);
        assertArrayEquals(new int[]{1, 1, 1, 1}, lights5);

        int[] lights6 = new int[]{0, 1, 1, 1};
        executeMaleStudentCommand(lights6, 2);
        assertArrayEquals(new int[]{0, 0, 1, 0}, lights6);
    }

    @Test
    void executeFemaleStudentCommandTest() {
        int[] lights1 = new int[]{0, 0, 0};
        executeFemaleStudentCommand(lights1, 2);
        assertArrayEquals(new int[]{1, 1, 1}, lights1);

        int[] lights2 = new int[]{0, 0, 0};
        executeFemaleStudentCommand(lights2, 3);
        assertArrayEquals(new int[]{0, 0, 1}, lights2);

        int[] lights3 = new int[]{0, 0, 0};
        executeFemaleStudentCommand(lights3, 1);
        assertArrayEquals(new int[]{1, 0, 0}, lights3);

        int[] lights4 = new int[]{1, 0, 1, 0};
        executeFemaleStudentCommand(lights4, 2);
        assertArrayEquals(new int[]{0, 1, 0, 0}, lights4);

        int[] lights5 = new int[]{1, 1, 1, 1};
        executeFemaleStudentCommand(lights5, 1);
        assertArrayEquals(new int[]{0, 1, 1, 1}, lights5);

        int[] lights6 = new int[]{0, 0, 1, 0};
        executeFemaleStudentCommand(lights6, 2);
        assertArrayEquals(new int[]{0, 1, 1, 0}, lights6);
    }

    private void executeMaleStudentCommand(int[] lights, int number) {
        for (int i = number - 1; i < lights.length; i += number) {
            lights[i] = lights[i] ^ 1;
        }
    }

    private void executeFemaleStudentCommand(int[] lights, int number) {
        int start = number - 1;
        int end = number - 1;
        while (start > 0 && end < lights.length - 1) {
            start--;
            end++;
            if (lights[start] != lights[end]) {
                start++;
                end--;
                break;
            }
        }

        for (int i = start; i <= end; i++) {
            lights[i] = lights[i] ^ 1;
        }
    }
}
~~~
