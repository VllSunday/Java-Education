---
создал заметку: 2024-07-21
---
[[Deque|LinkedList]]

Класс LinkedList имплементирует не только интерфейс
List, но и интерфейс Deque, который в свою очередь происходит от Queue

Наши очереди не ограниченны размером. Но есть очереди с ограниченным размером.

Методы: 
- add();
- offer();
- remove();
- poll


 Метод add();
 Добавляет элементы: 
```java
Queue<String> queue = new LinkedList<>();  
queue.add("Zaur");  
queue.add("Oleg");  
queue.add("Ivan");  
queue.add("Maria");  
queue.add("Alexandr");  

queue.add("Zhenya");
//Если очередь будет ограниченна 5 элементами, будет выбрашен Exeption
System.out.println(queue);
```
Как происходит добавление: 
1-м добавился Zaur
2-м после Zaur добавился Oleg
3-м после Oleg добавился Ivan и тд...


Когда мы добавляем в очередь методом add, и наша очередь ограниченна в размере (например 5) , при добавлении 6 элемента было бы выкинуто исключение. Потому что очередь уже полная.
Что бы избежать этого, нужно воспользоваться методом offer();


Метод offer();
```java
Queue<String> queue = new LinkedList<>();  
queue.offer("Zaur");  
queue.offer("Oleg");  
queue.offer("Ivan");  
queue.offer("Maria");  
queue.offer("Alexandr");  

queue.offer("Zhenya");
//Если очередь будет ограниченна 5 элементами, не будет выбрашен Exeption, мы просто не сможем добавить в очередь еще один элемент
System.out.println(queue);
```
Работает точно также как и add, только вот если наша очередь будет ограниченна в размере, исключение не будет выброшено. 
Мы бы просто не смогли бы добавить 6-й элемент


Метод remove();
Удаляет первый элемент в очереди: 
```Java
public static void main(String[] args) {  
    Queue<String> queue = new LinkedList<>();  
    queue.add("Zaur");  
    queue.add("Oleg");  
    queue.add("Ivan");  
    queue.add("Maria");  
    queue.add("Alexandr");  
    System.out.println(queue);  
    System.out.println(queue.remove());  
    System.out.println(queue.remove());  
    System.out.println(queue.remove());  
    System.out.println(queue.remove());  
    System.out.println(queue.remove());  
    System.out.println(queue.remove());  
}

/* 
[Zaur, Oleg, Ivan, Maria, Alexandr]
Zaur
Oleg
Ivan
Maria
Alexandr
Exception in thread "main" java.util.NoSuchElementException
*\
```
Вот что получается: мы удаляем элементы, которые у нас находятся первыми в очереди.
Если мы попытаемся удалить из очереди, а элементов в ней нет, будет выброшен Exсection: NoSuchElementException

Что бы избежать exeption мы можем использовать метод poll 


Метод poll();

```Java
public static void main(String[] args) {  
    Queue<String> queue = new LinkedList<>();  
    queue.add("Zaur");  
    queue.add("Oleg");  
    queue.add("Ivan");  
    queue.add("Maria");  
    queue.add("Alexandr");  
    System.out.println(queue);  
    System.out.println(queue.poll());  
    System.out.println(queue.poll());  
    System.out.println(queue.poll());  
    System.out.println(queue.poll());  
    System.out.println(queue.poll());  
    System.out.println(queue.poll());  
}

/* 
[Zaur, Oleg, Ivan, Maria, Alexandr]
Zaur
Oleg
Ivan
Maria
Alexandr
null
*\
```

Метод poll не выбросит не каких exсeptions, и в случае если объектов в массиве нет, он вернет ==null==



#### Метод element();
Метод element(); - показывает верхний элемент в очереди.

Если элемента в очереди нет, выброситься exception
NoSuchElementExeption

#### Метод peek();
Метод peek(); - показывает верхний элемент в очереди.

Если элемента в очереди нет, exception выброшен не будет.
Метод вернет ==null==


## Важная информация: 
Мы можем удалить элемент из середины очереди: 

```java
queue.remove("Ivan");
```

```java
public static void main(String[] args) {  
    Queue<String> queue = new LinkedList<>();  
    queue.add("Zaur");  
    queue.add("Oleg");  
    queue.add("Ivan");  
    queue.add("Maria");  
    queue.add("Alexandr");  
    System.out.println(queue);  
    queue.remove("Ivan");  
    System.out.println(queue);  
}

// [Zaur, Oleg, Ivan, Maria, Alexandr]
// [Zaur, Oleg, Maria, Alexandr]
```

Но лучше так не делать. Очередь предназначена для того что бы работать с первым элементом. Если нам часто приходиться удалять из середины, лучше воспользоваться другой очередью

