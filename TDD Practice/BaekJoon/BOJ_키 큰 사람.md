# 키 큰 사람 (Silver 5)

### 문제 설명

민우는 학창시절 승부욕이 강해서 달리기를 할 때에도 누가 가장 빠른지를 중요하게 생각하고, 시험을 볼 때에도 누가 가장 성적이 높은지를 중요하게 생각한다. 이번에 반에서 키를 측정하였는데, 민우는 마찬가지로 누구의 키가 가장 큰지 궁금해한다. 민우를 도와 가장 키가 큰 사람을 찾아보자.

출처 : https://www.acmicpc.net/problem/11292

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        while (n != 0) {
            People people = People.from(inputNamesAndHeights(scanner, n));
            System.out.println(people.getTallestPeople());

            n = Integer.parseInt(scanner.nextLine());
        }
    }

    private static List<String> inputNamesAndHeights(Scanner scanner, int n) {
        return IntStream.range(0, n)
                .mapToObj(i -> scanner.nextLine())
                .collect(Collectors.toList());
    }
}

class People {

    public static final String DELIMITER = " ";
    private final List<Person> people;

    private People(List<Person> people) {
        this.people = people;
    }

    public static People from(List<String> namesAndHeights) {
        return new People(initPeople(namesAndHeights));
    }

    private static List<Person> initPeople(List<String> namesAndHeights) {
        return namesAndHeights.stream()
                .map(Person::from)
                .collect(Collectors.toList());
    }

    public String getTallestPeople() {
        return people.stream()
                .filter(person -> getTallestHeight() == person.getHeight())
                .map(Person::getName)
                .collect(Collectors.joining(DELIMITER));
    }

    private double getTallestHeight() {
        return people.stream()
                .map(Person::getHeight)
                .max(Comparator.comparingDouble(x -> x))
                .orElseThrow();
    }
}

class Person {

    public static final String DELIMITER = " ";
    public static final int NAME_INDEX = 0;
    public static final int HEIGHT_INDEX = 1;

    private final String name;
    private final double height;

    private Person(String name, double height) {
        this.name = name;
        this.height = height;
    }

    public static Person from(String nameAndHeight) {
        String[] split = nameAndHeight.split(DELIMITER);
        return new Person(split[NAME_INDEX], Double.parseDouble(split[HEIGHT_INDEX]));
    }

    public String getName() {
        return name;
    }

    public double getHeight() {
        return height;
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

import java.util.Comparator;
import java.util.List;
import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideNameAndHeights")
    @DisplayName("가장 키가 큰 사람을 반환하고 키가 같은 경우 모두 반환한다.")
    void getTallestPeopleTest(List<String> namesAndHeights, String expected) {
        People sut = People.from(namesAndHeights);

        String actual = sut.getTallestPeople();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideNameAndHeights() {
        return Stream.of(
                Arguments.of(List.of("a 1.0"), "a"),
                Arguments.of(List.of("b 1.0"), "b"),
                Arguments.of(List.of("a 1.0", "b 1.2"), "b"),
                Arguments.of(List.of("a 1.0", "b 1.2", "c 1.3"), "c"),
                Arguments.of(List.of("a 1.3", "b 1.2", "c 1.3"), "a c"),
                Arguments.of(List.of("John 1.75", "Mary 1.64", "Sam 1.81"), "Sam"),
                Arguments.of(List.of("Jose 1.62", "Miguel 1.58"), "Jose"),
                Arguments.of(List.of("John 1.75", "Mary 1.75", "Sam 1.74", "Jose 1.75", "Miguel 1.75"), "John Mary Jose Miguel")
        );
    }
}

class People {

    public static final String DELIMITER = " ";
    private final List<Person> people;

    private People(List<Person> people) {
        this.people = people;
    }

    public static People from(List<String> namesAndHeights) {
        return new People(initPeople(namesAndHeights));
    }

    private static List<Person> initPeople(List<String> namesAndHeights) {
        return namesAndHeights.stream()
                .map(Person::from)
                .collect(Collectors.toList());
    }

    public String getTallestPeople() {
        return people.stream()
                .filter(person -> getTallestHeight() == person.getHeight())
                .map(Person::getName)
                .collect(Collectors.joining(DELIMITER));
    }

    private double getTallestHeight() {
        return people.stream()
                .map(Person::getHeight)
                .max(Comparator.comparingDouble(x -> x))
                .orElseThrow();
    }
}

class Person {

    public static final String DELIMITER = " ";
    public static final int NAME_INDEX = 0;
    public static final int HEIGHT_INDEX = 1;

    private final String name;
    private final double height;

    private Person(String name, double height) {
        this.name = name;
        this.height = height;
    }

    public static Person from(String nameAndHeight) {
        String[] split = nameAndHeight.split(DELIMITER);
        return new Person(split[NAME_INDEX], Double.parseDouble(split[HEIGHT_INDEX]));
    }

    public String getName() {
        return name;
    }

    public double getHeight() {
        return height;
    }
}
~~~
