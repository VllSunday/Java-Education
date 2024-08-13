---
создал заметку: 2024-08-04
---
### Использование `FileWriter` для записи в файл

`FileWriter` — это класс из пакета `java.io`, предназначенный для записи текстовых данных в файл. Он предоставляет простой способ записи символов в файл, поддерживая как запись в новый файл, так и дополнение данных к существующему файлу.

#### Основные методы `FileWriter`

- **Конструкторы**:
    - `FileWriter(String fileName)`: Создает объект `FileWriter`, связанный с файлом по указанному пути. Если файл уже существует, он будет перезаписан.
    - `FileWriter(String fileName, boolean append)`: Создает объект `FileWriter`, связанный с файлом по указанному пути. Если параметр `append` установлен в `true`, данные будут добавляться в конец файла. Если `false`, файл будет перезаписан.
    - Также можешь принимать в конструктор не только строку, но и сам объект класса File 
- **Методы**:
    - `write(String str)`: Записывает строку `str` в файл.
    - `write(char[] cbuf)`: Записывает массив символов `cbuf` в файл.
    - `flush()`: Очищает буфер, записывая все данные в файл.
    - `close()`: Закрывает `FileWriter`, освобождая все связанные ресурсы.

#### Пример использования `FileWriter`

Рассмотрим пример, в котором создается файл и записываются несколько строк текста:

```java
import java.io.FileWriter;
import java.io.IOException;

public class Main {
    public static void main(String[] args) {
        // Текст для записи в файл
        String poem = "Стихи уж точно не слова\n" +
                      "С набором рифм и расстановкой точек,\n" +
                      "Пускай их целая глава,\n" +
                      "Стихи лишь то, что сказано меж строчек.";

        // Создание FileWriter с дописыванием в файл
        try (FileWriter fileWriter = new FileWriter("text.txt", true)) {
            fileWriter.write("Hello ");
            fileWriter.write("World");
            fileWriter.write("!!!");
            fileWriter.write("\n");
            fileWriter.write(poem);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```

#### Пояснение кода

Создание экземпляра `FileWriter`
```java
try (FileWriter fileWriter = new FileWriter("text.txt", true)) {
```

Создаем экземпляр `FileWriter`, указывая путь к файлу `text.txt`. Второй параметр конструктора `true` означает, что данные будут дописываться в конец файла, а не перезаписывать его.

Запись данных в файл:
```java
fileWriter.write("Hello ");
fileWriter.write("World");
fileWriter.write("!!!");
fileWriter.write("\n");
fileWriter.write(poem);
```

Обработка исключений:

```java
} catch (IOException e) {
    e.printStackTrace();
}
```

#### Важные моменты

1. **Автоматическое закрытие ресурса**: Использование конструкции `try-with-resources` (обозначенной как `try (...)`) гарантирует, что `FileWriter` будет автоматически закрыт после завершения блока `try`, даже если возникнет исключение. Это помогает избежать утечек ресурсов.
    
2. **Дописывание в файл**: Второй параметр конструктора `FileWriter` (`true`) указывает на то, что данные будут добавляться в конец файла,