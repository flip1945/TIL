# 여우는 어떻게 울지? (Silver 4)

### 문제 설명

고대 미스테리로 전해지는 여우의 울음소리를 밝혀내기 위해 한신이는 고성능 녹음기로 무장하고 숲으로 들어갔다. 하지만 숲에는 동물들이 가득해, 녹음된 음성에는 다른 동물들의 울음소리가 섞여 있다. 그러나 한신이는 철저한 준비를 해 왔기 때문에 다른 동물들이 어떤 울음소리를 내는지 정확히 알고 있다. 그러므로 그 소리를 모두 걸러내면 남은 잡음은 분명히 여우의 울음소리일 것이다.

출처 : https://www.acmicpc.net/problem/9536

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int t = Integer.parseInt(scanner.nextLine());
        while (t --> 0) {
            String howling = scanner.nextLine();
            List<String> howlingOfAnimals = new ArrayList<>();
            String howlingOfAnimal = "";
            while (!howlingOfAnimal.equals("what does the fox say?")) {
                howlingOfAnimal = scanner.nextLine();
                howlingOfAnimals.add(howlingOfAnimal);
            }
            System.out.println(whatDoesFoxSay(howling, howlingOfAnimals));
        }
    }

    static String whatDoesFoxSay(String howling, List<String> howlingOfAnimals) {
        return getFoxHowling(howling, getOtherAnimalHowling(howlingOfAnimals));
    }

    static String getFoxHowling(String howling, Set<String> otherAnimalHowling) {
        StringBuilder result = new StringBuilder();
        for (String h : howling.split(" ")) {
            if (!otherAnimalHowling.contains(h)) {
                result.append(h).append(" ");
            }
        }
        return result.toString().trim();
    }

    static Set<String> getOtherAnimalHowling(List<String> howlingOfAnimals) {
        return howlingOfAnimals.stream()
                .map(Main::parseHowling)
                .collect(Collectors.toSet());
    }

    static String parseHowling(String howlingOfAnimal) {
        return howlingOfAnimal.split(" ")[2];
    }
}
~~~

---

#### 테스트 코드
~~~java
import org.junit.jupiter.api.Test;

import java.util.HashSet;
import java.util.List;
import java.util.Set;
import java.util.stream.Collectors;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @Test
    void whatDoesFoxSayTest() {
        assertEquals("a", whatDoesFoxSay("a b", List.of("b goes b")));
        assertEquals("a c", whatDoesFoxSay("a b c", List.of("b goes b")));
        assertEquals("b", whatDoesFoxSay("a b c", List.of("a goes a", "c goes c")));
        assertEquals("wa pa pa pa pa pa pow", whatDoesFoxSay("toot woof wa ow ow ow pa blub blub pa toot pa blub pa pa ow pow toot", List.of("dog goes woof", "fish goes blub", "elephant goes toot", "seal goes ow")));
    }

    private String whatDoesFoxSay(String howling, List<String> howlingOfAnimals) {
        return getFoxHowling(howling, getOtherAnimalHowling(howlingOfAnimals));
    }

    private String getFoxHowling(String howling, Set<String> otherAnimalHowling) {
        StringBuilder result = new StringBuilder();
        for (String h : howling.split(" ")) {
            if (!otherAnimalHowling.contains(h)) {
                result.append(h).append(" ");
            }
        }
        return result.toString().trim();
    }

    private Set<String> getOtherAnimalHowling(List<String> howlingOfAnimals) {
        return howlingOfAnimals.stream()
                .map(this::parseHowling)
                .collect(Collectors.toSet());
    }

    private String parseHowling(String howlingOfAnimal) {
        return howlingOfAnimal.split(" ")[2];
    }
}
~~~
