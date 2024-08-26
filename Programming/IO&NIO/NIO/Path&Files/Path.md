---
создал заметку: 2024-08-18
---

---
# Работа с интерфейсом Path в Java

## Интерфейс Path

### Определение
Интерфейс `Path` в Java представляет собой путь к файлу или директории в файловой системе. Это может быть как относительный, так и абсолютный путь.

### Пример создания объекта Path:
```java
Path path = Paths.get("text.txt");
```
Здесь `Paths.get()` возвращает объект типа `Path`, который представляет путь к файлу `text.txt`.

### Методы Path:

- **getFileName()**  
Возвращает имя файла или последней директории в пути.

  **Пример:**
  ```java
  Path filePath = Paths.get("test15.txt");
  System.out.println(filePath.getFileName());  // Вывод: test15.txt
  ```

- **getParent()**  
Возвращает путь к родительскому каталогу, если он существует. Если родителя нет (например, путь уже корневой), возвращает `null`.

  **Пример:**
  ```java
  Path directoryPath = Paths.get("C:\\Users\\AllSunday\\Desktop\\M");
  System.out.println(directoryPath.getParent());  // Вывод: C:\Users\AllSunday\Desktop
  ```

- **getRoot()**  
Возвращает корневой компонент пути (например, `C:\` для Windows). Если путь относительный, возвращает `null`.

  **Пример:**
  ```java
  System.out.println(directoryPath.getRoot());  // Вывод: C:\
  ```

- **isAbsolute()**  
Проверяет, является ли путь абсолютным.

  **Пример:**
  ```java
  System.out.println(filePath.isAbsolute());  // Вывод: false
  ```

- **toAbsolutePath()**  
Возвращает абсолютный путь. Если путь относительный, он конвертируется в абсолютный.

  **Пример:**
  ```java
  System.out.println(filePath.toAbsolutePath());  // Вывод: C:\Users\AllSunday\Desktop\M\test15.txt
  ```

- **resolve(Path other)**  
Соединяет текущий путь с переданным, создавая новый путь. Метод полезен для добавления подкаталога или файла к существующему пути.

  **Пример:**
  ```java
  System.out.println(directoryPath.resolve(filePath));  // Вывод: C:\Users\AllSunday\Desktop\M\test15.txt
  ```

- **relativize(Path other)**  
Возвращает относительный путь от текущего пути до указанного. Используется для определения, как перейти от одного пути к другому.
**relativize()** — метод вычисляет путь от текущего пути до указанного, возвращая относительный путь. В примере, если текущий путь — это директория `M`, а другой путь — это `M\N\Z\test20.txt`, то результатом будет `N\Z\test20.txt`.

  **Пример:**
  ```java
  Path anotherPath = Paths.get("C:\\Users\\AllSunday\\Desktop\\M\\N\\Z\\test20.txt");
  System.out.println(directoryPath.relativize(anotherPath));  // Вывод: N\Z\test20.txt
  ```


---
