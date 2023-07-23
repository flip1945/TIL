# 콰트로치즈피자 (Silver 5)

### 문제 설명

치즈와 피자에 환장하는 비행씨는 매일같이 치즈피자를 사 먹다가 지갑이 거덜 나고 말았다. 만들어 먹는 것이 사 먹는 것보다 싸다는 것을 안 비행씨는 여러 가지 토핑을 가져와서 직접 피자를 만들어 먹기로 했다.

콰트로치즈피자는 이름 그대로, 서로 다른 네 종류의 치즈가 토핑으로 들어가야 한다. 수많은 치즈피자를 먹어 온 비행씨는 토핑의 이름이 Cheese로 끝나면 이 토핑이 치즈라는 사실을 알고 있다. 비행씨가 가져온 토핑의 목록을 보고, 이 토핑의 일부 혹은 전부를 이용하여 콰트로치즈피자를 만들 수 있는지 답해 보자.

출처 : https://www.acmicpc.net/problem/27964

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        List<String> ingredients = inputIngredients(scanner);

        QuattroCheesePizza quattroCheesePizza = new QuattroCheesePizza(ingredients);
        System.out.println(quattroCheesePizza.canCook());
    }

    private static List<String> inputIngredients(Scanner scanner) {
        return Arrays.stream(scanner.nextLine().split(" ")).collect(Collectors.toList());
    }
}

class QuattroCheesePizza {

    public static final int QUATTRO_PIZZA_CHEESE_SIZE = 4;
    public static final String TRUE_STRING = "yummy";
    public static final String FALSE_STRING = "sad";

    private final List<String> ingredients;

    public QuattroCheesePizza(List<String> ingredients) {
        this.ingredients = ingredients;
    }

    public String canCook() {
        return (getCheeses().size() >= QUATTRO_PIZZA_CHEESE_SIZE) ? TRUE_STRING : FALSE_STRING;
    }

    private Set<Cheese> getCheeses() {
        return ingredients.stream()
                .map(Cheese::new)
                .filter(Cheese::isCheese)
                .collect(Collectors.toSet());
    }
}

class Cheese {

    public static final String CHEESE_SUFFIX = "Cheese";

    private final String ingredient;

    public Cheese(String ingredient) {
        this.ingredient = ingredient;
    }

    public boolean isCheese() {
        return ingredient.endsWith(CHEESE_SUFFIX);
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Cheese cheese = (Cheese) o;
        return Objects.equals(ingredient, cheese.ingredient);
    }

    @Override
    public int hashCode() {
        return Objects.hash(ingredient);
    }
}
~~~

---

#### 테스트 코드
~~~java
import java.util.*;
import java.util.stream.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = Integer.parseInt(scanner.nextLine());
        List<String> ingredients = inputIngredients(scanner);

        QuattroCheesePizza quattroCheesePizza = new QuattroCheesePizza(ingredients);
        System.out.println(quattroCheesePizza.canCook());
    }

    private static List<String> inputIngredients(Scanner scanner) {
        return Arrays.stream(scanner.nextLine().split(" ")).collect(Collectors.toList());
    }
}

class QuattroCheesePizza {

    public static final int QUATTRO_PIZZA_CHEESE_SIZE = 4;
    public static final String TRUE_STRING = "yummy";
    public static final String FALSE_STRING = "sad";

    private final List<String> ingredients;

    public QuattroCheesePizza(List<String> ingredients) {
        this.ingredients = ingredients;
    }

    public String canCook() {
        return (getCheeses().size() >= QUATTRO_PIZZA_CHEESE_SIZE) ? TRUE_STRING : FALSE_STRING;
    }

    private Set<Cheese> getCheeses() {
        return ingredients.stream()
                .map(Cheese::new)
                .filter(Cheese::isCheese)
                .collect(Collectors.toSet());
    }
}

class Cheese {

    public static final String CHEESE_SUFFIX = "Cheese";

    private final String ingredient;

    public Cheese(String ingredient) {
        this.ingredient = ingredient;
    }

    public boolean isCheese() {
        return ingredient.endsWith(CHEESE_SUFFIX);
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Cheese cheese = (Cheese) o;
        return Objects.equals(ingredient, cheese.ingredient);
    }

    @Override
    public int hashCode() {
        return Objects.hash(ingredient);
    }
}
~~~
