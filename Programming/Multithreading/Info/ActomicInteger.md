---
создал заметку: 2024-08-25
---

---

### Пример с использованием AtomicInteger

Рассмотрим следующий код:

```java
public class CounterExample {  
    static int counter = 0;  
  
    public static void increment() {  
        counter++;  
    }  
  
    public static void main(String[] args) throws InterruptedException {  
        Thread thread1 = new Thread(new MyRunnableImpl());  
        Thread thread2 = new Thread(new MyRunnableImpl());  
        thread1.start();  
        thread2.start();  
        thread1.join();  
        thread2.join();  
  
        System.out.println(counter);  
    }  
}  
  
class MyRunnableImpl implements Runnable {  
    @Override  
    public void run() {  
        for (int i = 0; i < 100; i++) {  
            CounterExample.increment();  
        }  
    }  
}
```

В этом примере мы увеличиваем значение переменной `counter`, используя метод `increment()`. Однако при выполнении этого кода в многопоточном окружении можно получить непредсказуемые результаты. Это происходит из-за того, что потоки, увеличивающие `counter`, не синхронизированы.

В прошлом мы решали эту проблему, добавляя синхронизацию:

```java
public synchronized static void increment() {  
    counter++;  
}  
```

Мы использовали ключевое слово `synchronized`, которое гарантировало, что только один поток в данный момент времени будет выполнять этот метод. Однако такой подход, несмотря на свою эффективность, является довольно затратным по ресурсам. Под капотом происходит множество сложных операций: захват монитора, выполнение операции увеличения и освобождение монитора.

Основная проблема заключается в том, что операция инкремента (`counter++`) не является атомарной. Это означает, что она состоит из нескольких шагов: чтение текущего значения, увеличение и запись обратно в память. Если два потока одновременно выполняют эту операцию, могут возникнуть проблемы гонки.

### Решение с использованием AtomicInteger

Java предлагает использовать класс `AtomicInteger` для работы с целочисленными значениями, где операции выполняются атомарно. Это позволяет избежать необходимости синхронизации и делает код более эффективным.

```java
import java.util.concurrent.atomic.AtomicInteger;

public class AtomicIntegerExample {  
    static AtomicInteger counter = new AtomicInteger(0);  
  
    public static void main(String[] args) throws InterruptedException {  
        Thread thread1 = new Thread(new MyRunnableImpl());  
        Thread thread2 = new Thread(new MyRunnableImpl());  
        thread1.start();  
        thread2.start();  
        thread1.join();  
        thread2.join();  
  
        System.out.println(counter.get());  
    }  
}  
  
class MyRunnableImpl implements Runnable {  
    @Override  
    public void run() {  
        for (int i = 0; i < 100; i++) {  
            AtomicIntegerExample.counter.incrementAndGet();  
        }  
    }  
}
```

В этом примере `counter` является объектом класса `AtomicInteger`. Мы используем метод `incrementAndGet()`, который атомарно увеличивает значение и возвращает его.

### Методы класса AtomicInteger

Класс `AtomicInteger` предоставляет множество методов для атомарных операций над целочисленными значениями. Рассмотрим некоторые из них:

- **`incrementAndGet()`** — атомарно увеличивает значение на единицу и возвращает обновлённое значение.
- **`getAndIncrement()`** — атомарно увеличивает значение на единицу, но возвращает старое значение.
- **`addAndGet(int delta)`** — атомарно увеличивает значение на заданное число (`delta`) и возвращает обновлённое значение.
- **`getAndAdd(int delta)`** — атомарно увеличивает значение на заданное число (`delta`), но возвращает старое значение.
- **`decrementAndGet()`** — атомарно уменьшает значение на единицу и возвращает обновлённое значение.
- **`getAndDecrement()`** — атомарно уменьшает значение на единицу, но возвращает старое значение.

### Дополнительные классы в пакете java.util.concurrent.atomic

В Java также существуют другие атомарные классы для различных типов данных:

- `AtomicLong` — для работы с типом `long`.
- `AtomicBoolean` — для работы с типом `boolean`.
- `AtomicReference<V>` — для работы с объектными ссылками.
- `AtomicIntegerArray` и `AtomicLongArray` — для работы с массивами `int` и `long`.

Использование атомарных классов рекомендуется в тех случаях, когда необходимо обеспечить безопасность потоков при работе с примитивными типами данных без использования синхронизации.

---
