# 아시아 정보올림피아드 (Silver 5)

### 문제 설명

최근 아시아 지역의 학생들만 참여하는 정보 올림피아드 대회가 만들어졌다. 이 대회는 온라인으로 치러지기 때문에 각 나라에서 이 대회에 참여하는 학생 수의 제한은 없다. 

참여한 학생들의 성적순서대로 세 명에게만 금, 은, 동메달을 수여한다. 단, 동점자는 없다고 가정한다. 그리고 나라별 메달 수는 최대 두 개다.

예를 들어, 대회 결과가 다음의 표와 같이 주어졌다고 하자.

|참가국|	학생번호|	점수|
|-|-|-|
|1|	1|	230|
|1|	2|	210|
|1|	3|	205|
|2|	1|	100|
|2|	2|	150|
|3|	1|	175|
|3|	2|	190|
|3|	3|	180|
|3|	4|	195|

이 경우, 금메달 수상자는 1번 국가의 1번 학생이고, 은메달 수상자는 1번 국가의 2번 학생이며, 동메달 수상자는 3번 국가의 4번 학생이다. (1번 국가의 3번 학생의 성적이 동메달 수여자보다 높지만, 나라 별 메달 수가 두 개 이하 이므로 1번 국가 3번 학생은 동메달을 받을 수 없다.)

대회 결과가 입력으로 주어질 때, 메달 수상자를 결정하여 출력하는 프로그램을 작성하시오.

출처 : https://www.acmicpc.net/problem/2535

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        List<String> testResult = IntStream.range(0, n)
            .mapToObj(i -> scanner.nextLine())
            .collect(Collectors.toList());
        AsiaOlympiadInInformatics olympiad = new AsiaOlympiadInInformatics();
        olympiad.getMedalists(testResult).forEach(System.out::println);
    }
}

class AsiaOlympiadInInformatics {
    private Student goldMedalist;
    private Student silverMedalist;

    public List<String> getMedalists(List<String> testResults) {
        List<Student> students = getSortedStudents(testResults);
        this.goldMedalist = students.get(0);
        this.silverMedalist = students.get(1);
        Student bronzeMedalist = getBronzeMedalist(students.subList(2, students.size()));
        return List.of(goldMedalist.toString(), silverMedalist.toString(), bronzeMedalist.toString());
    }

    private List<Student> getSortedStudents(List<String> testResults) {
        return testResults.stream()
            .map(Student::new)
            .sorted()
            .collect(Collectors.toList());
    }

    private Student getBronzeMedalist(List<Student> students) {
        Set<Integer> countryOfMedalists = getCountryOfMedalists();
        for (Student student : students) {
            if (!isSameCountryOfGoldAndSilverMedalist(countryOfMedalists, student.getCountry())) {
                return student;
            }
        }
        throw new IllegalStateException("a bronze medalist must exist");
    }

    private boolean isSameCountryOfGoldAndSilverMedalist(Set<Integer> countryOfMedalists, int country) {
        return countryOfMedalists.size() == 1 && countryOfMedalists.contains(country);
    }

    private Set<Integer> getCountryOfMedalists() {
        return new HashSet<>(List.of(goldMedalist.getCountry(), silverMedalist.getCountry()));
    }
}

class Student implements Comparable<Student> {
    private final int country;
    private final int id;
    private final int score;

    public Student(String testResult) {
        String[] splitResult = testResult.split(" ");
        this.country = Integer.parseInt(splitResult[0]);
        this.id = Integer.parseInt(splitResult[1]);
        this.score = Integer.parseInt(splitResult[2]);
    }

    public int getCountry() {
        return country;
    }

    @Override
    public String toString() {
        return country + " " + id;
    }

    @Override
    public int compareTo(Student o) {
        return Integer.compare(o.score, this.score);
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

import static org.junit.jupiter.api.Assertions.*;

import java.util.HashSet;
import java.util.List;
import java.util.Set;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class MainTest {

    @ParameterizedTest
    @MethodSource("provideResults")
    @DisplayName("메달리스트의 목록을 반환한다.")
    void it_returns_list_of_medalists(List<String> testResults, List<String> expected) {
        AsiaOlympiadInInformatics sut = new AsiaOlympiadInInformatics();
        
        List<String> actual = sut.getMedalists(testResults);
        
        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideResults() {
        return Stream.of(
            Arguments.of(List.of("1 1 1", "1 2 2", "2 1 3"), List.of("2 1", "1 2", "1 1")),
            Arguments.of(List.of("1 1 4", "1 2 2", "2 1 3"), List.of("1 1", "2 1", "1 2")),
            Arguments.of(List.of("1 1 4", "1 2 2", "2 1 3", "3 1 5"), List.of("3 1", "1 1", "2 1")),
            Arguments.of(List.of("1 1 4", "1 2 3", "1 3 5", "2 1 2"), List.of("1 3", "1 1", "2 1")),
            Arguments.of(List.of("1 1 230", "1 2 210", "1 3 205", "2 1 100", "2 2 150", "3 1 175", "3 2 190", "3 3 180", "3 4 195"), List.of("1 1", "1 2", "3 4"))
        );
    }
}

class AsiaOlympiadInInformatics {
    private Student goldMedalist;
    private Student silverMedalist;
    
    public List<String> getMedalists(List<String> testResults) {
        List<Student> students = getSortedStudents(testResults);
        this.goldMedalist = students.get(0);
        this.silverMedalist = students.get(1);
        Student bronzeMedalist = getBronzeMedalist(students.subList(2, students.size()));
        return List.of(goldMedalist.toString(), silverMedalist.toString(), bronzeMedalist.toString());
    }

    private List<Student> getSortedStudents(List<String> testResults) {
        return testResults.stream()
            .map(Student::new)
            .sorted()
            .collect(Collectors.toList());
    }

    private Student getBronzeMedalist(List<Student> students) {
        Set<Integer> countryOfMedalists = getCountryOfMedalists();
        for (Student student : students) {
            if (!isSameCountryOfGoldAndSilverMedalist(countryOfMedalists, student.getCountry())) {
                return student;
            }
        }
        throw new IllegalStateException("a bronze medalist must exist");
    }

    private boolean isSameCountryOfGoldAndSilverMedalist(Set<Integer> countryOfMedalists, int country) {
        return countryOfMedalists.size() == 1 && countryOfMedalists.contains(country);
    }

    private Set<Integer> getCountryOfMedalists() {
        return new HashSet<>(List.of(goldMedalist.getCountry(), silverMedalist.getCountry()));
    }
}

class Student implements Comparable<Student> {
    private final int country;
    private final int id;
    private final int score;

    public Student(String testResult) {
        String[] splitResult = testResult.split(" ");
        this.country = Integer.parseInt(splitResult[0]);
        this.id = Integer.parseInt(splitResult[1]);
        this.score = Integer.parseInt(splitResult[2]);
    }

    public int getCountry() {
        return country;
    }

    @Override
    public String toString() {
        return country + " " + id;
    }

    @Override
    public int compareTo(Student o) {
        return Integer.compare(o.score, this.score);
    }
}
~~~
