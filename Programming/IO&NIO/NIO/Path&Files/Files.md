---
создал заметку: 2024-08-18
---

# Работа с классом Files в Java
## Класс Files

### Определение
Класс `Files` — это утилитарный класс, который содержит статические методы для работы с файлами и директориями. Он предоставляет методы для чтения, записи, создания, удаления файлов и получения информации о них.

### Методы Files:

- **exists(Path path)**  
Проверяет, существует ли файл или директория по указанному пути.

  **Пример:**
  ```java
  if (!Files.exists(filePath)) {
      Files.createFile(filePath);  // Создает файл, если он не существует
  }
  ```

- **createFile(Path path)**  
Создает новый файл по указанному пути. Если файл уже существует, будет выброшено исключение `FileAlreadyExistsException`.

  **Пример:**
  ```java
  Files.createFile(filePath);  // Создает новый файл
  ```

- **createDirectory(Path path)**  
Создает новую директорию по указанному пути. Если директория уже существует, будет выброшено исключение `FileAlreadyExistsException`.

  **Пример:**
  ```java
  Files.createDirectory(directoryPath);  // Создает новую директорию
  ```

- **isReadable(Path path), isWritable(Path path), isExecutable(Path path)**  
Проверяют, доступен ли файл для чтения, записи или выполнения соответственно.

  **Пример:**
  ```java
  System.out.println(Files.isReadable(filePath));  // Проверка на возможность чтения файла
  ```

- **isSameFile(Path path1, Path path2)**  
Проверяет, ссылаются ли два пути на один и тот же файл.

  **Пример:**
  ```java
  System.out.println(Files.isSameFile(filePath2, filePath));  // Проверка на совпадение файлов
  ```

- **size(Path path)**  
Возвращает размер файла в байтах.

  **Пример:**
  ```java
  System.out.println(Files.size(filePath));  // Получение размера файла
  ```

- **getAttribute(Path path, String attribute)**  
Возвращает значение указанного атрибута файла. Например, `creationTime` для получения времени создания файла.

  **Пример:**
  ```java
  System.out.println(Files.getAttribute(filePath, "creationTime"));  // Время создания файла
  ```

- **readAttributes(Path path, String attributes)**  
Возвращает набор атрибутов для файла в виде `Map`, где ключ — название атрибута, а значение — его значение.

  **Пример:**
  ```java
  Map<String, Object> attributes = Files.readAttributes(filePath, "*");
  for (Map.Entry<String, Object> entry : attributes.entrySet()) {
      System.out.println(entry.getKey() + " | " + entry.getValue());
  }
  ```

### getAttribute() vs. readAttributes()\
`getAttribute()` возвращает значение одного конкретного атрибута, тогда как `readAttributes()` возвращает сразу все атрибуты файла в виде карты.

---

## Работа с методами `copy()`, `move()`, `delete()` в Java

### Основные методы класса `Files`

1. **Метод `copy()`**:
    - Метод `copy()` используется для копирования файлов или каталогов.
    - Если файл или каталог с таким именем уже существует в целевом месте, то будет выброшено исключение.
    - Чтобы избежать этого, можно использовать опцию `StandardCopyOption.REPLACE_EXISTING`, которая позволяет заменить уже существующий файл.

    ```java
    Files.copy(filePath, directoryPath.resolve(filePath), StandardCopyOption.REPLACE_EXISTING);
    ```

    - Пример без замены существующего файла:

    ```java
    Files.copy(filePath, directoryPath.resolve("test15.txt"));
    ```

    - Копирование директории с вложенными файлами невозможно: метод `copy()` скопирует только пустую папку.

2. **Метод `move()`**:
    - Метод `move()` используется для перемещения файлов или каталогов.
    - Если файл перемещен, его больше не будет в исходной директории, и при повторной попытке перемещения будет выброшено исключение.

    ```java
    Files.move(filePath, directoryPath.resolve("test15.txt"));
    ```

    - Метод `move()` также может использоваться для переименования файлов:

    ```java
    Files.move(Paths.get("test10.txt"), Paths.get("test11.txt"));
    ```

