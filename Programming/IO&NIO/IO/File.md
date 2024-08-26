---
создал заметку: 2024-08-17
---
Класс File позволяет управлять информацией о файлах и директориях.

Класс File не работает на прямую с потоками, его задачей является, управление о файлах и каталогах

```java
File file = new File("test1.txt");
```

Основные методы: 

- getAbsolutePath
- isAbsolute
- isDirectory
- exists
- createNewFile
- mkdir
- length
- delete
- listFiles
- isHidden
- canRead
- canWrite
- canExecute


