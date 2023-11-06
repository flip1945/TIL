# 컵라면 측정하기 (Bronze 3)

### 문제 설명

컵라면은 두 개의 밑면이 서로 평행하며, 원 모양인 원뿔대이다. 따라서 컵라면을 옆에서 본 모습은 아래 그림과 같은 등변사다리꼴이다.

<img src='https://upload.acmicpc.net/d64aada2-3953-4dd1-90aa-27eca6cfbb35/'>

위 등변사다리꼴에서 민수가 측정한 컵라면의 윗면의 지름은 D1, 아랫면의 지름은 D2이다. 민수가 아직 측정하지 않은 변의 길이는 K이다. 이때, (컵라면의 높이)2의 값을 알아내는 프로그램을 작성하시오. (단, 컵라면의 높이는 등변사다리꼴에서 평행한 두 변 사이의 거리로 정의한다.)

출처 : https://www.acmicpc.net/problem/16479

---

#### 풀이
~~~java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int leg = scanner.nextInt();
        int longBase = scanner.nextInt();
        int shortBase = scanner.nextInt();

        CupRamen cupRamen = new CupRamen(leg, longBase, shortBase);
        System.out.println(cupRamen.height());
    }
}

class CupRamen {

    private final int leg;
    private final int longBase;
    private final int shortBase;

    public CupRamen(int leg, int longBase, int shortBase) {
        this.leg = leg;
        this.longBase = longBase;
        this.shortBase = shortBase;
    }

    public double height() {
        return Math.pow(leg, 2) - Math.pow(getBaseOfTriangle(), 2);
    }

    private double getBaseOfTriangle() {
        return (longBase - shortBase) / 2.0;
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

import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideSides")
    @DisplayName("윗변, 아랫변, 빗변의 길이가 주어지면 컵라면의 높이를 구한다.")
    void cupRamenCalculatesHeightGivenSides(int leg, int longBase, int shortBase, double expected) {
        CupRamen sut = new CupRamen(leg, longBase, shortBase);

        double actual = sut.height();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideSides() {
        return Stream.of(
                Arguments.of(14, 12, 12, 196),
                Arguments.of(16, 12, 12, 256),
                Arguments.of(8, 9, 3, 55),
                Arguments.of(15, 13, 6, 212.75)
        );
    }
}

class CupRamen {

    private final int leg;
    private final int longBase;
    private final int shortBase;

    public CupRamen(int leg, int longBase, int shortBase) {
        this.leg = leg;
        this.longBase = longBase;
        this.shortBase = shortBase;
    }

    public double height() {
        return Math.pow(leg, 2) - Math.pow(getBaseOfTriangle(), 2);
    }

    private double getBaseOfTriangle() {
        return (longBase - shortBase) / 2.0;
    }
}
~~~
