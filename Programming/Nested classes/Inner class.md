---
создал заметку: 2024-07-21
---
### Конспект: Inner Класс в Java

#### Основные положения:

1. **Ассоциация с внешним классом**:
    
    - Каждый объект `inner` класса всегда ассоциируется с объектом внешнего класса.
2. **Создание объектов**:
    
    - Создавая объект `inner` класса, нужно перед этим создать объект его внешнего класса.
3. **Содержимое**:
    
    - `Inner` класс может содержать только нестатические (`non-static`) элементы.
4. **Доступ к элементам внешнего класса**:
    
    - `Inner` класс может обращаться даже к `private` элементам внешнего класса.
5. **Доступ внешнего класса к элементам `inner` класса**:
    
    - Внешний класс может обращаться даже к `private` элементам `inner` класса, прежде создав его объект.


#### Примеры:

##### Пример 1: Создание и использование `inner` класса

```java
public class OuterClass {
    private int outerField = 10;

    class InnerClass {
        private int innerField = 20;

        void display() {
            // Доступ к членам внешнего класса
            System.out.println("Outer Field: " + outerField);

            // Доступ к своим членам
            System.out.println("Inner Field: " + innerField);
        }
    }

    public void outerDisplay() {
        // Создание объекта inner класса
        InnerClass innerObject = new InnerClass();

        // Доступ к членам inner класса
        System.out.println("Inner Field from Outer: " + innerObject.innerField);
    }
}

public class Main {
    public static void main(String[] args) {
        // Создание объекта внешнего класса
        OuterClass outerObject = new OuterClass();

        // Создание объекта inner класса через объект внешнего класса
        OuterClass.InnerClass innerObject = outerObject.new InnerClass();

        // Вызов метода inner класса
        innerObject.display();

        // Вызов метода внешнего класса
        outerObject.outerDisplay();
    }
}
```

Пример 2: Доступ к `private` элементам
```java
public class OuterClass {
    private int outerField = 10;
    private int privateOuterField = 50;

    class InnerClass {
        private int innerField = 20;
        private int privateInnerField = 60;

        void display() {
            // Доступ к приватным членам внешнего класса
            System.out.println("Private Outer Field: " + privateOuterField);
        }
    }

    public void outerDisplay() {
        // Создание объекта inner класса
        InnerClass innerObject = new InnerClass();

        // Доступ к приватным членам inner класса
        System.out.println("Private Inner Field from Outer: " + innerObject.privateInnerField);
    }
}

public class Main {
    public static void main(String[] args) {
        // Создание объекта внешнего класса
        OuterClass outerObject = new OuterClass();

        // Создание объекта inner класса через объект внешнего класса
        OuterClass.InnerClass innerObject = outerObject.new InnerClass();

        // Вызов метода inner класса
        innerObject.display();

        // Вызов метода внешнего класса
        outerObject.outerDisplay();
    }
}
```

### Заключение

`Inner` классы в Java позволяют организовать более тесную связь между классами и дают возможность `inner` классу обращаться к членам внешнего класса, включая `private` элементы. Для создания объекта `inner` класса сначала нужно создать объект внешнего класса, так как `inner` класс всегда ассоциируется с его экземпляром.