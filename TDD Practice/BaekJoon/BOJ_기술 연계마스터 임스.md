# 기술 연계마스터 임스 (Silver 5)

### 문제 설명

임스는 연계 기술을 사용하는 게임을 플레이 중에 있다. 연계 기술은 사전 기술과 본 기술의 두 개의 개별 기술을 순서대로 사용해야만 정상적으로 사용 가능한 기술을 말한다.

하나의 사전 기술은 하나의 본 기술과만 연계해서 사용할 수 있으며, 연계할 사전 기술 없이 본 기술을 사용했을 경우에는 게임의 스크립트가 꼬여서 이후 사용하는 기술들이 정상적으로 발동되지 않는다. 그렇지만 반드시 사전 기술을 사용한 직후에 본 기술을 사용할 필요는 없으며, 중간에 다른 기술을 사용하여도 연계는 정상적으로 이루어진다.

임스가 사용할 수 있는 기술에는 
$1$~
$9$, 
$L$, 
$R$, 
$S$, 
$K$가 있다. 
$1$~
$9$는 연계 없이 사용할 수 있는 기술이고, 
$L$은 
$R$의 사전 기술, 
$S$은 
$K$의 사전 기술이다.

임스가 정해진 순서대로 
$N$개의 기술을 사용할 때, 기술이 몇 번이나 정상적으로 발동하는지를 구해보자.

단, 연계 기술은 사전 기술과 본 기술 모두 정상적으로 발동되었을 때만 하나의 기술이 발동된 것으로 친다.

출처 : https://www.acmicpc.net/problem/25497

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        System.out.println(count(scanner.nextLine()));
    }

    private static int count(String skills) {
        int result = 0;
        Stack<Character> sk = new Stack<>();
        Stack<Character> lr = new Stack<>();
        for (Character skill : skills.toCharArray()) {
            if (Character.isDigit(skill)) {
                result++;
            } else if (skill == 'S') {
                sk.push(skill);
            } else if (skill == 'L') {
                lr.push(skill);
            } else if (skill == 'K') {
                if (sk.size() == 0) {
                    return result;
                } else if (sk.pop() == 'S') {
                    result++;
                }
            } else if (skill == 'R') {
                if (lr.size() == 0) {
                    return result;
                } else if (lr.pop() == 'L') {
                    result++;
                }
            }
        }
        return result;
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.Arguments;
import org.junit.jupiter.params.provider.MethodSource;

import java.util.Stack;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideSkills")
    @DisplayName("정상적으로 기술을 사용한 횟수를 반환한다.")
    void skillCountTest(String skills, int expected) {
        int actual = count(skills);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideSkills() {
        return Stream.of(
                Arguments.of("1", 1),
                Arguments.of("123", 3),
                Arguments.of("SK", 1),
                Arguments.of("S1K", 2),
                Arguments.of("KS", 0),
                Arguments.of("123K123S", 3),
                Arguments.of("S12K2", 4),
                Arguments.of("1LKR", 1),
                Arguments.of("1LR", 2),
                Arguments.of("SSKK", 2),
                Arguments.of("LSRK", 2)
        );
    }

    private int count(String skills) {
        int result = 0;
        Stack<Character> sk = new Stack<>();
        Stack<Character> lr = new Stack<>();
        for (Character skill : skills.toCharArray()) {
            if (Character.isDigit(skill)) {
                result++;
            } else if (skill == 'S') {
                sk.push(skill);
            } else if (skill == 'L') {
                lr.push(skill);
            } else if (skill == 'K') {
                if (sk.size() == 0) {
                    return result;
                } else if (sk.pop() == 'S') {
                    result++;
                }
            } else if (skill == 'R') {
                if (lr.size() == 0) {
                    return result;
                } else if (lr.pop() == 'L') {
                    result++;
                }
            }
        }
        return result;
    }
}
~~~
