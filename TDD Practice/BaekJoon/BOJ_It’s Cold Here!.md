# It’s Cold Here! (Bronze 3)

### 문제 설명

Canada is cold in winter, but some parts are colder than others. Your task is very simple, you need to find the coldest city in Canada. So, when given a list of cities and their temperatures, you are to determine which city in the list has the lowest temperature and is thus the coldest.

출처 : https://www.acmicpc.net/problem/11170

---

#### 풀이
~~~java
import java.util.*;
import java.util.stream.Collectors;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String cityAndTemperature = scanner.nextLine();
        List<String> citiesAndTemperatures = new ArrayList<>();

        while (!cityAndTemperature.startsWith("Waterloo ")) {
            citiesAndTemperatures.add(cityAndTemperature);
            cityAndTemperature = scanner.nextLine();
        }
        citiesAndTemperatures.add(cityAndTemperature);

        Cities cities = Cities.from(citiesAndTemperatures);
        System.out.println(cities.coldestCityName());
    }
}

class Cities {

    private final List<City> cities;

    public Cities(List<City> cities) {
        this.cities = cities;
    }

    public static Cities from(List<String> citiesAndTemperatures) {
        return new Cities(citiesAndTemperatures.stream()
                .map(City::from)
                .collect(Collectors.toList()));
    }

    public String coldestCityName() {
        return cities.stream()
                .sorted()
                .findFirst()
                .map(City::getName)
                .orElseThrow();
    }
}

class City implements Comparable<City> {

    private static final String DELIMITER = " ";
    private static final int NAME_INDEX = 0;
    private static final int TEMPERATURE_INDEX = 1;

    private final String name;
    private final int temperature;

    public City(String name, int temperature) {
        this.name = name;
        this.temperature = temperature;
    }

    public static City from(String cityAndTemperature) {
        String[] split = cityAndTemperature.split(DELIMITER);
        return new City(split[NAME_INDEX], Integer.parseInt(split[TEMPERATURE_INDEX]));
    }

    public String getName() {
        return name;
    }

    @Override
    public int compareTo(City o) {
        return Integer.compare(temperature, o.temperature);
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
import java.util.stream.Collectors;
import java.util.stream.Stream;

import static org.junit.jupiter.api.Assertions.*;

class MainTest {

    @ParameterizedTest
    @MethodSource("provideCities")
    @DisplayName("가장 추운 도시를 반환한다.")
    void citiesReturnTheColdestCity(List<String> citiesAndTemperatures, String expected) {
        Cities sut = Cities.from(citiesAndTemperatures);

        String actual = sut.coldestCityName();

        assertEquals(expected, actual);
    }

    static Stream<Arguments> provideCities() {
        return Stream.of(
                Arguments.of(List.of("Toronto 0"), "Toronto"),
                Arguments.of(List.of("Vancouver 0"), "Vancouver"),
                Arguments.of(List.of("Vancouver 0", "Toronto -10"), "Toronto"),
                Arguments.of(List.of("Saskatoon -20", "Toronto -2", "Winnipeg -40", "Vancouver 8", "Halifax 0", "Montreal -4", "Waterloo -3"), "Winnipeg")
        );
    }
}

class Cities {

    private final List<City> cities;

    public Cities(List<City> cities) {
        this.cities = cities;
    }

    public static Cities from(List<String> citiesAndTemperatures) {
        return new Cities(citiesAndTemperatures.stream()
                .map(City::from)
                .collect(Collectors.toList()));
    }

    public String coldestCityName() {
        return cities.stream()
                .sorted()
                .findFirst()
                .map(City::getName)
                .orElseThrow();
    }
}

class City implements Comparable<City> {

    private static final String DELIMITER = " ";
    private static final int NAME_INDEX = 0;
    private static final int TEMPERATURE_INDEX = 1;

    private final String name;
    private final int temperature;

    public City(String name, int temperature) {
        this.name = name;
        this.temperature = temperature;
    }

    public static City from(String cityAndTemperature) {
        String[] split = cityAndTemperature.split(DELIMITER);
        return new City(split[NAME_INDEX], Integer.parseInt(split[TEMPERATURE_INDEX]));
    }

    public String getName() {
        return name;
    }

    @Override
    public int compareTo(City o) {
        return Integer.compare(temperature, o.temperature);
    }
}
~~~
