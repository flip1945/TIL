# 너의 핸들은 (Bronze 1)

### 문제 설명

문제 해결(Problem Solving) 능력을 기르는 데에 도움이 되는 여러 사이트가 있다. 국내 최대 규모의 Baekjoon Online Judge와 아주대학교 알고리즘 소학회 A.N.S.I.에서 관리하는 Lavida Online Judge와 같은 국내 온라인 채점 사이트를 비롯해 전 세계인이 모여 (비)정기적으로 대회를 치르는 Codeforces나 TopCoder 등 다양한 특색의 사이트가 존재한다.

온라인 게임에서 서로를 구분하기 위해 ID를 사용하듯, 이러한 알고리즘 사이트들에서도 ID를 사용한다. 여러 알고리즘 사이트에서 동일한 ID를 사용하는 사람들도 많은데, 이 ID는 핸들(Handle)이라고도 불린다. 대화에서도 유저의 본명보다 핸들이 등장할 때가 종종 있으며, 유명한 유저의 핸들은 대명사로 사용되기도 한다.

상민이는 알고리즘 사이트와 대회장 등에서 'qilip'라는 귀엽고 대칭성마저 완벽한 핸들을 사용한다. 하지만 상민이에게는 말 못할 고민이 있는데, 바로 남들이 자신의 핸들을 자꾸 헷갈린다는 것이다. 그중에서도 현정이는 상민이의 핸들을 'q'로 시작하고 'p'로 끝나는 것만 기억하며 'qp'라고 부른다. 'quilip'까지는 헷갈릴 수 있지만 'qp'라니, 자신의 정체성을 부정당한 상민이는 어떻게든 현정이에게 자신의 핸들을 각인시키고 싶다.

현정이는 사람의 이름과 사람을 매칭시키는 능력은 매우 떨어지지만, 어떤 핸들이 자신이 아는 핸들 중 사전 순으로 몇 번째인지 기억하는 쓸데없는 능력을 가지고 있다. 이를 이용해 상민이는 현정이가 아는 핸들 목록 중 자신이 몇 번째인지를 알아내어 앞으로 번호를 붙여 다니려고 한다. 정체성을 지키고 싶은 상민이를 위해 현정이의 어지러운 머리속에서 임의의 순서를 가지는 핸들을 찾아보자.

출처 : https://www.acmicpc.net/problem/15819

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        StringTokenizer st = new StringTokenizer(scanner.nextLine());
        int n = Integer.parseInt(st.nextToken());
        int x = Integer.parseInt(st.nextToken());
        Handles handles = Handles.from(inputHandles(scanner, n));
        System.out.println(handles.indexOf(x));
    }

    private static List<String> inputHandles(Scanner scanner, int n) {
        return IntStream.range(0, n)
            .mapToObj(i -> scanner.nextLine())
            .collect(Collectors.toList());
    }
}

class Handles {

    private final List<String> handles;

    private Handles(List<String> handles) {
        this.handles = handles;
    }

    public static Handles from(List<String> handles) {
        return new Handles(handles.stream()
            .sorted()
            .collect(Collectors.toList()));
    }

    public String indexOf(int index) {
        return handles.get(index - 1);
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

import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class MainTest {

    @ParameterizedTest
    @MethodSource("provideHandles")
    @DisplayName("사전 순으로 n번째인 핸들을 반환한다.")
    void it_the_nth_handle_in_dictionary_order(List<String> handles, int n, String expected) {
        Handles sut = Handles.from(handles);
        
        String actual = sut.indexOf(n);
        
        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideHandles() {
        return Stream.of(
            Arguments.of(List.of("a", "b"), 1, "a"),
            Arguments.of(List.of("a", "b", "c"), 1, "a"),
            Arguments.of(List.of("a", "b", "c"), 2, "b"),
            Arguments.of(List.of("a", "b", "c"), 3, "c"),
            Arguments.of(List.of("a"), 1, "a"),
            Arguments.of(List.of("1"), 1, "1"),
            Arguments.of(List.of("acka1357", "spectaclehong", "mitslll", "luke0201"), 1, "acka1357"),
            Arguments.of(List.of("tourist", "petr", "qilip", "won0114", "hmy3743", "jujh97", "hjhj97", "bio8641", "kangjieun9843"), 7, "qilip")
        );
    }
}

class Handles {
    
    private final List<String> handles;

    private Handles(List<String> handles) {
        this.handles = handles;
    }
    
    public static Handles from(List<String> handles) {
        return new Handles(handles.stream()
            .sorted()
            .collect(Collectors.toList()));
    }

    public String indexOf(int index) {
        return handles.get(index - 1);
    }
}
~~~