3. **Метод `delete()`**:
    - Метод `delete()` используется для удаления файлов или каталогов.
    - Нельзя удалить непустую директорию; сначала нужно удалить все файлы внутри неё.

    ```java
    Files.delete(Paths.get("test5.txt"));
    ```

    - Пример удаления директории:

    ```java
    Files.delete(directoryPath);
    ```

    Если директория не пуста, будет выброшено исключение.

### Особенности и исключения
- **Копирование директории с файлами**: Метод `copy()` не может копировать директорию с вложенными файлами. Чтобы скопировать такую директорию, необходимо скопировать папку и файлы внутри неё вручную.
- **Удаление непустых директорий**: Перед удалением директории с файлами нужно удалить все файлы, иначе метод `delete()` выбросит исключение.
---

## Методы `write()` и `readAllLines()` в Java

### Метод `write()`

1. **Назначение**:
    - Метод `write()` используется для записи данных в файл. 
    - Он позволяет записать строку или массив байтов в файл, перезаписывая его содержимое или добавляя новые данные в файл.

2. **Применение**:
    - В твоем коде пример создания файла и записи в него данных выглядит так:
    
    ```java
    Path filePath = Paths.get("test10.txt");
    Files.createFile(filePath);
    
    String dialog = "-Privet \n -Privet \n -Kak dela? \n -Normalno \n -Tvoi kak? ";
    Files.write(filePath, dialog.getBytes());
    ```
    
    - Здесь:
        - `Path filePath = Paths.get("test10.txt");` — создается объект `Path`, указывающий на файл `test10.txt`.
        - `Files.createFile(filePath);` — создается новый файл по указанному пути.
        - `Files.write(filePath, dialog.getBytes());` — строка `dialog` преобразуется в массив байтов с помощью `getBytes()` и записывается в файл.

3. **Особенности**:
    - Если файл уже существует, метод `write()` перезапишет его содержимое. 
    - В случае, если нужно добавить данные в существующий файл, можно использовать `StandardOpenOption.APPEND`:
    
    ```java
    Files.write(filePath, dialog.getBytes(), StandardOpenOption.APPEND);
    ```

### Метод `readAllLines()`

1. **Назначение**:
    - Метод `readAllLines()` используется для чтения всех строк из файла и возврата их в виде списка строк (`List<String>`).
    - Этот метод удобен для чтения текстовых файлов целиком.

2. **Применение**:
    - В твоем коде пример чтения строк из файла и их вывода на экран выглядит так:
    
    ```java
    List<String> strings = Files.readAllLines(filePath);
    for (String s : strings) {
        System.out.println(s + " ");
    }
    ```

    - Здесь:
        - `List<String> strings = Files.readAllLines(filePath);` — считываются все строки из файла `test10.txt` и сохраняются в список `strings`.
        - `for (String s : strings)` — цикл проходит по каждой строке из списка и выводит её на экран.

3. **Особенности**:
    - Метод `readAllLines()` читает весь файл в память, поэтому он может быть неэффективен для работы с очень большими файлами.
    - Если файл содержит много строк, можно рассмотреть альтернативные методы, такие как `Files.lines()`, который возвращает поток строк и позволяет работать с файлами более эффективно.

### Пример использования

Полный пример программы:

```java
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.List;

public class FilesAndPathEx3 {
    public static void main(String[] args) throws IOException {
        Path filePath = Paths.get("test10.txt");

        // Создание файла и запись строки в файл
        Files.createFile(filePath);
        String dialog = "-Privet \n -Privet \n -Kak dela? \n -Normalno \n -Tvoi kak? ";
        Files.write(filePath, dialog.getBytes());

        // Чтение всех строк из файла и вывод их на экран
        List<String> strings = Files.readAllLines(filePath);
        for (String s : strings) {
            System.out.println(s + " ");
        }
    }
}
```

### Заключение

Методы `write()` и `readAllLines()` позволяют легко работать с текстовыми файлами в Java. С их помощью можно записывать строки в файлы и считывать их обратно для дальнейшей обработки. Однако стоит учитывать объем данных, так как чтение больших файлов целиком может быть ресурсозатратным.

---