# Dog Treats (Bronze 4)

### 문제 설명

Barley the dog loves treats. At the end of the day he is either happy or sad depending on the number and size of treats he receives throughout the day. The treats come in three sizes: small, medium, and large. His happiness score can be measured using the following formula:

1 × S + 2 × M + 3 × L

where S is the number of small treats, M is the number of medium treats and L is the number of large treats.

If Barley’s happiness score is 10 or greater then he is happy. Otherwise, he is sad. Determine whether Barley is happy or sad at the end of the day.

출처 : https://www.acmicpc.net/problem/19602

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int smallTreatCount = scanner.nextInt();
        int mediumTreatCount = scanner.nextInt();
        int largeTreatCount = scanner.nextInt();

        DogTreats dogTreats = new DogTreats(smallTreatCount, mediumTreatCount, largeTreatCount);
        Dog dog = new Dog(dogTreats);

        System.out.println(dog.emotion());
    }
}

class Dog {

    private static final String HAPPY_EMOTION = "happy";
    private static final String SAD_EMOTION = "sad";
    private static final int HAPPY_SCORE = 10;

    private final DogTreats dogTreats;

    public Dog(DogTreats dogTreats) {
        this.dogTreats = dogTreats;
    }

    public String emotion() {
        return isHappy() ? HAPPY_EMOTION : SAD_EMOTION;
    }

    private boolean isHappy() {
        return dogTreats.score() >= HAPPY_SCORE;
    }
}

class DogTreats {

    private static final int MEDIUM_TREAT_SCORE = 2;
    private static final int LARGE_TREAT_SCORE = 3;

    private final int smallTreatCount;
    private final int mediumTreatCount;
    private final int largeTreatCount;

    public DogTreats(int smallTreatCount, int mediumTreatCount, int largeTreatCount) {
        this.smallTreatCount = smallTreatCount;
        this.mediumTreatCount = mediumTreatCount;
        this.largeTreatCount = largeTreatCount;
    }

    public int score() {
        return smallTreatScore() + mediumTreatScore() + largeTreatScore();
    }

    private int smallTreatScore() {
        return smallTreatCount;
    }

    private int mediumTreatScore() {
        return mediumTreatCount * MEDIUM_TREAT_SCORE;
    }

    private int largeTreatScore() {
        return largeTreatCount * LARGE_TREAT_SCORE;
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
    @MethodSource("provideDogTreatsAndScore")
    @DisplayName("강아지 간식이 주어지면 간식 점수를 반환한다.")
    void dogTreatsReturnsTreatsScore(int smallTreatCount, int mediumTreatCount, int largeTreatCount, int expected) {
        DogTreats sut = new DogTreats(smallTreatCount, mediumTreatCount, largeTreatCount);

        int actual = sut.score();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideDogTreatsAndScore() {
        return Stream.of(
                Arguments.of(0, 0, 0, 0),
                Arguments.of(1, 1, 1, 6),
                Arguments.of(3, 1, 0, 5),
                Arguments.of(3, 2, 1, 10)
        );
    }

    @ParameterizedTest
    @MethodSource("provideDogTreats")
    @DisplayName("강아지 간식이 주어지면 강아지의 감정을 반환한다.")
    void dogReturnsEmotionGivenDogTreats(int smallTreatCount, int mediumTreatCount, int largeTreatCount, String expected) {
        DogTreats dogTreats = new DogTreats(smallTreatCount, mediumTreatCount, largeTreatCount);
        Dog sut = new Dog(dogTreats);

        String actual = sut.emotion();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideDogTreats() {
        return Stream.of(
                Arguments.of(0, 0, 0, "sad"),
                Arguments.of(1, 1, 1, "sad"),
                Arguments.of(3, 1, 0, "sad"),
                Arguments.of(3, 2, 1, "happy"),
                Arguments.of(10, 0, 0, "happy"),
                Arguments.of(10, 10, 10, "happy")
        );
    }
}

class Dog {

    private static final String HAPPY_EMOTION = "happy";
    private static final String SAD_EMOTION = "sad";
    private static final int HAPPY_SCORE = 10;

    private final DogTreats dogTreats;

    public Dog(DogTreats dogTreats) {
        this.dogTreats = dogTreats;
    }

    public String emotion() {
        return isHappy() ? HAPPY_EMOTION : SAD_EMOTION;
    }

    private boolean isHappy() {
        return dogTreats.score() >= HAPPY_SCORE;
    }
}

class DogTreats {

    private static final int MEDIUM_TREAT_SCORE = 2;
    private static final int LARGE_TREAT_SCORE = 3;

    private final int smallTreatCount;
    private final int mediumTreatCount;
    private final int largeTreatCount;

    public DogTreats(int smallTreatCount, int mediumTreatCount, int largeTreatCount) {
        this.smallTreatCount = smallTreatCount;
        this.mediumTreatCount = mediumTreatCount;
        this.largeTreatCount = largeTreatCount;
    }

    public int score() {
        return smallTreatScore() + mediumTreatScore() + largeTreatScore();
    }

    private int smallTreatScore() {
        return smallTreatCount;
    }

    private int mediumTreatScore() {
        return mediumTreatCount * MEDIUM_TREAT_SCORE;
    }

    private int largeTreatScore() {
        return largeTreatCount * LARGE_TREAT_SCORE;
    }
}
~~~
