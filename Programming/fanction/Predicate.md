---
создал заметку: 2024-07-24
---
Predicate<`T`>

### Зачем он нужен

Интерфейс `Predicate<T>` представляет собой функциональный интерфейс, который используется для определения условий или предикатов, возвращающих логическое значение (true или false). Этот интерфейс является основой для написания кода, который фильтрует данные или проверяет соответствие определенным условиям.

#### Когда используется

`Predicate<T>` часто используется в таких случаях:

1. **Фильтрация коллекций**: Например, когда нужно отфильтровать список объектов по определенному критерию.
2. **Проверка условий**: Когда нужно проверить, удовлетворяет ли объект определенному условию.
3. **Методы, принимающие Predicate в качестве параметра**: Например, методы в стандартных коллекциях, такие как `removeIf`.

#### Методы интерфейса Predicate<`T`>

Основные методы интерфейса `Predicate<T>`:

- **boolean test(T t)**: Основной метод, который принимает объект типа `T` и возвращает `boolean` значение.
- **default Predicate<`T`> and(Predicate`<`? super T other`>`): Возвращает составной предикат, который представляет собой логическое И между текущим предикатом и другим.
- **default Predicate<`T`> negate()**: Возвращает предикат, который является логическим отрицанием текущего предиката.
- **default Predicate<`T`> or(Predicate`<`? super T other`>`)**: Возвращает составной предикат, который представляет собой логическое ИЛИ между текущим предикатом и другим.
- **static <`T`> Predicate<`T`> isEqual(Object targetRef)**: Возвращает предикат, который проверяет равенство с указанным объектом.

#### Примеры использования

1. **Фильтрация списка строк на основе длины**:
```java
import java.util.ArrayList;
import java.util.List;
import java.util.function.Predicate;
import java.util.stream.Collectors;

public class PredicateExample {
    public static void main(String[] args) {
        List<String> names = new ArrayList<>();
        names.add("John");
        names.add("Jane");
        names.add("Jim");
        names.add("Jill");

        Predicate<String> lengthGreaterThanThree = name -> name.length() > 3;

        List<String> filteredNames = names.stream()
                                          .filter(lengthGreaterThanThree)
                                          .collect(Collectors.toList());

        System.out.println(filteredNames); // Output: [John, Jane, Jill]
    }
}
```

**Использование методов and() и or()**:
```java
import java.util.function.Predicate;

public class PredicateExample {
    public static void main(String[] args) {
        Predicate<String> startsWithJ = name -> name.startsWith("J");
        Predicate<String> lengthGreaterThanThree = name -> name.length() > 3;

        Predicate<String> combinedPredicate = startsWithJ.and(lengthGreaterThanThree);

        System.out.println(combinedPredicate.test("John")); // Output: true
        System.out.println(combinedPredicate.test("Jo"));   // Output: false

        Predicate<String> orPredicate = startsWithJ.or(name -> name.startsWith("A"));

        System.out.println(orPredicate.test("John")); // Output: true
        System.out.println(orPredicate.test("Alice")); // Output: true
        System.out.println(orPredicate.test("Bob"));   // Output: false
    }
}
```
**Использование метода negate()**:
```java
import java.util.function.Predicate;

public class PredicateExample {
    public static void main(String[] args) {
        Predicate<String> isEmpty = String::isEmpty;
        Predicate<String> isNotEmpty = isEmpty.negate();

        System.out.println(isNotEmpty.test("Hello")); // Output: true
        System.out.println(isNotEmpty.test(""));      // Output: false
    }
}

```

**Пример с использованием removeIf()**:
```java
import java.util.ArrayList;
import java.util.List;

public class PredicateExample {
    public static void main(String[] args) {
        List<String> names = new ArrayList<>();
        names.add("John");
        names.add("Jane");
        names.add("Jim");
        names.add("Jill");

        Predicate<String> startsWithJ = name -> name.startsWith("J");

        names.removeIf(startsWithJ);

        System.out.println(names); // Output: []
    }
}

```

#### Заключение

Интерфейс `Predicate<T>` является мощным инструментом для работы с условиями в Java. Он упрощает написание кода для фильтрации данных, проверки условий и создания составных предикатов. Использование встроенных функциональных интерфейсов, таких как `Predicate<T>`, делает ваш код более понятным и поддерживаемым.



### Связь Predicate и Comparator (Comparable)

`Predicate` и `Comparator` (или `Comparable`) — это два различных функциональных интерфейса, предназначенные для разных целей:

1. **Predicate**:
    
    - Это функциональный интерфейс, представляющий собой одноаргументную функцию, возвращающую логическое значение (`boolean`).
    - Используется для проверки условия, фильтрации данных и т.п.
    - Основной метод: `boolean test(T t)`.
    
    **Пример использования Predicate**:
    ```java
    Predicate<Integer> isEven = num -> num % 2 == 0;
	System.out.println(isEven.test(4)); // Output: true
	System.out.println(isEven.test(5)); // Output: false

```

**Comparator** и **Comparable**:

- Это функциональные интерфейсы, предназначенные для сравнения объектов с целью их сортировки.
- `Comparable` используется для естественного порядка сортировки (например, сортировка строк в алфавитном порядке).
- `Comparator` используется для задания пользовательского порядка сортировки.

**Основной метод Comparator**:
```java
int compare(T o1, T o2);
```

**Пример использования Comparator**:
```java
Comparator<Integer> ascendingOrder = (a, b) -> a - b;
List<Integer> numbers = Arrays.asList(5, 3, 8, 1);
Collections.sort(numbers, ascendingOrder);
System.out.println(numbers); // Output: [1, 3, 5, 8]
```

**Пример использования Comparable**:

```java
public class Person implements Comparable<Person> {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public int compareTo(Person other) {
        return Integer.compare(this.age, other.age);
    }
}

List<Person> people = Arrays.asList(
    new Person("Alice", 30),
    new Person("Bob", 25),
    new Person("Charlie", 35)
);
Collections.sort(people);
```

