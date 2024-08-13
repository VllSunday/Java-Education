---
создал заметку: 2024-07-27
---
### Извлечение и соединение потоков данных

Два потока данных можно соединить вместе с помощью статического метода concat() из
интерфейса Stream(). Первый из этих потоков не должен быть бесконечным, иначе второй
поток не сможет соединиться с ним:

```java
public static void main(String[] args) {
    final Stream<String> combined = Stream.concat(letters("Hello"), letters("World"));
    // получается следующий поток данных ["H", "e", "l", "l", "o", "W", "o", "r", "l", "d"]
}

public static Stream<String> letters(String s) {
    List<String> result = new ArrayList<>();
    for (int i = 0; i < s.length(); i++) {
        result.add(s.substring(i, i + 1));
    }
    return result.stream();
}
```

