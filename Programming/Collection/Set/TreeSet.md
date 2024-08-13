---
создал заметку: 2024-07-17
---
[[Set|TreeSet]]

TreeSet хранит элементы в отсортированном по
возрастанию порядке. Используется [[красно-чёрное дерево|красно-черное дерево]]
В основе TreeSet лежит TreeMар. У элементов данного
TreeMap: ключи - это элементы TreeSet, значения - это
константа-заглушка.


```java
Set<Integer> treeSet = new TreeSet<>();  
        treeSet.add(1);  
        treeSet.add(2);  
        treeSet.add(5);  
        treeSet.add(3);  
        treeSet.add(4);  
//        treeSet.add(null);  
        System.out.println(treeSet);
    // Выведет: 
    // [1, 2, 3, 4, 5]
```

TreeSet так же как и TreeMap не может хранить в себе значение: ==null==


## Работа с кастомными классами

Так же как и TreeMap, TreeSet сравнивает объекты.
Делает он это через метод compareTo. Или же мы можем создать компаратор прямо в конструкторе TreeSet

Рассмотрим каждый из этих примеров: 
Если мы попытается сделать вот так: 
```java 
public class Main {  
    public static void main(String[] args) {  
        Student st1 = new Student("Zhenya" , 1);  
        Student st2 = new Student("Sasha" , 3);  
        Student st3 = new Student("Oleg" , 2);  
        Student st4 = new Student("Masha" , 5);  
        Student st5 = new Student("Nina" , 4);  
        Set<Student> treeSet = new TreeSet<>();  
        treeSet.add(st1);  
        treeSet.add(st2);  
        treeSet.add(st3);  
        treeSet.add(st4);  
        treeSet.add(st5);  
        System.out.println(treeSet);  
    }  
}  
  
class Student {  
    String name;  
    int course;  
  
    public Student(String name, int course) {  
        this.name = name;  
        this.course = course;  
    }  
  
    @Override  
    public String toString() {  
        return "Student{" +  
                "name='" + name + '\'' +  
                ", course=" + course +  
                '}';  
    }  
}
```
То мы получим: "ClassCastException". Потому что TreeSet не знает как именно ему сортировать TreeSet.
Значит нам нужно дать понять TreeSet как именно нужно сортировать Student
1-й способ: 

```java 
public class Main {  
    public static void main(String[] args) {  
        Student st1 = new Student("Zhenya" , 1);  
        Student st2 = new Student("Sasha" , 3);  
        Student st3 = new Student("Oleg" , 2);  
        Student st4 = new Student("Masha" , 5);  
        Student st5 = new Student("Nina" , 4);  
        Set<Student> treeSet = new TreeSet<>();  
        treeSet.add(st1);  
        treeSet.add(st2);  
        treeSet.add(st3);  
        treeSet.add(st4);  
        treeSet.add(st5);  
        System.out.println(treeSet);  
    }  
}  
  
class Student implements Comparable<Student>{  
    String name;  
    int course;  
  
    public Student(String name, int course) {  
        this.name = name;  
        this.course = course;  
    }  
  
    @Override  
    public String toString() {  
        return "Student{" +  
                "name='" + name + '\'' +  
                ", course=" + course +  
                '}';  
    }  
  
    @Override  
    public int compareTo(Student o) {  
        return this.course - o.course;  
    }  
}
```
Мы реализовали интерфейс Comparable, тем самым дали понять TreeSet как именно нужно сортировать наших студентов. 


2-й способ:
```java 
public class Main {  
    public static void main(String[] args) {  
        Student st1 = new Student("Zhenya" , 1);  
        Student st2 = new Student("Sasha" , 3);  
        Student st3 = new Student("Oleg" , 2);  
        Student st4 = new Student("Masha" , 5);  
        Student st5 = new Student("Nina" , 4);  
        Set<Student> treeSet = new TreeSet<>(new Comparator<Student>() {  
            @Override  
            public int compare(Student o1, Student o2) {  
                return o1.course - o2.course;  
            }  
        });  
        treeSet.add(st1);  
        treeSet.add(st2);  
        treeSet.add(st3);  
        treeSet.add(st4);  
        treeSet.add(st5);  
        System.out.println(treeSet);  
    }  
}  
  
class Student {  
    String name;  
    int course;  
  
    public Student(String name, int course) {  
        this.name = name;  
        this.course = course;  
    }  
  
    @Override  
    public String toString() {  
        return "Student{" +  
                "name='" + name + '\'' +  
                ", course=" + course +  
                '}';  
    }  
}
```

В этом способе мы в конструкторе сразу создали Comparator для Student, и также сравнили их по курсам.


## Методы TreeSet

