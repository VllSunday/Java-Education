---
создал заметку: 2024-07-27
---
### Другие потоковые преобразования

Метод distinct() возвращает поток данных, получающий свои
элементы из исходного потока данных в том же самом порядке,
за исключением того, что дубликаты в нем подавляются.
Пример:

```java
public static void main(String[] args) {
    final Stream<String> distinct = Stream.of("Hello", "Hello", "Hello", "world").distinct();
    
    System.out.println(distinct.count()); // 2
}
```

