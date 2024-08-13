---
создал заметку: 2024-07-21
---
### Local inner класс

-  Local inner класс располагается в блоках кода таких, как, например, метод или конструктор
-  Local inner класс не может быть static
-  Область видимости local inner класса - это блок, в котором он находится
-  Local inner класс может обращаться даже к private элементам внешнего класса
-  Local inner класс может обращаться к элементам блока, в котором он написан при условии, что они final или effectively final


#### Основные положения:

1. **Расположение**:
    
    - Local inner класс располагается в блоках кода, таких как метод или конструктор.
2. **Статичность**:
    
    - Local inner класс не может быть `static`.
3. **Область видимости**:
    
    - Область видимости local inner класса — это блок, в котором он находится.
4. **Доступ к элементам внешнего класса**:
    
    - Local inner класс может обращаться даже к `private` элементам внешнего класса.
5. **Доступ к элементам блока**:
    
    - Local inner класс может обращаться к элементам блока, в котором он написан, при условии, что они `final` или `effectively final`.

#### Примеры:

##### Пример 1: Local Inner Класс внутри метода

```java
public class Calculator {
    private int result = 0;

    public void add(int a, int b) {
        // Local inner класс внутри метода add
        class Adder {
            public int addNumbers() {
                return a + b;
            }
        }

        Adder adder = new Adder();
        result = adder.addNumbers();
        System.out.println("Result: " + result);
    }

    public static void main(String[] args) {
        Calculator calculator = new Calculator();
        calculator.add(5, 3); // Output: Result: 8
    }
}
```

Пример 2: Доступ к элементам внешнего класса и блока
```java
public class Person {
    private String name = "Alice";

    public void greet(String greeting) {
        // greeting должно быть final или effectively final
        // final String greeting = "Hello";

        // Local inner класс внутри метода greet
        class Greeter {
            public void sayHello() {
                // Доступ к private элементу внешнего класса
                System.out.println(greeting + ", " + name);
            }
        }

        Greeter greeter = new Greeter();
        greeter.sayHello(); // Output: Hello, Alice
    }

    public static void main(String[] args) {
        Person person = new Person();
        person.greet("Hello");
    }
}
```

Пример 3: Local Inner Класс внутри конструктора

```java
public class MessagePrinter {
    private String message;

    public MessagePrinter(String initialMessage) {
        this.message = initialMessage;

        // Local inner класс внутри конструктора
        class Printer {
            public void printMessage() {
                // Доступ к private элементу внешнего класса
                System.out.println("Message: " + message);
            }
        }

        Printer printer = new Printer();
        printer.printMessage(); // Output: Message: Hello, World!
    }

    public static void main(String[] args) {
        new MessagePrinter("Hello, World!");
    }
}
```

### Заключение

Local inner классы в Java полезны для инкапсуляции логики, которая будет использована только в конкретном методе или блоке. Они имеют доступ ко всем членам внешнего класса, включая `private` элементы, а также могут обращаться к переменным из блока, в котором они объявлены, если эти переменные являются `final` или `effectively final`.

Final - означает что переменную нельзя изменить.

Effectivele final - означает что переменная может быть не объявлена final, но меняться она тоже не должна. То есть можно сказать что ее значение также неизменно.