1)  first();
2) last();
3) headSet();
4) tailSet();
5) subSet
   
   
Метод first(); вернет нам элемент который находится самым первым: 
```java 
public class Main {  
    public static void main(String[] args) {  
        Student st1 = new Student("Zhenya", 1);  
        Student st2 = new Student("Sasha", 3);  
        Student st3 = new Student("Oleg", 2);  
        Student st4 = new Student("Masha", 5);  
        Student st5 = new Student("Nina", 4);  
  
        TreeSet<Student> treeSet = new TreeSet<>(Comparator.comparingInt(o -> o.course));  
  
        treeSet.add(st1);  
        treeSet.add(st2);  
        treeSet.add(st3);  
        treeSet.add(st4);  
        treeSet.add(st5);  
  
        System.out.println(treeSet.first());  
    }  
}  
  
class Student {  
    String name;  
    int course;  
  
    Student(String name, int course) {  
        this.name = name;  
        this.course = course;  
    }  
  
    @Override  
    public String toString() {  
        return "Student{name='" + name + "', course=" + course + '}';  
    }  
}
//Выведет: Student{name='Zhenya', course=1}

```


Метод last(); поможет вывести последний элемент в нашем TreeSet

```java 
public static void main(String[] args) {  
    Student st1 = new Student("Zhenya", 1);  
    Student st2 = new Student("Sasha", 3);  
    Student st3 = new Student("Oleg", 2);  
    Student st4 = new Student("Masha", 5);  
    Student st5 = new Student("Nina", 4);  
  
    TreeSet<Student> treeSet = new TreeSet<>(Comparator.comparingInt(o -> o.course));  
  
    treeSet.add(st1);  
    treeSet.add(st2);  
    treeSet.add(st3);  
    treeSet.add(st4);  
    treeSet.add(st5);  
  
    System.out.println(treeSet.last());  
}
//Выведет: Student{name='Masha', course=5}
```

Метод headSet поможет вывести элементы которые находятся выше (не включительно)чем элемент по которому едет проверка

```java
public static void main(String[] args) {  
    Student st1 = new Student("Zhenya", 1);  
    Student st2 = new Student("Sasha", 3);  
    Student st3 = new Student("Oleg", 2);  
    Student st4 = new Student("Masha", 5);  
    Student st5 = new Student("Nina", 4);  
  
    TreeSet<Student> treeSet = new TreeSet<>(Comparator.comparingInt(o -> o.course));  
  
    treeSet.add(st1);  
    treeSet.add(st2);  
    treeSet.add(st3);  
    treeSet.add(st4);  
    treeSet.add(st5);  
    Student st6 = new Student("Nina", 3);  
  
    System.out.println(treeSet.headSet(st6));  
}

// Выведет: [Student{name='Zhenya', course=1}, Student{name='Oleg', course=2}]


```

Метод tailSet поможет вывести элементы которые находятся ниже(включительно) чем элемент по которому едет проверка

```java
public static void main(String[] args) {  
    Student st1 = new Student("Zhenya", 1);  
    Student st2 = new Student("Sasha", 3);  
    Student st3 = new Student("Oleg", 2);  
    Student st4 = new Student("Masha", 5);  
    Student st5 = new Student("Nina", 4);  
  
    TreeSet<Student> treeSet = new TreeSet<>(Comparator.comparingInt(o -> o.course));  
  
    treeSet.add(st1);  
    treeSet.add(st2);  
    treeSet.add(st3);  
    treeSet.add(st4);  
    treeSet.add(st5);  
    Student st6 = new Student("Nina", 3);  
  
    System.out.println(treeSet.tailSet(st6));  
}

// Выведет: [Student{name='Sasha', course=3}, Student{name='Nina', course=4}, Student{name='Masha', course=5}]

```


Метод subSet(); позволяет получить множество элементы которого находятся между двумя какими то значениями: 
```java
public static void main(String[] args) {  
    Student st1 = new Student("Zhenya", 1);  
    Student st2 = new Student("Sasha", 3);  
    Student st3 = new Student("Oleg", 2);  
    Student st4 = new Student("Masha", 5);  
    Student st5 = new Student("Nina", 4);  
  
    TreeSet<Student> treeSet = new TreeSet<>(Comparator.comparingInt(o -> o.course));  
  
    treeSet.add(st1);  
    treeSet.add(st2);  
    treeSet.add(st3);  
    treeSet.add(st4);  
    treeSet.add(st5);  
    Student st6 = new Student("Oleg", 3);  
    Student st7 = new Student("Ivan", 5);  
  
    System.out.println(treeSet.subSet(st6 , st7));  
}
// Выведется: [Student{name='Sasha', course=3}, Student{name='Nina', course=4}]

```

```java 
    Student st6 = new Student("Oleg", 3);  
	Student st7 = new Student("Ivan", 5);  
  
    System.out.println(treeSet.subSet(st6 , st7));    
```
Вот эта строчка означает:  >= 3 || 5 <
То есть 1 значение включительно, 2 значение исключительно



# Важное правило: 

Если 
```java
a.equals(b); -> true;
a.compareTo(b) -> 0;
```

То есть если мы переопределяем метод equals ( а мы должны всегда это сделать )
Если у нас а.equals(b) -> true;
То метод a.compareTo(b) должен вернуть ноль -> 0; (то есть должен быть равно)


