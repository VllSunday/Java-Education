---
создал заметку: 2024-07-23
---
Лямбда выражение появились в 8 версии Java.

В теле лямбда-выражения мы создаем код, который будет выполнятся при вызове абстрактного метода.


----------------------------------------------------------

![[MainExampleOfLambda.png]]

`Lambda `выражение  - это просто способ передать какой то кусок кода в метод, или же они позволяют нам обойтись без использования ==анонимных классов==.

#### Важный момент: 
---


Если лямбда выражение присваивается значением интерфейсной переменной, а интерфейс функциональный. То в результате такой операции, автоматически создается объект. Ссылка на который записывается в интерфейсную переменную. 
Объект создается на основе анонимного класса, который реализует данный функциональный интерфейс. 
А этот единственный метод из интерфейса, он как раз токи реализуется а соответствии с лямбда выражением. Которая присвоена интерфейсной переменной.

То есть структура лямбда выражений, количество , тип аргумента, а так же тип возвращаемого значения должно соответствовать сигнатуре абстрактного метода функционального интерфейса.  

---







`Lambda` выражения - не привносят не какие изменения, или функционала в Java. Это просто способ делать уже привычный действия (создание анонимных классов) еще быстрее.

```java
Вот пример: 
public static void main(String[] args) {  
    Thread thread = new Thread(new Runnable() {  
        @Override  
        public void run() {  
            System.out.println("Hello");  
        }  
    });  
}
```
здесь у нас есть анонимный класс, который реализует интерфейс Runnable, и переопределяет его методы.
Но этот же код, можно повторить используя более лаконичный подход. 


```java
public static void main(String[] args) {  
    Thread thread = new Thread(() -> System.out.println("Hello"));  
}
```

Это то же самый код, только написан с использованием `Lambda`

Важно так же упомянуть, что лямбда в первую очередь это *==анонимная функция==* (метод), то есть не имеет ==название==.

```Java 
Подробнее про синтаксис: 
() -> System.out.println("Hello");

() - в этих скобках передаются аргументы. Если аргумент 1, то можно не ставить скобки.

-> синтаксис который указывает на тело функции / метода

System.out.println("Hello") - тело функции / метода
```


## Пример

Сейчас мы рассмотрим 3 способа, передать в качестве аргумента  реализацию интерфейса  Executable.
 
У нас есть вот такой код: 
```java
interface Executable{  
    void execute();  
}  
  
class Runner {  
    public void run(Executable e){  
  
    }}  
  
public class Main {  
    public static void main(String[] args) {  
        Runner runner = new Runner();  
        runner.run();  
    }  
}
```

1) Можно создать класс реализует интерфейс Executable 

```java
class ExecutableImplementation implements Executable{  
    @Override  
    public void execute() {  
        System.out.println("Hello");  
    }  
}  
  
public class Main {  
    public static void main(String[] args) {  
        Runner runner = new Runner();  
        runner.run(new ExecutableImplementation());  
    }  
}
```
То есть мы создали отдельный класс который реализует интерфейс Executable, и передали его в качестве аргумента в метод run();

Это не слишком удобно и быстро. Поэтому в Java есть анонимные классы.

2) Можно сделать с помощью анонимных классов
```java
runner.run(new Executable() {  
    @Override  
    public void execute() {  
        System.out.println("Hello");  
    }  
});
```

Можно добиться аналогичного результата если использовать анонимный класс. Получается в разы меньше кода, а эффект тот же самый.
Но это не самый удобный способ передать в аргумент реализацию интерфейса Executable.

Дело в том что интерфейс [[Executable]] - это функциональный интерфейс(то есть содержит только один абстрактный метод)
И в случаях когда мы имеем дело с ==функциональными интерфейсами==, и где мы можем применить анонимный класс.
Мы также можем применить и ==Lamda exprestions==

3) Использовать Lambda выражения: 
```java
runner.run(() -> System.out.println("Hello"));
```

Важно еще сказать что если мы использует Lambda, то значит мы имеем дело с функциональным интерфейсом. То есть нам не нужно создавать отдельный класс или анонимный класс для все одного метода.

По этому мы даже название метода мы не пишем. 
А просто использует максимально простую и емкую реализацию.

### Что в итоге получилось
У нас есть 3 реализации: 

```java 
interface Executable{  
    void execute();  
}  
  
class Runner {  
    public void run(Executable e){  
	  e.execute();
    }}  
  
class ExecutableImplementation implements Executable{  
    @Override  
    public void execute() {  
        System.out.println("Hello");  
    }  
}  
  
public class Main {  
    public static void main(String[] args) {  
        Runner runner = new Runner();  
    
        runner.run(new ExecutableImplementation()); // 1
         
        runner.run(new Executable() {  // 2
            @Override  
            public void execute() {  
                System.out.println("Hello");  
            }  
        });  
  
        runner.run(() -> System.out.println("Hello")); // 3  
    }  
}
```


