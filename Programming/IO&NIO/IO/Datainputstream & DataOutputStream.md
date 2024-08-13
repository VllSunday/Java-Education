---
создал заметку: 2024-08-04
---
### `DataInputStream` и `DataOutputStream`

`DataInputStream` и `DataOutputStream` являются классами в Java, которые предоставляют методы для чтения и записи примитивных типов данных (таких как `int`, `float`, `double`, `boolean`, `char` и другие) в потоках. Они полезны для записи и чтения данных в/из бинарного файла, обеспечивая портативность и удобство работы с различными типами данных.

#### `DataOutputStream`

`DataOutputStream` используется для записи примитивных типов данных в выходной поток в формате, независимом от платформы.

##### Основные методы

- `writeBoolean(boolean v)`: Записывает значение типа `boolean`.
- `writeByte(int v)`: Записывает значение типа `byte`.
- `writeChar(int v)`: Записывает значение типа ``char`.
- `writeDouble(double v)`: Записывает значение типа `double`.
- `writeFloat(float v)`: Записывает значение`float`.
- `writeInt(int v)`: Записывает значение типа `int`.
- `writeLong(long v)`: Записывает значение типа `long`.
- `writeShort(int v)`: Записывает значение типа `short`.
- `writeUTF(String s)`: Записывает строку в формате UTF-8.

##### Пример использования
```java
import java.io.DataOutputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class DataOutputStreamExample {
    public static void main(String[] args) {
        try (DataOutputStream dos = new DataOutputStream(new FileOutputStream("data.bin"))) {
            dos.writeInt(123);
            dos.writeDouble(456.78);
            dos.writeBoolean(true);
            dos.writeUTF("Hello, World!");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

#### `DataInputStream`

`DataInputStream` используется для чтения примитивных типов данных из входного потока в формате, независимом от платформы.
##### Основные методы

- `readBoolean()`: Читает значение типа `boolean`.
- `readByte()`: Читает значение типа `byte`.
- `readChar()`: Читает значение типа `char`.
- `readDouble()`: Читает значение типа `double`.
- `readFloat()`: Читает значение типа `float`.
- `readInt()`: Читает значение типа `int`.
- `readLong()`: Читает значение типа `long`.
- `readShort()`: Читает значение типа `short`.
- `readUTF()`: Читает строку в формате UTF-8.

##### Пример использования

```java
import java.io.DataInputStream;
import java.io.FileInputStream;
import java.io.IOException;

public class DataInputStreamExample {
    public static void main(String[] args) {
        try (DataInputStream dis = new DataInputStream(new FileInputStream("data.bin"))) {
            int intValue = dis.readInt();
            double doubleValue = dis.readDouble();
            boolean booleanValue = dis.readBoolean();
            String utfValue = dis.readUTF();

            System.out.println("Int value: " + intValue);
            System.out.println("Double value: " + doubleValue);
            System.out.println("Boolean value: " + booleanValue);
            System.out.println("UTF value: " + utfValue);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### Преимущества использования `DataInputStream` и `DataOutputStream`

1. **Портативность**: Эти классы обеспечивают независимый от платформы формат данных, что позволяет легко обмениваться данными между различными системами.
2. **Удобство**: Методы для чтения и записи различных примитивных типов данных делают код более чистым и понятным.
3. **Производительность**: Поскольку эти классы работают с бинарными данными, они могут быть более производительными по сравнению с текстовыми форматами.
### Ограничения
- **и `Obj** Для более сложных объектов необходимо использовать сериализацию (`ObjectInput`DataInputStream` работают только с примитивными типами данных и строками. `DataOutputStream``ObjectInputStream` Необработанные данные`ObjectOutputStream`).
- **Формат данных**: Если вы записываете данные с помощью `DataOutputStream`, ==вам нужно точно знать, в каком порядке и в каком формате данные были записаны==, чтобы правильно их прочитать с помощью `DataInputStream`.
### Заключение

`DataInputStream` и `DataOutputStream` являются мощными инструментами для работы с бинарными данными в Java. Они обеспечивают независимый от платформы формат данных и упрощают процесс чтения и записи примитивных типов данных и строк. Использование этих классов помогает разработчикам создавать эффективные и портативные приложения.