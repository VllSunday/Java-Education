---
создал заметку: 2024-08-04
---
 ### Использование `FileReader` для чтения из файла

`FileReader` — это класс из пакета `java.io`, предназначенный для чтения текстовых данных из файла. Он предоставляет простой способ чтения символов из файла, поддерживая как чтение отдельных символов, так и целых строк.

#### Основные методы `FileReader`

- **Конструкторы**:
    
    - `FileReader(String fileName)`: Создает объект `FileReader`, связанный с файлом по указанному пути.
    - `FileReader(File file)`: Создает объект `FileReader`, связанный с объектом `File`.
- **Методы**:
    
    - `int read()`: Читает один символ из файла и возвращает его в виде целого числа. Возвращает `-1`, если достигнут конец файла.
    - `int read(char[] cbuf, int offset, int length)`: Читает символы в массив `cbuf`, начиная с указанного `offset` и до `length` символов.
    - `void close()`: Закрывает `FileReader`, освобождая все связанные ресурсы.

#### Пример использования `FileReader`

Рассмотрим пример, в котором создается файл и читаются несколько строк текста:

```java
import java.io.FileReader;
import java.io.IOException;

public class Main {
    public static void main(String[] args) {
        // Путь к файлу
        String filePath = "text.txt";

        // Чтение содержимого файла
        try (FileReader fileReader = new FileReader(filePath)) {
            int character;
            while ((character = fileReader.read()) != -1) {
                System.out.print((char) character);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

Чтение данных из файла:
```java
int character;
while ((character = fileReader.read()) != -1) {
    System.out.print((char) character);
}
```

Используем метод `read()`, чтобы последовательно читать символы из файла. Метод возвращает -1, когда достигнут конец файла. Прочитанные символы выводятся на консоль.


#### Дополнительные примеры

##### Чтение файла построчно с использованием `BufferedReader`

Для более эффективного чтения больших файлов можно использовать `BufferedReader` в сочетании с `FileReader`:

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class Main {
    public static void main(String[] args) {
        // Путь к файлу
        String filePath = "text.txt";

        // Чтение содержимого файла построчно
        try (BufferedReader bufferedReader = new BufferedReader(new FileReader(filePath))) {
            String line;
            while ((line = bufferedReader.readLine()) != null) {
                System.out.println(line);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

Использование `BufferedReader`

```java
try (BufferedReader bufferedReader = new BufferedReader(new FileReader(filePath))) {
```

Создаем экземпляр `BufferedReader`, обернув его вокруг `FileReader`. Это позволяет читать файл построчно, что может быть более эффективно для больших файлов.

Чтение строк:
```java
String line;
while ((line = bufferedReader.readLine()) != null) {
    System.out.println(line);
}
```

1. Используем метод `readLine()` для чтения строк из файла. Метод возвращает `null`, когда достигнут конец файла.    

Этот конспект дает подробное понимание работы с `FileReader` и его основными аспектами в Java, а также дополнительные способы чтения файлов с использованием `BufferedReader`.