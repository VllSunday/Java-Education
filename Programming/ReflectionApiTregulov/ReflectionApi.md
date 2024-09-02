---
создал заметку: 2024-09-01
tags:
---
<font color="#938953">Рефлексия</font> - это механизм исследования данных о
программе во время её выполнения.
Рефлексия позволяет исследовать информацию о
полях, методах, конструкторах и других
составляющих классов

### Класс Class 
Java предоставляет класс который называется Class. Экземпляры класса класс, представляют классы и интерфейсы в работающем приложении Java. Примитивные типы в Java также представлены в виде в виде объектов Class. То есть получается у нас есть классы такие как 
"Car" "Student" "String" "HashMap" и тд, и они все являются объектами класса Class. Чтобы исследовать информацию о классе, нам нужен объект который эту информацию содержит.
Таким объектом является объект класса Class  


## Практика

##### Как мы можем создать вот этот объект класса Class? 

Рассмотрим 3 варианта

##### 1) Возможно самый популярный метод, для создания

```java
Class employeeClass = Class.forName("ReflectionEx.Employee");
```

Мы создаем объект ==employeeClass== типа Class. У класса Class есть метод forName, в аргументах которого, нужно написать с каким классом мы хотим работать.  

Самое главное что в метод forName нужно указать полный путь до класса.

То есть что получается? Объектом класса Class является класс Employee.

##### Второй способ


```java
Class employeeClassa = Employee.class;
```
Это второй вариант создания

##### Третий способ


```java
Employee emp = new Employee();  
Class employeeClass3 = emp.getClass();
```

То есть мы можем на объекте какого то класса вызвать метод getClass() который и будет служить в роль объекта класса Class 



### Работа с полями, методами и конструкторе

Твой пример кода наглядно демонстрирует, как использовать Reflection API в Java для работы с полями и методами классов. Давай разберем его по частям, чтобы лучше понять, что происходит на каждом этапе.

### 1. Получение класса через `Class.forName`
```java
Class employeeClass = Class.forName("ReflectionEx.Employee");
```
- Этот код загружает класс `Employee`, указанный по его полному имени (включая пакет), и создает объект `Class`. Через этот объект можно получить доступ к метаданным о классе, включая его поля, методы, конструкторы и другие элементы.

### 2. Получение и работа с полями
#### Получение конкретного поля
```java
Field someField = employeeClass.getField("id");
System.out.println("Type of id field = " + someField.getType());
```
- Здесь метод `getField` используется для получения публичного поля `id` класса `Employee`. Затем через метод `getType` определяется тип этого поля. Этот метод возвращает тип поля в виде объекта `Class`.

#### Получение всех публичных полей
```java
Field[] fields = employeeClass.getFields();
for (Field field : fields) {
    System.out.println("Type of " + field.getName() + " = " + field.getType());
}
```
- Метод `getFields` возвращает массив всех публичных полей класса, включая унаследованные поля. В цикле выводится имя и тип каждого поля.

#### Получение всех полей, включая приватные
```java
Field[] allField = employeeClass.getDeclaredFields();
for (Field field : allField) {
    System.out.println("Type of " + field.getName() + " = " + field.getType());
}
```
- `getDeclaredFields` возвращает массив всех полей класса, включая приватные, защищенные и пакетные поля, но не включает унаследованные поля. Это полезно для работы с полями, которые не являются публичными.

### 3. Работа с методами
#### Получение информации о конкретном методе
```java
Method method = employeeClass.getMethod("increaseSalary");
System.out.println("Return type of method increaseSalary "
        + method.getReturnType() + ", " + " parametrs types" + Arrays.toString(method.getParameterTypes()));
```
- Метод `getMethod` позволяет получить публичный метод по его имени и параметрам. В примере берется метод `increaseSalary`, и выводится его возвращаемый тип и типы параметров.

#### Еще один пример с методом
```java
Method method2 = employeeClass.getMethod("setSalary", double.class);
System.out.println("Return type of method setSalary "
        + method2.getReturnType() + ", " + " parametrs types" + Arrays.toString(method2.getParameterTypes()));
```
- Здесь ищется метод `setSalary`, принимающий параметр типа `double`. Аналогично, выводится информация о типе возвращаемого значения и типах параметров.

#### Получение всех методов класса
```java
Method[] methods = employeeClass.getMethods();
for (Method m : methods) {
    System.out.println("Name of methods "
            + m.getName() + ", "
            + " return type"
            + Arrays.toString(m.getParameterTypes()));
}
```
- Метод `getMethods` возвращает массив всех публичных методов класса, включая унаследованные. В цикле выводится имя каждого метода и его параметры.

