---
создал заметку: 2024-07-25
---
### Function <`T,R`>

Что делает этот интерфейс? 
Представьте себе что перед нами стоит задача, сделать метод который будет считать средне - арифметическое каких то значений.

Например у студентов есть: `Курс` `Возраст` `Баллы` 

И нам нужно написать какой то уникальный метод который будет на вход принимать класс `Student` а на выходе например выдавать `double`
Для этого нам не пойдут интерфейсы: `Predicate` `Supplier` `Consumer`
Но может подойти интерфейс `Function`

#### Метод интерфейса Function<`T,R`>
##### R apply (T t)
То есть у нас есть 2 параметра
T - это входной параметр ( То есть здесь мы будем передавать Student)
R - это параметр который мы будем возвращать (То есть наш double)


```java
public static void main(String[] args) {  
  
    List<Student> listS = new ArrayList<>();  
  
    listS.add(new Student("Sasha", 7.6));  
    listS.add(new Student("Maria", 8.9));  
    listS.add(new Student("Tom", 8.3));  
  
    double res = avgOfSmth(listS, stud -> stud.avgGrade);  
    System.out.println(res);  
}  
  
private static double avgOfSmth(List<Student> list, Function<Student, Double> f){  
    double result = 0;  
    for (Student st : list){  
        result += f.apply(st);  
    }  
    result = result / list.size();  
    return result;  
}
```
Вот что у нас получилось.
Что происходит в этом коде: 
1) Мы создаем метод `avgOfSmth`, который принимает на вход список студентов и объект типа `Function<Student, Double>`.
2) Затем реализуем вычисление среднего арифметического.
3) Важно понимать, что происходит: мы вызываем метод `apply` на объекте типа `Function`, который принимает объект типа `Student` и возвращает значение типа `Double`. В каждой итерации цикла мы передаем студента, и результат метода `apply` добавляется к переменной `result`.
4) Далее мы создаем переменную `res`, которой присваиваем результат вызова метода `avgOfSmth`.
5) Метод `avgOfSmth` принимает список студентов и лямбда-выражение, которое определяет, что возвращать для каждого студента. Лямбда-выражение `stud -> stud.avgGrade` говорит, что для каждого студента нужно возвращать значение его средней оценки (`avgGrade`).
6) Метод `avgOfSmth` суммирует значения, возвращенные лямбда-выражением, и делит на количество студентов, чтобы получить среднее значение. Результат сохраняется в переменную `res`, которая затем выводится.


Более подробно: 
```java
public static void main(String[] args) {  
  
    List<Student> listS = new ArrayList<>();  
  
    listS.add(new Student("Sasha", 7.6));  
    listS.add(new Student("Maria", 8.9));  
    listS.add(new Student("Tom", 8.3));  
  
    double res = avgOfSmth(listS, stud -> stud.avgGrade);  
    System.out.println(res);  
}  
  
private static double avgOfSmth(List<Student> list, Function<Student, Double> f){  
    double result = 0;  
    for (Student st : list){  
        result += f.apply(st);  
    }  
    result = result / list.size();  
    return result;  
}
```

### Объяснение кода:

1. **Метод `avgOfSmth`**:
    
    - Принимает список студентов и объект типа `Function<Student, Double>`.
    - Вычисляет среднее значение некоторого свойства студентов.
2. **Реализация среднего арифметического**:
    
    - С помощью цикла проходит по всем студентам в списке.
    - Вызывает метод `apply` объекта `Function`, который возвращает значение типа `Double` для каждого студента.
    - Суммирует полученные значения и делит на количество студентов для получения среднего.
3. **Подробное объяснение**:
    
    - Важно понимать, что метод `apply` вызывается на объекте типа `Function`. Этот метод принимает на вход объект типа `Student` и возвращает значение типа `Double`.
    - В цикле для каждого студента из списка вызывается метод `apply`, и возвращаемое значение добавляется к `result`.
4. **Создание переменной `res`**:
    
    - Переменной `res` присваивается результат вызова метода `avgOfSmth`.
    - Метод `avgOfSmth` принимает список студентов и лямбда-выражение, которое определяет, что возвращать для каждого студента.
5. **Описание нахождения среднего арифметического**:
    
    - Метод `avgOfSmth` принимает список и лямбда-выражение, которое указывает, какое свойство студентов использовать для вычисления.
    - Лямбда-выражение в примере `stud -> stud.avgGrade` говорит, что для каждого студента нужно возвращать значение его средней оценки (`avgGrade`).
    - Метод `avgOfSmth` суммирует значения, возвращенные лямбда-выражением, и делит на количество студентов, чтобы получить среднее значение.
    - Результат сохраняется в переменную `res`, которая затем выводится.




#### **Добавление более сложного примера**:

```java
public static void main(String[] args) {

    List<Student> listS = new ArrayList<>();

    listS.add(new Student("Sasha", 7.6));
    listS.add(new Student("Maria", 8.9));
    listS.add(new Student("Tom", 8.3));

    double avgGrade = avgOfSmth(listS, stud -> stud.avgGrade);
    System.out.println("Average Grade: " + avgGrade);

    double avgNameLength = avgOfSmth(listS, stud -> (double) stud.name.length());
    System.out.println("Average Name Length: " + avgNameLength);
}

private static double avgOfSmth(List<Student> list, Function<Student, Double> f) {
    double result = 0;
    for (Student st : list) {
        result += f.apply(st);
    }
    result = result / list.size();
    return result;
}

class Student {
    String name;
    double avgGrade;

    public Student(String name, double avgGrade) {
        this.name = name;
        this.avgGrade = avgGrade;
    }

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", avgGrade=" + avgGrade +
                '}';
    }
}
```




