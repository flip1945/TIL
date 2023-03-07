# 단축키 지정 (Silver 1)

### 문제 설명

한글 프로그램의 메뉴에는 총 N개의 옵션이 있다. 각 옵션들은 한 개 또는 여러 개의 단어로 옵션의 기능을 설명하여 놓았다. 그리고 우리는 위에서부터 차례대로 각 옵션에 단축키를 의미하는 대표 알파벳을 지정하기로 하였다. 단축키를 지정하는 법은 아래의 순서를 따른다.

1. 먼저 하나의 옵션에 대해 왼쪽에서부터 오른쪽 순서로 단어의 첫 글자가 이미 단축키로 지정되었는지 살펴본다. 만약 단축키로 아직 지정이 안 되어있다면 그 알파벳을 단축키로 지정한다.
2. 만약 모든 단어의 첫 글자가 이미 지정이 되어있다면 왼쪽에서부터 차례대로 알파벳을 보면서 단축키로 지정 안 된 것이 있다면 단축키로 지정한다.
3. 어떠한 것도 단축키로 지정할 수 없다면 그냥 놔두며 대소문자를 구분치 않는다.
4. 위의 규칙을 첫 번째 옵션부터 N번째 옵션까지 차례대로 적용한다.

출처 : https://www.acmicpc.net/problem/1283

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.IntStream;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        String[] options = IntStream.range(0, n).mapToObj(i -> scanner.nextLine()).toArray(String[]::new);

        getShortcuts(options).forEach(System.out::println);
    }

    static List<String> getShortcuts(String[] options) {
        List<String> shortcuts = new ArrayList<>();
        Set<Character> usedShortcut = new HashSet<>();

        for (String option : options) {
            int shortcutIndex = getShortcutIndexOfOption(usedShortcut, option);
            if (canUseShortcut(shortcutIndex)) {
                usedShortcut.add(Character.toLowerCase(option.charAt(shortcutIndex)));
            }
            shortcuts.add(getShortcutString(option, shortcutIndex));
        }
        return shortcuts;
    }

    static int getShortcutIndexOfOption(Set<Character> usedShortcut, String option) {
        int shortcutIndex = -1;
        char prevCharacter = ' ';
        for (int i = 0; i < option.length(); i++) {
            char currentCharacter = Character.toLowerCase(option.charAt(i));
            shortcutIndex = updateShortcutIndexIfNotUsed(usedShortcut, shortcutIndex, i, currentCharacter);
            if (isFirstCharacterOfWordAndNotUsed(usedShortcut, prevCharacter, currentCharacter)) {
                return i;
            }
            prevCharacter = currentCharacter;
        }
        return shortcutIndex;
    }

    static int updateShortcutIndexIfNotUsed(Set<Character> usedShortcut, int shortcutIndex, int i, char currentCharacter) {
        if (isNotUsedShortcut(usedShortcut, currentCharacter) && currentCharacter != ' ' && shortcutIndex == -1) {
            return i;
        }
        return shortcutIndex;
    }

    static boolean isNotUsedShortcut(Set<Character> usedShortcut, char currentCharacter) {
        return !usedShortcut.contains(currentCharacter);
    }

    static boolean isFirstCharacterOfWordAndNotUsed(Set<Character> usedShortcut, char prevCharacter, char currentCharacter) {
        return prevCharacter == ' ' && isNotUsedShortcut(usedShortcut, currentCharacter);
    }

    static boolean canUseShortcut(int shortcutIndex) {
        return shortcutIndex != -1;
    }

    static String getShortcutString(String option, int shortcutIndex) {
        if (shortcutIndex < 0 || shortcutIndex >= option.length()) {
            return option;
        }
        return option.substring(0, shortcutIndex) + "[" + option.charAt(shortcutIndex) + "]" + option.substring(shortcutIndex + 1);
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.*;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void getShortcutsTest() {
        assertEquals(List.of("[N]ew"), getShortcuts(new String[]{"New"}));
        assertEquals(List.of("[O]pen"), getShortcuts(new String[]{"Open"}));
        assertEquals(List.of("[N]ew", "N[e]w"), getShortcuts(new String[]{"New", "New"}));
        assertEquals(List.of("[S]ave", "Save [F]ile"), getShortcuts(new String[]{"Save", "Save File"}));
        assertEquals(List.of("[N]ew", "[O]pen", "[S]ave", "Save [A]s", "Sa[v]e All"), getShortcuts(new String[]{"New", "Open", "Save", "Save As", "Save All"}));
        assertEquals(List.of("[N]ew Window", "New [f]ile", "[C]opy", "[U]ndo", "F[o]rmat", "Fon[t]", "Cut", "[P]aste"), getShortcuts(new String[]{"New Window", "New file", "Copy", "Undo", "Format", "Font", "Cut", "Paste"}));
        assertEquals(List.of("[c]", "[u]", "[t]", "cut", "cut cut"), getShortcuts(new String[]{"c", "u", "t", "cut", "cut cut"}));
    }

    @Test
    void getShortcutStringTest() {
        assertEquals("[N]ew", getShortcutString("New", 0));
        assertEquals("N[e]w", getShortcutString("New", 1));
        assertEquals("Ne[w]", getShortcutString("New", 2));
        assertEquals("New", getShortcutString("New", -1));
        assertEquals("New", getShortcutString("New", 3));
        assertEquals("For[m]at", getShortcutString("Format", 3));
    }

    private List<String> getShortcuts(String[] options) {
        List<String> shortcuts = new ArrayList<>();
        Set<Character> usedShortcut = new HashSet<>();

        for (String option : options) {
            int shortcutIndex = getShortcutIndexOfOption(usedShortcut, option);
            if (canUseShortcut(shortcutIndex)) {
                usedShortcut.add(Character.toLowerCase(option.charAt(shortcutIndex)));
            }
            shortcuts.add(getShortcutString(option, shortcutIndex));
        }
        return shortcuts;
    }

    private int getShortcutIndexOfOption(Set<Character> usedShortcut, String option) {
        int shortcutIndex = -1;
        char prevCharacter = ' ';
        for (int i = 0; i < option.length(); i++) {
            char currentCharacter = Character.toLowerCase(option.charAt(i));
            shortcutIndex = updateShortcutIndexIfNotUsed(usedShortcut, shortcutIndex, i, currentCharacter);
            if (isFirstCharacterOfWordAndNotUsed(usedShortcut, prevCharacter, currentCharacter)) {
                return i;
            }
            prevCharacter = currentCharacter;
        }
        return shortcutIndex;
    }

    private int updateShortcutIndexIfNotUsed(Set<Character> usedShortcut, int shortcutIndex, int i, char currentCharacter) {
        if (isNotUsedShortcut(usedShortcut, currentCharacter) && currentCharacter != ' ' && shortcutIndex == -1) {
            return i;
        }
        return shortcutIndex;
    }

    private boolean isNotUsedShortcut(Set<Character> usedShortcut, char currentCharacter) {
        return !usedShortcut.contains(currentCharacter);
    }

    private boolean isFirstCharacterOfWordAndNotUsed(Set<Character> usedShortcut, char prevCharacter, char currentCharacter) {
        return prevCharacter == ' ' && isNotUsedShortcut(usedShortcut, currentCharacter);
    }

    private boolean canUseShortcut(int shortcutIndex) {
        return shortcutIndex != -1;
    }

    private String getShortcutString(String option, int shortcutIndex) {
        if (shortcutIndex < 0 || shortcutIndex >= option.length()) {
            return option;
        }
        return option.substring(0, shortcutIndex) + "[" + option.charAt(shortcutIndex) + "]" + option.substring(shortcutIndex + 1);
    }
}
~~~