#### Получение всех методов, включая приватные
```java
Method[] allMethods = employeeClass.getDeclaredMethods();
for (Method m : allMethods) {
    System.out.println("Name of methods "
            + m.getName() + ", "
            + " return type"
            + Arrays.toString(m.getParameterTypes()));
}
```
- `getDeclaredMethods` возвращает массив всех методов класса, включая приватные и защищенные. Эти методы также включают методы, которые не являются унаследованными.

#### Получение только публичных методов
```java
Method[] allPublicMethods = employeeClass.getDeclaredMethods();
for (Method m : allPublicMethods) {
    if (Modifier.isPublic(m.getModifiers())) {
        System.out.println("Name of methods "
                + m.getName() + ", "
                + " return type"
                + Arrays.toString(m.getParameterTypes()));
    }
}
```
- Здесь в массиве методов отфильтровываются только публичные методы с использованием метода `Modifier.isPublic`, который проверяет, является ли метод публичным.


### Работа с конструкторами через Reflection API

#### Получение и работа с конструкторами

Конструкторы в Java позволяют создавать экземпляры классов. Через Reflection API можно получать информацию о конструкторах, вызывать их, а также создавать новые экземпляры объектов.

##### Получение конструктора без параметров


```java
 Constructor constructor1 = employeeClass.getConstructor();
System.out.println("Constructor has " + constructor1.getParameterCount() +
        " parameters, their types are: " + Arrays.toString(constructor1.getParameterTypes()));
```

- Метод `getConstructor()` используется для получения публичного конструктора без параметров. В данном примере выводится количество параметров конструктора (должно быть 0) и их типы.

##### Получение конструктора с параметрами


```java
Constructor constructor2 = employeeClass.getConstructor(int.class, String.class, String.class);
System.out.println("Constructor has " + constructor2.getParameterCount() +
        " parameters, their types are: " + Arrays.toString(constructor2.getParameterTypes()));
```


- Здесь используется метод `getConstructor(Class<?>... parameterTypes)` для получения конструктора с определенными типами параметров (в данном случае `int`, `String`, `String`). В выводе показывается количество параметров конструктора и их типы.

##### Получение всех публичных конструкторов


```java
Constructor[] constructors = employeeClass.getConstructors();
for (Constructor c : constructors) {
    System.out.println("Constructor " + c.getName() + " has " + c.getParameterCount() +
            " parameters, their types are: " + Arrays.toString(c.getParameterTypes()));
}
```


- Метод `getConstructors()` возвращает массив всех публичных конструкторов класса. В цикле для каждого конструктора выводится его имя, количество параметров и типы этих параметров.

#### Получение всех конструкторов, включая приватные

Если необходимо получить все конструкторы, включая приватные и защищенные, нужно использовать метод `getDeclaredConstructors()`:


```java
Constructor[] allConstructors = employeeClass.getDeclaredConstructors();
for (Constructor c : allConstructors) {
    System.out.println("Constructor " + c.getName() + " has " + c.getParameterCount() +
            " parameters, their types are: " + Arrays.toString(c.getParameterTypes()));
}
```


- Этот метод возвращает массив всех конструкторов класса, включая приватные. Это полезно, когда нужно создать объект с использованием приватного конструктора.

### Создание объектов через Reflection

После получения конструктора, можно создать новый объект, используя метод `newInstance()`:


```java
Constructor constructor = employeeClass.getConstructor(int.class, String.class, String.class);
Object employeeInstance = constructor.newInstance(1, "John", "Doe");
```

- В данном примере создается новый объект класса `Employee` с помощью конструктора, принимающего три параметра (`int`, `String`, `String`). Метод `newInstance()` создает и возвращает экземпляр объекта.

### Важные моменты при работе с Reflection API

1. **Безопасность:** Reflection может нарушать инкапсуляцию, позволяя доступ к приватным полям и методам, что может привести к непредсказуемым результатам и проблемам безопасности.
    
2. **Производительность:** Использование Reflection может быть медленнее, чем обычный вызов методов и доступ к полям, так как оно связано с дополнительными накладными расходами.
    
3. **Обработка исключений:** Большинство методов Reflection бросают проверяемые исключения, такие как `NoSuchMethodException`, `IllegalAccessException`, `InvocationTargetException`. Эти исключения нужно обрабатывать или явно указывать в сигнатуре метода.
    
4. **Проверка типов:** Так как большинство операций Reflection возвращают объекты типа `Object`, необходимо явное приведение типов при работе с возвращаемыми значениями.
    

### Заключение

