---
создал заметку: 2024-08-23
---
Иногда при взаимодействии потоков встает вопрос, о извещении одних потоков о действиях других. 

Например, действие одного потока может на прямую зависить от действия другого потока, и нужно как то известить один поток о том что другой поток сделал какую то работу. 

Для подобных ситуаций, у класса Object определенно ряд методов, для извещения потоком других потоков о своих действиях часто используются следующие методы: 


**Метод ==`wait()`==**: Освобождает монитор и переводит поток в состояние ожидания, пока не будет вызван `notify()` или `notifyAll()`.

**Метод ==`notify()`==**: Уведомляет один из ожидающих потоков, но не освобождает монитор. Текущий поток остаётся обладателем монитора до завершения синхронизированного блока или метода.

==notifyAll ==- НЕ освобождает монитор и будет все потоки, у
которых ранее был вызван метод wait();

### Пример:

![[Pasted image 20240823144925.png]]

Для того что бы закрепить материал, мы сделаем программу, где будет производитель хлеба, и покупатель.

Мы постораемся сделать так что покупатель мог купить хлеб только есть он есть в продаже. Если его в продаже нет, значит нужно оповестить производителя что бы он сделал его. 

Вот пример реализации кода: 

```java
public class WaitNotifyExample {  
    public static void main(String[] args) {  
        Market market = new Market();  
        Producer producer = new Producer(market);  
        Consumer consumer = new Consumer(market);  
  
        Thread thread1 = new Thread(producer);  
        Thread thread2 = new Thread(consumer);  
  
        thread1.start();  
        thread2.start();  
    }  
}  
  
class Market {  
    private static final Object LOCK = new Object();  
    public int breadCount = 0;  
  
    public void getBread() {  
        synchronized (LOCK) {  
            while (breadCount < 1) {  
                try {  
                    LOCK.wait();  
                } catch (InterruptedException e) {  
                    throw new RuntimeException(e);  
                }  
            }  
            breadCount--;  
            System.out.println("Потребитель купил хлеб");  
  
            System.out.println("Кол-во хлеба в магазине" + breadCount);  
            LOCK.notify();  
        }  
    }  
  
    public synchronized void putBread() {  
        synchronized (LOCK) {  
            while (breadCount >= 5) {  
                try {LOCK.wait();}  
                catch (InterruptedException e) {throw new RuntimeException(e);}  
            }  
            breadCount++;  
            System.out.println("+1 булка");  
            System.out.println("Кол-во хлеба в магазине" + breadCount);  
            LOCK.notify();  
        }  
    }  }  
  
class Producer implements Runnable {  
    Market market;  
  
    public Producer(Market market) {  
        this.market = market;  
    }  
  
    @Override  
    public void run() {  
        for (int i = 0; i < 10; i++) {  
            market.putBread();  
        }  
    }  
}  
  
class Consumer implements Runnable {  
    Market market;  
  
    public Consumer(Market market) {  
        this.market = market;  
    }  
  
    @Override  
    public void run() {  
        for (int i = 0; i < 10; i++) {  
            market.getBread();  
        }  
    }  
}
```

### Очень важные моменты
1) На монитор какого объекта происходит синхронизация на том же объекте нужно вызывать методы wait() & notify()
2) Метод wait() может принимать в параметры миллисекунды.
	то есть сколько времени будет ждать этот поток до того как он станет активным
3) Почему мы wait() помещаем в while(loop) а не в if(condition) ?
    - Потому что это рекомендация от Java Dock. Там говориться что поток может проснуться без notify() и тогда мы должны быть уверенны в том что while() перепроверит условие еще раз

