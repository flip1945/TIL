# 행복한지 슬픈지 (Bronze 1)

### 문제 설명

승엽이는 자신의 감정을 표현하기 위해서 종종 문자 메시지에 이모티콘을 넣어 보내곤 한다. 승엽이가 보내는 이모티콘은 세 개의 문자가 붙어있는 구조로 이루어져 있으며, 행복한 얼굴을 나타내는 :-) 와 슬픈 얼굴을 나타내는 :-( 가 있다.

혜성이는 승엽이의 이모티콘을 귀여운 척이라고 생각해 매우 싫어하므로, 승엽이의 문자가 오면 전체적인 분위기만 판단해서 알려주는 프로그램을 작성하고 싶다.

출처 : https://www.acmicpc.net/problem/10769

---

#### 풀이
~~~java
import java.util.*;
import java.util.function.BiPredicate;
import java.util.regex.Pattern;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Emotion emotion = new Emotion();
        System.out.println(emotion.getState(scanner.nextLine()));
    }
}

class Emotion {
    private static final Pattern happy = Pattern.compile(":-\\)");
    private static final Pattern sad = Pattern.compile(":-\\(");

    public String getState(String text) {
        int happyCount = getCount(happy, text);
        int sadCount = getCount(sad, text);
        EmotionState emotionState = EmotionState.valueOf(happyCount, sadCount);
        return emotionState.getState();
    }

    private int getCount(Pattern pattern, String text) {
        return (int) pattern.matcher(text)
                .results()
                .count();
    }

    enum EmotionState {
        NONE("none", (happy, sad) -> happy == 0 && sad == 0),
        HAPPY("happy", (happy, sad) -> happy > sad),
        SAD("sad", (happy, sad) -> sad > happy),
        UNSURE("unsure", (happy, sad) -> false);

        private final String state;
        private final BiPredicate<Integer, Integer> expression;

        EmotionState(String state, BiPredicate<Integer, Integer> expression) {
            this.state = state;
            this.expression = expression;
        }

        public static EmotionState valueOf(int happyCount, int sadCount) {
            for (EmotionState emotionState : values()) {
                if (emotionState.expression.test(happyCount, sadCount)) {
                    return emotionState;
                }
            }
            return UNSURE;
        }

        public String getState() {
            return this.state;
        }
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

import java.util.function.BiPredicate;
import java.util.regex.Pattern;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideHappyTexts")
    @DisplayName("행복한 이모티콘 :-)이 많다면 happy를 반환한다.")
    void isHappyTest(String text) {
        Emotion sut = new Emotion();

        String actual = sut.getState(text);

        assertEquals("happy", actual);
    }

    private static Stream<Arguments> provideHappyTexts() {
        return Stream.of(
                Arguments.of(":-)"),
                Arguments.of(":-) :-( :-)"),
                Arguments.of("How are you :-) doing :-( today :-)?")
        );
    }

    @ParameterizedTest
    @MethodSource("provideSadTexts")
    @DisplayName("슬픈 이모티콘 :-(이 많다면 sad를 반환한다.")
    void isSadTest(String text) {
        Emotion sut = new Emotion();

        String actual = sut.getState(text);

        assertEquals("sad", actual);
    }

    private static Stream<Arguments> provideSadTexts() {
        return Stream.of(
                Arguments.of(":-("),
                Arguments.of("This:-(is str:-(:-(ange te:-)xt.")
        );
    }

    @ParameterizedTest
    @MethodSource("provideNoneTexts")
    @DisplayName("이모티콘 없다면 none을 반환한다.")
    void isNoneTest(String text) {
        Emotion sut = new Emotion();

        String actual = sut.getState(text);

        assertEquals("none", actual);
    }

    private static Stream<Arguments> provideNoneTexts() {
        return Stream.of(
                Arguments.of("asdf"),
                Arguments.of(":)"),
                Arguments.of(" "),
                Arguments.of("")
        );
    }

    @ParameterizedTest
    @MethodSource("provideUnsureTexts")
    @DisplayName("행복한 이모티콘과 슬픈 이모티콘의 수가 같다면 unsure를 반환한다.")
    void isUnsureTest(String text) {
        Emotion sut = new Emotion();

        String actual = sut.getState(text);

        assertEquals("unsure", actual);
    }

    private static Stream<Arguments> provideUnsureTexts() {
        return Stream.of(
                Arguments.of(":-) :-("),
                Arguments.of(":-) :-( :-) :-( :-) :-(")
        );
    }
}

class Emotion {
    private static final Pattern happy = Pattern.compile(":-\\)");
    private static final Pattern sad = Pattern.compile(":-\\(");

    public String getState(String text) {
        int happyCount = getCount(happy, text);
        int sadCount = getCount(sad, text);
        EmotionState emotionState = EmotionState.valueOf(happyCount, sadCount);
        return emotionState.getState();
    }

    private int getCount(Pattern pattern, String text) {
        return (int) pattern.matcher(text)
                .results()
                .count();
    }

    enum EmotionState {
        NONE("none", (happy, sad) -> happy == 0 && sad == 0),
        HAPPY("happy", (happy, sad) -> happy > sad),
        SAD("sad", (happy, sad) -> sad > happy),
        UNSURE("unsure", (happy, sad) -> false);

        private final String state;
        private final BiPredicate<Integer, Integer> expression;

        EmotionState(String state, BiPredicate<Integer, Integer> expression) {
            this.state = state;
            this.expression = expression;
        }

        public static EmotionState valueOf(int happyCount, int sadCount) {
            for (EmotionState emotionState : values()) {
                if (emotionState.expression.test(happyCount, sadCount)) {
                    return emotionState;
                }
            }
            return UNSURE;
        }

        public String getState() {
            return this.state;
        }
    }
}
~~~