И все 3 случаю приводят нас к одной вещи.

class Runner {  
    public void run(Executable e){  
	e.execute();
}} 
Вот здесь мы получим объект реализующий интерфейс Executable
И сделали мы это все 3-мя разными путями.




## Вариации Лямбда выражений 

- Если у нас в Лямбда выражении всего одна строка, то можно не писать фигурные скобки
```java
runner.run(() -> System.out.println("Hello"));
```
если же строчек кода несколько, то мы должны писать фигурные скобки, и после каждой строчки кода мы должны будет ставить точку с запятой `;`
```java
runner.run(() -> {
	System.out.println("Hello");
	System.out.println("Wolrld");
	});
```

- Также сигнатуры методов могут меняться.
-Например, теперь мы будет возвращать int 
```java
interface Executable{  
    int execute();  
}  
  
class Runner {  
    public void run(Executable e){  
	  int a = e.execute();
	  System.out.println(a);
    }}
```

Как будет выглядеть наша Лямбда функция: 
```java
runner.run(() -> {
	System.out.println("Hello");
	System.out.println("World");
	return 5;
	//Будет выведенно 
	// Hello
	// World
	// 5
	});
```
То есть мы можем возвращать любое значение которое прописано в сигнатуре метода `execute()`

Удобство лямбды в том что мы не указывает тип который  мы возвращаем, Java сама знает какой тип мы возвращаем. 
Потому что это лямбда реализует метод `int execute` 

И Java просто сопоставляет  эту сигнатуру а нашу лямбду 

Так же это код можно сократить, если нам будет нужно просто вернуть какое то число: 

```java
runner.run(() -> 1);
```
То есть из за того что у нас 1 строка, мы можем убрать фигурные скобки, так же мы можем убрать `key word` return, и просто написать само число.


- Так же в аргументах метода могут быть какие то параметры.
  ```java
  interface Executable{  
    int execute(int x);  
}  
```
В лямбда это будет выглядеть вот так: 
```java 
runner.run((int x) -> x + 5)
```
Но можно еще сократить эту запись: 
```java 
runner.run(x -> x + 5)
```
То есть, Java и так знает какие параметры мы передаем, и нам не нужно писать (int)
Так же если у нас всего 1 аргумент ,мы можем не ставить круглые скобки а просто написать имея переменной



## Важные моменты: 
Что если мы хотим получить доступ к переменной которая находиться вне лямбда выражения? Например: 
```java
int a = 5;

runner.run(() -> System.out.println(a + 5));
```

То есть мы спокойно можем получить к ней доступ. 
Но есть несколько условий:
- Переменная должна быть final или 
- Должна быть Effectively final (то есть не должна меняться по ходу программы)


##### Также важно сказать что нет своей собственной области видимости как у методов


Как пример возьмем вот этот код: 
```java
int a = 1;  
runner.run(new Executable() {  
    @Override  
    public int execute(int x) {  
        int a = 5;  
        System.out.println("Hello");  
        return 5;  
    }  
});
```

Что мы тут видим? - Что у нас есть несколько переменных вида `int a = ,,,`
Потому что они были созданы в разной области видимости.


Но если мы посмотрим на следующий пример: 
```java
public static void main(String[] args) {  
    Runner runner = new Runner();  
  
    int a = 1;  
    runner.run(new Executable() {  
        @Override  
        public int execute(int x) {  
            int a = 5;  
            System.out.println("Hello");  
            return 5;  
        }  
    });  
  
    runner.run(x -> {  
        int a = 1; // Ошибка: 
        // Variable 'a' is already defined in the scope  
        int result = x + a;  
        return result;  
    });  
}
```

Что мы тут видим? - Что переменная уже была создана, и мы не можем создать такую же переменную в этой области видимости!



## Реализация Comparator с использованием Lambda


```java
public static void main(String[] args) {  
    List<String> list = new ArrayList<>();  
  
    list.add("Hello");  
    list.add("add");  
    list.add("a");  
    list.add("Worlddd");  
  
    list.sort((s1, s2) ->  {  
        return s1.length() - s2.length();  
    });  
    System.out.println(list);  
}
```

Еще один момент связанный с Лямбда выражениями это то что мы можем записывать их переменный. То есть лямбда можно воспринимать как выражение: 
Вот пример: 

```java
Comparator<String> comparator =(s1, s2) ->  {  
    return s1.length() - s2.length();  
};  
list.sort(comparator);  
  
System.out.println(list);
```
