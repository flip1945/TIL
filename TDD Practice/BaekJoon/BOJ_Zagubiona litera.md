# Zagubiona litera (Bronze 4)

### 문제 설명

Bajtosia pracuje w przedszkolu jako nauczycielka angielskiego. Dzisiaj na lekcji dzieci mają poznać litery alfabetu angielskiego. Bajtosia przygotowała na tę lekcję 26 specjalnych tabliczek, z których każda zawierała jedną literę alfabetu.

Niestety w ostatniej chwili Bajtosia zauważyła, że ma jedynie 25 tabliczek. Jedna z nich musiała gdzieś się zapodziać! Czy możesz określić, której tabliczki brakuje?

출처 : https://www.acmicpc.net/problem/26731

---

#### 풀이
~~~java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        AlphabetDishes alphabetDishes = new AlphabetDishes(scanner.nextLine());
        System.out.println(alphabetDishes.missingDish());
    }
}

class AlphabetDishes {

    private static final String FULL_DISHES = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";

    private final String dishes;

    public AlphabetDishes(String dishes) {
        this.dishes = dishes;
    }

    public String missingDish() {
        return Arrays.stream(FULL_DISHES.split(""))
                .filter(this::isMissingDish)
                .findFirst()
                .orElseThrow(IllegalArgumentException::new);
    }

    private boolean isMissingDish(String dish) {
        return !dishes.contains(dish);
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

import java.util.Arrays;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.assertEquals;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideDishes")
    @DisplayName("알파벳 접시가 주어지면 빠진 접시를 반환한다.")
    void alphabetDishesReturnsMissingDish(String dishes, String expected) {
        AlphabetDishes sut = new AlphabetDishes(dishes);

        String actual = sut.missingDish();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideDishes() {
        return Stream.of(
                Arguments.of("BCDEFGHIJKLMNOPQRSTUVWXYZ", "A"),
                Arguments.of("ACDEFGHIJKLMNOPQRSTUVWXYZ", "B"),
                Arguments.of("QWERTYUIPASDFGHJKLZXCVBNM", "O"),
                Arguments.of("QAZWSXEDCRFVTGBYHNUJMKOLP", "I"),
                Arguments.of("ABCDEFGHIKLMNOPQRSTUVWXYZ", "J")
        );
    }
}

class AlphabetDishes {

    private static final String FULL_DISHES = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";

    private final String dishes;

    public AlphabetDishes(String dishes) {
        this.dishes = dishes;
    }

    public String missingDish() {
        return Arrays.stream(FULL_DISHES.split(""))
                .filter(this::isMissingDish)
                .findFirst()
                .orElseThrow(IllegalArgumentException::new);
    }

    private boolean isMissingDish(String dish) {
        return !dishes.contains(dish);
    }
}
~~~
