---
создал заметку: 2024-08-23
---
DeadLock - ситуация, когда 2 или более потоков залочены навсегда, ожидают друг друга и ничего не делают
Когда это может быть? 

Когда несколько потоков используют синхронизацию на нескольких объектах и эти потоки используют синх. на этих объектах не в одинаковом порядке

### Пример

```java
public class DeadLock {  
    public static final Object lock1 = new Object();  
    public static final Object lock2 = new Object();  
  
    public static void main(String[] args) {  
        Thread10 thread10 = new Thread10();  
        Thread20 thread20 = new Thread20();  
  
        thread10.start();  
        thread20.start();  
    }  
}  
  
class Thread10 extends Thread{  
    @Override  
    public void run() {  
        System.out.println("Thread10: Попытка захватить монитор объекта Lock1");  
        synchronized (DeadLock.lock1){  
            System.out.println("Thread10: монитор объекта Lock1 захвачен");  
            System.out.println("Thread10: Попытка захватить монитор объекта Lock2");  
            synchronized (DeadLock.lock2){  
                System.out.println("Thread10: мониторы объектов Lock1 & Lock2 захвачен");  
            }  
        }  
    }  
}  
  
class Thread20 extends Thread{  
    @Override  
    public void run() {  
        System.out.println("Thread20: Попытка захватить монитор объекта Lock1");  
        synchronized (DeadLock.lock2){  
            System.out.println("Thread20: монитор объекта Lock2 захвачен");  
            System.out.println("Thread20: Попытка захватить монитор объекта Lock1");  
            synchronized (DeadLock.lock1){  
                System.out.println("Thread20: мониторы объектов Lock1 & Lock2 захвачен");  
            }  
        }  
    }  
}
```

### Как выглядит DeadLock?

На примере видно, как именно может произойти DeadLock

У нас есть Lock1 & Lock2

Так же у нас есть два класса реализующие Runnable

```java
class Thread10 extends Thread{  
    @Override  
    public void run() {  
        System.out.println("Thread10: Попытка захватить монитор объекта Lock1");  
        synchronized (DeadLock.lock1){  
            System.out.println("Thread10: монитор объекта Lock1 захвачен");  
            System.out.println("Thread10: Попытка захватить монитор объекта Lock2");  
            synchronized (DeadLock.lock2){  
                System.out.println("Thread10: мониторы объектов Lock1 & Lock2 захвачен");  
            }  
        }  
    }  
}  
  
class Thread20 extends Thread{  
    @Override  
    public void run() {  
        System.out.println("Thread20: Попытка захватить монитор объекта Lock1");  
        synchronized (DeadLock.lock2){  
            System.out.println("Thread20: монитор объекта Lock2 захвачен");  
            System.out.println("Thread20: Попытка захватить монитор объекта Lock1");  
            synchronized (DeadLock.lock1){  
                System.out.println("Thread20: мониторы объектов Lock1 & Lock2 захвачен");  
            }  
        }  
    }  
}
```

Какая ситуация называется DeadLock?

Такая когда:
поток Thread10 захватывает монитор lock1 а 
поток Thread20 захватывает монитор lock2 

И каждый из потоков не может продолжить свою работу. Потому что они теперь находятся в бесконечном ожидании тогда освободятся  противоположные мониторы.

То есть: 
поток Thread10 ждет пока монитор lock2 (захваченый lock1) не освободиться а 
поток Thread20 ждет пока монитор lock1 (захваченый lock2) не освободиться то же

И он ничего не делают просто ждут. 

Для того что бы не возникало такой проблемы. Когда мы будем работать с блоками, и пытаться синхронизироваться по монитору больше чем одного объекта, нужно быть осторожным. Нужно стараться синхронизироваться в одинаковом порядке для разных методов: 

### Исправленный вариант, при котором всегда будет нормальный вывод: 
```java
class Thread10 extends Thread{  
    @Override  
    public void run() {  
        synchronized (DeadLock.lock1){  
            synchronized (DeadLock.lock2){  
            
            }  
        }  
    }  
}  
  
class Thread20 extends Thread{  
    @Override  
    public void run() {  
        synchronized (DeadLock.lock1){  
           synchronized (DeadLock.lock2){  
           
            }  
        }  
    }  
}
```
