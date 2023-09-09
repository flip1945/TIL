# 삼각형 외우기 (Bronze 4)

### 문제 설명

창영이는 삼각형의 종류를 잘 구분하지 못한다. 따라서 프로그램을 이용해 이를 외우려고 한다.

삼각형의 세 각을 입력받은 다음, 

* 세 각의 크기가 모두 60이면, Equilateral
* 세 각의 합이 180이고, 두 각이 같은 경우에는 Isosceles
* 세 각의 합이 180이고, 같은 각이 없는 경우에는 Scalene
* 세 각의 합이 180이 아닌 경우에는 Error

를 출력하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/10101

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Triangle triangle = new Triangle(scanner.nextInt(), scanner.nextInt(), scanner.nextInt());

        System.out.println(triangle.getType());
    }
}

class Triangle {

    private final int side1;
    private final int side2;
    private final int side3;

    public Triangle(int side1, int side2, int side3) {
        this.side1 = side1;
        this.side2 = side2;
        this.side3 = side3;
    }

    public String getType() {
        if (isTriangle()) {
            return getTriangleType();
        }
        return TriangleType.ERROR.getType();
    }

    private boolean isTriangle() {
        return side1 + side2 + side3 == 180;
    }

    private String getTriangleType() {
        if (isEquilateral()) {
            return TriangleType.EQUILATERAL.getType();
        } else if (isIsosceles()) {
            return TriangleType.ISOSCELES.getType();
        }
        return TriangleType.SCALENE.getType();
    }

    private boolean isEquilateral() {
        return side1 == 60 && side2 == 60 && side3 == 60;
    }

    private boolean isIsosceles() {
        return side1 == side2 || side1 == side3 || side2 == side3;
    }
}

enum TriangleType {
    EQUILATERAL("Equilateral"),
    ISOSCELES("Isosceles"),
    SCALENE("Scalene"),
    ERROR("Error");

    private final String type;

    TriangleType(String type) {
        this.type = type;
    }

    public String getType() {
        return type;
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

import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideSides")
    @DisplayName("삼각형형의 종류를 반환해야 한다.")
    void shouldReturnTypeOfTriangle(int side1, int side2, int side3, String expected) {
        Triangle sut = new Triangle(side1, side2, side3);

        String actual = sut.getType();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideSides() {
        return Stream.of(
                Arguments.of(61, 61, 61, "Error"),
                Arguments.of(10, 15, 155, "Scalene"),
                Arguments.of(50, 60, 70, "Scalene"),
                Arguments.of(60, 60, 60, "Equilateral"),
                Arguments.of(10, 160, 10, "Isosceles"),
                Arguments.of(11, 160, 10, "Error")
        );
    }
}

class Triangle {

    private final int side1;
    private final int side2;
    private final int side3;

    public Triangle(int side1, int side2, int side3) {
        this.side1 = side1;
        this.side2 = side2;
        this.side3 = side3;
    }

    public String getType() {
        if (isTriangle()) {
            return getTriangleType();
        }
        return TriangleType.ERROR.getType();
    }

    private boolean isTriangle() {
        return side1 + side2 + side3 == 180;
    }

    private String getTriangleType() {
        if (isEquilateral()) {
            return TriangleType.EQUILATERAL.getType();
        } else if (isIsosceles()) {
            return TriangleType.ISOSCELES.getType();
        }
        return TriangleType.SCALENE.getType();
    }

    private boolean isEquilateral() {
        return side1 == 60 && side2 == 60 && side3 == 60;
    }

    private boolean isIsosceles() {
        return side1 == side2 || side1 == side3 || side2 == side3;
    }
}

enum TriangleType {
    EQUILATERAL("Equilateral"),
    ISOSCELES("Isosceles"),
    SCALENE("Scalene"),
    ERROR("Error");

    private final String type;

    TriangleType(String type) {
        this.type = type;
    }

    public String getType() {
        return type;
    }
}
~~~