Reflection API в Java предоставляет мощные средства для динамической работы с классами, полями, методами и конструкторами. Несмотря на свою гибкость, его использование должно быть осторожным и обдуманным, учитывая возможные проблемы с производительностью и безопасностью. В твоем примере показана работа с полями, методами и конструкторами, что помогает глубже понять, как использовать Reflection для анализа и манипуляции классами во время выполнения программы.


### Что мы можем сделать с помощью рефлексии чего мы не сможем сделать без нее ? 



```java
public class Calculator {  
    void sum(int a, int b) {  
        int result = a + b;  
        System.out.println("Сумма " + a + " и " + b  +" равна " + result);  
    }  
  
    void subtraction(int a, int b) {  
        int result = a - b;  
        System.out.println("Разница " + a + " и " + b  +" равна " + result);  
    }  
  
    void multiplication(int a, int b) {  
        int result = a * b;  
        System.out.println("Произведение " + a + " и " + b  +" равна " + result);  
    }  
  
    void division(int a, int b) {  
        int result = a / b;  
        System.out.println("Частное " + a + " и " + b  +" равна " + result);  
    }  
}  
  
class TestCalculator {  
    public static void main(String[] args) {  
        try (BufferedReader reader = new BufferedReader(new FileReader("test100.txt"))){  
  
            String methodName = reader.readLine();  
            String firstArgument = reader.readLine();  
            String secondArgument = reader.readLine();  
  
            Calculator calculator = new Calculator();  
            Class cl = calculator.getClass();  
            Method method = null;  
  
            Method[] methods = cl.getDeclaredMethods();  
            for (Method m : methods) {  
                if (m.getName().equals(methodName)) {  
                    method = m;  
                }  
            }  
  
            method.invoke(calculator, Integer.parseInt(firstArgument), Integer.parseInt(secondArgument));  
  
        } catch (FileNotFoundException e) {  
            throw new RuntimeException(e);  
        } catch (IOException e) {  
            throw new RuntimeException(e);  
        } catch (InvocationTargetException e) {  
            throw new RuntimeException(e);  
        } catch (IllegalAccessException e) {  
            throw new RuntimeException(e);  
        }  
    }  
}
```

### Объяснение задания и использование Reflection API

#### Задача
Задание заключается в создании программы, которая выполняет одну из арифметических операций (`sum`, `subtraction`, `multiplication`, `division`) на основании данных, прочитанных из файла (`test100.txt`). В файле указано название метода, который нужно вызвать, и два аргумента для этого метода.

#### Почему это задание нельзя выполнить без Reflection API?
Здесь задача состоит в том, чтобы на основании строки (названия метода) выбрать соответствующий метод класса и вызвать его. Без использования Reflection API это невозможно сделать динамически, так как методы должны быть известны на этапе компиляции. С помощью Reflection программа может динамически определять и вызывать метод, основываясь на строковом значении имени метода, что и реализовано в данном коде.

#### Что происходит в задании?
1. **Чтение данных из файла:**
   - Программа открывает файл `test100.txt` и считывает из него три строки: название метода, первый аргумент, и второй аргумент.

2. **Создание экземпляра класса `Calculator`:**
   - Программа создает объект `Calculator`, который содержит методы для арифметических операций.

3. **Получение информации о классе через Reflection:**
   - С помощью метода `getClass()` программа получает объект `Class`, представляющий класс `Calculator`. Этот объект содержит информацию обо всех методах класса.

4. **Поиск метода по имени:**
   - Программа перебирает все методы класса `Calculator`, используя `getDeclaredMethods()`. Если название метода совпадает с прочитанным из файла (`methodName`), программа сохраняет ссылку на этот метод.

5. **Вызов метода через Reflection:**
   - Используя метод `invoke()`, программа вызывает найденный метод, передавая ему аргументы, которые также были прочитаны из файла и преобразованы из строки в тип `int`.

6. **Обработка исключений:**
   - Программа обрабатывает возможные исключения, такие как отсутствие файла, ошибка ввода-вывода, неправильный доступ к методу или ошибка в вызове метода.

#### Пример выполнения
Если содержимое файла `test100.txt`:
```
division
13
6
```
То программа выполнит следующие шаги:
- Считает из файла название метода `division`.
- Найдет метод `division` в классе `Calculator`.
- Вызовет этот метод с аргументами `13` и `6`.
- Выведет на экран результат: `Частное 13 и 6 равна 2`.

### Заключение
В этом задании использование Reflection API позволяет динамически выбирать и вызывать методы на основе входных данных. Это мощный инструмент, который может быть полезен в случаях, когда имена методов неизвестны на этапе компиляции, но становятся доступны на этапе выполнения программы.
