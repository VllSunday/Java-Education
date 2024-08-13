---
создал заметку: 2024-07-21
---
Вложенный классы нам нужны тогда, когда мы хотим сделать вложенный класс элементом внешнего класса.

`static nested class` — это вложенный класс, который объявлен как `static` внутри другого класса. Он очень похож на обычный внешний класс, но находится внутри другого класса. Вложенный класс имеет доступ ко всем статическим членам внешнего класса, включая private элементы, но не к нестатическим членам напрямую.

#### Основные особенности:

1. **Позиционирование**:
    - `static nested class` объявляется внутри другого класса.
2. **Создание объектов**:
    - При создании объекта `static nested class` необходимо указывать имя внешнего класса.
3. **Содержимое**:
    - `static nested class` может содержать как статические, так и нестатические элементы.
4. **Доступ к элементам внешнего класса**:
    - `static nested class` может обращаться к статическим элементам внешнего класса, включая private элементы.
5. **Доступ внешнего класса к элементам вложенного класса**:
    - Внешний класс может обращаться ко всем элементам `static nested class`, включая private элементы.


Пример 1: Основные концепции
```java
public class OuterClass {
    private static int staticOuterField = 10;
    private int nonStaticOuterField = 20;

    static class StaticNestedClass {
        private static int staticNestedField = 30;
        private int nonStaticNestedField = 40;

        void display() {
            // Доступ к статическим членам внешнего класса
            System.out.println("Static Outer Field: " + staticOuterField);

            // Доступ к своим членам
            System.out.println("Static Nested Field: " + staticNestedField);
            System.out.println("Non-static Nested Field: " + nonStaticNestedField);

            // Нельзя напрямую получить доступ к нестатическим членам внешнего класса
            // System.out.println("Non-static Outer Field: " + nonStaticOuterField); // Ошибка
        }

        static void staticDisplay() {
            // Доступ к статическим членам внешнего класса
            System.out.println("Static Outer Field (static method): " + staticOuterField);
        }
    }

    public void outerDisplay() {
        // Создание объекта static nested класса
        StaticNestedClass nestedObject = new StaticNestedClass();

        // Доступ к членам static nested класса
        System.out.println("Static Nested Field from Outer: " + StaticNestedClass.staticNestedField);
        System.out.println("Non-static Nested Field from Outer: " + nestedObject.nonStaticNestedField);
    }
}

public class Main {
    public static void main(String[] args) {
        // Создание объекта static nested класса
        OuterClass.StaticNestedClass nestedObject = new OuterClass.StaticNestedClass();
        nestedObject.display();

        // Вызов статического метода static nested класса
        OuterClass.StaticNestedClass.staticDisplay();

        // Вызов метода внешнего класса
        OuterClass outerObject = new OuterClass();
        outerObject.outerDisplay();
    }
}

```

Пример 2: Доступ к private элементам
```java
public class OuterClass {
    private static int staticOuterField = 10;
    private static int privateStaticOuterField = 50;

    static class StaticNestedClass {
        private static int staticNestedField = 30;
        private int nonStaticNestedField = 40;
        private int privateNestedField = 60;

        void display() {
            // Доступ к приватным статическим членам внешнего класса
            System.out.println("Private Static Outer Field: " + privateStaticOuterField);
        }

        static void staticDisplay() {
            // Доступ к приватным статическим членам внешнего класса
            System.out.println("Private Static Outer Field (static method): " + privateStaticOuterField);
        }
    }

    public void outerDisplay() {
        // Создание объекта static nested класса
        StaticNestedClass nestedObject = new StaticNestedClass();

        // Доступ к приватным членам static nested класса
        System.out.println("Private Nested Field from Outer: " + nestedObject.privateNestedField);
    }
}

public class Main {
    public static void main(String[] args) {
        // Создание объекта static nested класса
        OuterClass.StaticNestedClass nestedObject = new OuterClass.StaticNestedClass();
        nestedObject.display();

        // Вызов статического метода static nested класса
        OuterClass.StaticNestedClass.staticDisplay();

        // Вызов метода внешнего класса
        OuterClass outerObject = new OuterClass();
        outerObject.outerDisplay();
    }
}

```

### Заключение

`static nested class` в Java позволяет организовать классы, которые логически связаны между собой, в одной структуре. Такие классы имеют доступ ко всем статическим элементам внешнего класса и могут содержать как статические, так и нестатические члены. Внешний класс также имеет доступ ко всем элементам `static nested class`, включая private.