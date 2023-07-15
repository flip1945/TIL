# 학생 인기도 측정 (Silver 5)

### 문제 설명

학생 이름이 공백으로 구분된 문자열 A가 주어진다. 문자열 A에는 중복된 학생 이름이 존재하지 않는다. 학생 이름은 알파벳 소문자로 이루어져 있다. 각 학생이 좋아하는 학생의 학생 이름 목록이 공백으로 구분된 문자열로 주어진다. 각 학생이 좋아하는 학생은 1명 이상 주어지고, 내가 나를 좋아하는 예는 없다. 나를 좋아하는 학생이 많을수록 나의 인기도가 높다. 인기도가 높은 학생부터 낮은 학생 순으로 학생 이름과 해당 학생을 좋아하는 학생 수를 출력하자. 인기도가 같은 경우 학생 이름 기준으로 오름차순으로 출력하자.

출처 : https://www.acmicpc.net/problem/25325

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        String studentNames = scanner.nextLine();
        List<String> favoriteStudents = IntStream.range(0, n)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toList());

        Students students = new Students(studentNames);
        students.getSortedStudents(favoriteStudents).forEach(System.out::println);
    }
}

class Students {

    private final Map<String, Integer> popularity;

    public Students(String students) {
        this.popularity = initPopularity(students);
    }

    private Map<String, Integer> initPopularity(String students) {
        Map<String, Integer> popularity = new TreeMap<>();
        for (String student : students.split(" ")) {
            popularity.put(student, 0);
        }
        return popularity;
    }

    public List<String> getSortedStudents(List<String> favoriteStudents) {
        fillPopularity(favoriteStudents);
        return getSortedStudents();
    }

    private void fillPopularity(List<String> favoriteStudents) {
        for (String favoriteStudent : favoriteStudents) {
            for (String favorite : favoriteStudent.split(" ")) {
                popularity.merge(favorite, 1, Integer::sum);
            }
        }
    }

    private List<String> getSortedStudents() {
        return popularity.entrySet().stream()
                .sorted(Map.Entry.comparingByValue(Comparator.reverseOrder()))
                .map(entry -> entry.getKey() + " " + entry.getValue())
                .collect(Collectors.toList());
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

import java.util.*;
import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideStudentsAndFavoriteStudents")
    @DisplayName("인기도가 높은 학생부터 낮은 학생순으로 정렬한다. 인기도가 같다면 이름순으로 정렬한다.")
    void sortByPopularityAndNameTest(String students, List<String> favoriteStudents, List<String> expected) {
        Students sut = new Students(students);

        List<String> actual = sut.getSortedStudents(favoriteStudents);

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideStudentsAndFavoriteStudents() {
        return Stream.of(
                Arguments.of("a", List.of("a"), List.of("a 1")),
                Arguments.of("b", List.of("b"), List.of("b 1")),
                Arguments.of("a b", List.of("b"), List.of("b 1", "a 0")),
                Arguments.of("a b", List.of("b", "a b"), List.of("b 2", "a 1")),
                Arguments.of("a b c", List.of("a b c"), List.of("a 1", "b 1", "c 1")),
                Arguments.of("c b a", List.of("a b c"), List.of("a 1", "b 1", "c 1")),
                Arguments.of("c b a", List.of("c"), List.of("c 1", "a 0", "b 0")),
                Arguments.of("aaa bbb ccc ddd", List.of("bbb ddd", "aaa ddd", "aaa", "aaa bbb"), List.of("aaa 3", "bbb 2", "ddd 2", "ccc 0"))
        );
    }
}

class Students {

    private final Map<String, Integer> popularity;

    public Students(String students) {
        this.popularity = initPopularity(students);
    }

    private Map<String, Integer> initPopularity(String students) {
        Map<String, Integer> popularity = new TreeMap<>();
        for (String student : students.split(" ")) {
            popularity.put(student, 0);
        }
        return popularity;
    }

    public List<String> getSortedStudents(List<String> favoriteStudents) {
        fillPopularity(favoriteStudents);
        return getSortedStudents();
    }

    private void fillPopularity(List<String> favoriteStudents) {
        for (String favoriteStudent : favoriteStudents) {
            for (String favorite : favoriteStudent.split(" ")) {
                popularity.merge(favorite, 1, Integer::sum);
            }
        }
    }

    private List<String> getSortedStudents() {
        return popularity.entrySet().stream()
                .sorted(Map.Entry.comparingByValue(Comparator.reverseOrder()))
                .map(entry -> entry.getKey() + " " + entry.getValue())
                .collect(Collectors.toList());
    }
}
~~~
