# 다면체 (Bronze 3)

### 문제 설명

<img src='https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/upload/images2/poly.png'>

수학자가 구를 깎아서 볼록다면체를 만들었다. 이 수학자는 임의의 볼록다면체에 대해 (꼭짓점의 수) - (모서리의 수) + (면의 수) = 2가 성립한다는 것을 알고 있다. 그래서 구를 깎는 게 취미인 이 사람은 꼭짓점, 모서리와 면의 수를 기록할 때 꼭짓점과 모서리의 수만 세고 면의 수는 세지 않는다.

출처 : https://www.acmicpc.net/problem/10569

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        for (int i = 0; i < n; i++) {
            Polyhedron polyhedron = Polyhedron.of(scanner.nextInt(), scanner.nextInt());
            System.out.println(polyhedron.getCountOfFaces());
        }
    }
}

class Polyhedron {

    private final int countOfVertexes;
    private final int countOfEdges;
    private final int countOfFaces;

    public Polyhedron(int countOfVertexes, int countOfEdges) {
        this.countOfVertexes = countOfVertexes;
        this.countOfEdges = countOfEdges;
        this.countOfFaces = initCountOfFaces();
    }

    private int initCountOfFaces() {
        return 2 - countOfVertexes + countOfEdges;
    }

    public static Polyhedron of(int countOfVertexes, int countOfEdges) {
        return new Polyhedron(countOfVertexes, countOfEdges);
    }

    public int getCountOfFaces() {
        return countOfFaces;
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
    @MethodSource("provideVertexesAndEdges")
    @DisplayName("꼭지점의 개수와 모서리의 개수가 주어졌을 때 다면체 면의 개수를 반환한다.")
    void polyhedronTest(int vertex, int edge, int expected) {
        Polyhedron sut = Polyhedron.of(vertex, edge);

        int actual = sut.getCountOfFaces();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideVertexesAndEdges() {
        return Stream.of(
                Arguments.of(4, 6, 4),
                Arguments.of(8, 12, 6),
                Arguments.of(6, 12, 8),
                Arguments.of(20, 30, 12),
                Arguments.of(12, 30, 20)
        );
    }
}

class Polyhedron {

    private final int countOfVertexes;
    private final int countOfEdges;
    private final int countOfFaces;

    public Polyhedron(int countOfVertexes, int countOfEdges) {
        this.countOfVertexes = countOfVertexes;
        this.countOfEdges = countOfEdges;
        this.countOfFaces = initCountOfFaces();
    }

    private int initCountOfFaces() {
        return 2 - countOfVertexes + countOfEdges;
    }

    public static Polyhedron of(int countOfVertexes, int countOfEdges) {
        return new Polyhedron(countOfVertexes, countOfEdges);
    }

    public int getCountOfFaces() {
        return countOfFaces;
    }
}
~~~
