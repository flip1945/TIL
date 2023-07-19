# 선린인터넷고등학교 교가 (Bronze 3)

### 문제 설명

드높은 남산 위에 우뚝 선

송백은 흰 눈빛에 푸르고

옛부터 흘러가는 한가람

장 할 손 우리 학원 이룩한

굳세고 다함 없는 거룩한 뜻이

백이십년 빛난 역사 자랑이로세

비바람 몰아쳐도 나가자

공들여 쌓은 탑은 빛난다

울려라 삼천리에 힘차게

세워라 반석 위에

선린의터를

선린인터넷고등학교 학생들은 이미 잘 알고 있겠지만, 학교 교가를 부를 때는 마지막 5글자인 "선린의터를" 부분만 크고 우렁차게 불러야 한다.

정휘는 여기에 영감을 받아, 문자열이 주어지면 마지막 5글자만 우렁차게 읽으려고 한다. 공백이 없는 문자열이 주어지면 마지막 5글자만 출력하는 프로그램을 작성해보자.

출처 : https://www.acmicpc.net/problem/21964

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        scanner.nextLine();

        SchoolSong schoolSong = new SchoolSong(scanner.nextLine());
        System.out.println(schoolSong.getLastWord());
    }
}

class SchoolSong {

    public static final int LAST_WORD_SIZE = 5;
    private final String schoolSong;

    public SchoolSong(String schoolSong) {
        this.schoolSong = schoolSong;
    }

    public String getLastWord() {
        return schoolSong.substring(getBeginIndexOfLastWord());
    }

    private int getBeginIndexOfLastWord() {
        return schoolSong.length() - LAST_WORD_SIZE;
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
    @MethodSource("provideSchoolSongs")
    @DisplayName("교가의 마지막 단어를 반환한다.")
    void schoolSongTest(String song, String expected) {
        SchoolSong sut = new SchoolSong(song);

        String actual = sut.getLastWord();

        assertEquals(expected, actual);
    }

    private static Stream<Arguments> provideSchoolSongs() {
        return Stream.of(
                Arguments.of("asdfe", "asdfe"),
                Arguments.of("abcde", "abcde"),
                Arguments.of("abcde.", "bcde."),
                Arguments.of("Sunrin,Hair.", "Hair.")
        );
    }
}

class SchoolSong {

    public static final int LAST_WORD_SIZE = 5;
    private final String schoolSong;

    public SchoolSong(String schoolSong) {
        this.schoolSong = schoolSong;
    }

    public String getLastWord() {
        return schoolSong.substring(getBeginIndexOfLastWord());
    }

    private int getBeginIndexOfLastWord() {
        return schoolSong.length() - LAST_WORD_SIZE;
    }
}
~~~
