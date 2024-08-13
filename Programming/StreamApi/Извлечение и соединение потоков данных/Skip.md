---
создал заметку: 2024-07-27
---
Извлечение и соединение потоков данных

В результате вызова поток.skip(n) происходит совершенно противоположное:
отбрасываются n элементов.
Избавиться от нежелательной первой пустой строки можно так:

```java
public static void main(String[] args) {  
    // Считываем данные из файла  
    byte[] bytes = File.readAllBytes(Path.get(PATH_TO_DEMO_TXT_FILE));  
  
    // Преобразуем текст из файла в текстовую строку  
    String contents = new String(bytes, StandardCharsets.UTF_8);  
  
    //Выделяем из всего файла - список слов и пропукс первой пустой строки   
Stream.of(contents.split("\\PL+")).skip(1);  
}
```

