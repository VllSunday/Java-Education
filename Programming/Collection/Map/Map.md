---
создал заметку: 2024-07-15
---
Интерфейс [[Collection|Map]] в Java представляет собой коллекцию объектов, где каждый элемент имеет уникальный ключ и связанное с ним значение. Основные характеристики `Map`:

1. **Ключи и значения**: `Map` хранит пары "ключ-значение". Каждый ключ уникален, а значения могут повторяться.
2. **Быстрый доступ к данным**: Можно быстро получить значение по ключу.
3. **Операции с элементами**: Можно добавлять, удалять и обновлять элементы по ключу.

Основные методы интерфейса `Map`:
- `put(K key, V value)`: добавляет пару "ключ-значение" в коллекцию.
- `get(Object key)`: возвращает значение, связанное с указанным ключом.
- `remove(Object key)`: удаляет пару "ключ-значение" по указанному ключу.
- `containsKey(Object key)`: проверяет, существует ли указанный ключ в коллекции.
- `containsValue(Object value)`: проверяет, существует ли указанное значение в коллекции.
- `size()`: возвращает количество пар "ключ-значение" в коллекции.

Пример использования `Map`:

```java
import java.util.HashMap;
import java.util.Map;

public class MapExample {
    public static void main(String[] args) {
        Map<Integer, String> map = new HashMap<>();

        // Добавление элементов
        map.put(1, "Apple");
        map.put(2, "Banana");
        map.put(3, "Cherry");

        // Получение значения по ключу
        String value = map.get(2);
        System.out.println("Key 2: " + value); // Вывод: Key 2: Banana

        // Проверка наличия ключа
        boolean hasKey = map.containsKey(1);
        System.out.println("Contains key 1: " + hasKey); // Вывод: Contains key 1: true

        // Удаление элемента
        map.remove(3);

        // Количество элементов
        int size = map.size();
        System.out.println("Size: " + size); // Вывод: Size: 2
    }
}
```

`Map` является частью Java Collections Framework и предоставляет множество различных реализаций, таких как `HashMap`, `TreeMap`, `LinkedHashMap` и другие, каждая из которых имеет свои особенности.

![[MapInterface.png]]
