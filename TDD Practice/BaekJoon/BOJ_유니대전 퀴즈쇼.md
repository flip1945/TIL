# 유니대전 퀴즈쇼 (Bronze 1)

### 문제 설명

올해 인천대에서는 코로나19로 인해 온라인 축제를 개최했다. 축제 내용 중에는 퀴즈쇼가 있는데, 초청 연예인이 채팅을 보고 정답을 맞힌 사람의 닉네임을 읽어 1명에게 상품을 주는 이벤트이다.

축제를 즐기던 철이는 퀴즈쇼가 끝난 뒤 커뮤니티에 당첨자보다 정답을 빨리 쳤다며 아쉬워하는 사람들이 나타난 것을 보았다. 채팅 기록을 갖고 있는 철이는 그런 아쉬운 사람들이 몇 명이나 있는지 알고 싶어졌다. 채팅 기록은 여러 줄로 이루어져 있는데, 각 줄에는 채팅을 친 사람의 닉네임과 채팅 내용이 담겨있다.

채팅 기록과 당첨자가 주어졌을 때 아쉬운 사람의 수를 구해보자. 아쉬운 사람은 당첨자보다 빨리 정답을 외친 사람이다.

출처 : https://www.acmicpc.net/problem/20362

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        StringTokenizer st = new StringTokenizer(scanner.nextLine());
        int n = Integer.parseInt(st.nextToken());
        String winner = st.nextToken();
        List<String> chattingLogs = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            chattingLogs.add(scanner.nextLine());
        }

        QuizShow quizShow = new QuizShow(chattingLogs, winner);
        System.out.println(quizShow.countEarlyAnswers());
    }
}

class QuizShow {

    private final List<String> chattingLogs;
    private final String winner;

    public QuizShow(List<String> chattingLogs, String winner) {
        this.chattingLogs = chattingLogs;
        this.winner = winner;
    }

    public int countEarlyAnswers() {
        return (int) chattingLogs.stream()
                .map(Chatting::from)
                .takeWhile(chatting -> !chatting.equalUser(winner))
                .filter(chatting -> Objects.equals(chatting.getChatting(), getAnswer()))
                .count();
    }

    private String getAnswer() {
        return chattingLogs.stream()
                .map(Chatting::from)
                .filter(chatting -> chatting.equalUser(winner))
                .map(Chatting::getChatting)
                .findFirst()
                .orElseThrow();
    }
}

class Chatting {

    private final String user;
    private final String chatting;

    private Chatting(String user, String chatting) {
        this.user = user;
        this.chatting = chatting;
    }

    public static Chatting from(String chattingLog) {
        String[] split = chattingLog.split(" ");
        return new Chatting(split[0], split[1]);
    }

    public String getUser() {
        return user;
    }

    public String getChatting() {
        return chatting;
    }

    public boolean equalUser(String user) {
        return Objects.equals(user, this.user);
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

import java.util.List;
import java.util.Objects;
import java.util.stream.*;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideChattingLogs")
    @DisplayName("정답자 보다 먼저 정답을 맟춘 사람의 수를 반환한다.")
    void quizShowTest(List<String> chattingLogs, String winner, int expected) {
        QuizShow sut = new QuizShow(chattingLogs, winner);

        int actual = sut.countEarlyAnswers();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideChattingLogs() {
        return Stream.of(
                Arguments.of(List.of("a answer", "b answer"), "b", 1),
                Arguments.of(List.of("a answer", "b no"), "a", 0),
                Arguments.of(List.of("oridya hello", "orihehe hi", "duck hi"), "duck", 1),
                Arguments.of(List.of("orihehe duck", "skynet duck", "rdd duck", "vega duck", "reversing duck", "dongbin duck", "kimyh duck", "hunni duck"), "orihehe", 0),
                Arguments.of(List.of("hunni duck", "skynet duck", "rdd duck", "vega duck", "reversing duck", "dongbin duck", "kimyh duck", "orihehe duck"), "orihehe", 7),
                Arguments.of(List.of("hunni dduck", "skynet dduck", "rdd dduck", "vega dduck", "reversing dduck", "dongbin dduck", "kimyh dduck", "orihehe duck"), "orihehe", 0)
        );
    }
}

class QuizShow {

    private final List<String> chattingLogs;
    private final String winner;

    public QuizShow(List<String> chattingLogs, String winner) {
        this.chattingLogs = chattingLogs;
        this.winner = winner;
    }

    public int countEarlyAnswers() {
        return (int) chattingLogs.stream()
                .map(Chatting::from)
                .takeWhile(chatting -> !chatting.equalUser(winner))
                .filter(chatting -> Objects.equals(chatting.getChatting(), getAnswer()))
                .count();
    }

    private String getAnswer() {
        return chattingLogs.stream()
                .map(Chatting::from)
                .filter(chatting -> chatting.equalUser(winner))
                .map(Chatting::getChatting)
                .findFirst()
                .orElseThrow();
    }
}

class Chatting {

    private final String user;
    private final String chatting;

    private Chatting(String user, String chatting) {
        this.user = user;
        this.chatting = chatting;
    }

    public static Chatting from(String chattingLog) {
        String[] split = chattingLog.split(" ");
        return new Chatting(split[0], split[1]);
    }

    public String getUser() {
        return user;
    }

    public String getChatting() {
        return chatting;
    }

    public boolean equalUser(String user) {
        return Objects.equals(user, this.user);
    }
}
~~~
