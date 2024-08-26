---
создал заметку: 2024-08-23
---
Lock - интерфейс, который имплементируется классом
ReentrantLock.

Также как ключевое слово synchronized, Lock нужен для
достижения синхронизации между потоками.

### Пример
```java
public class LockExample {  
    public static void main(String[] args) {  
  
        Call call = new Call();  
        Thread thread1 = new Thread(call::mobileCall);  
        Thread thread2 = new Thread(call::skypeCall);  
        Thread thread3 = new Thread(call::whatsappCall);  
  
        thread1.start();  
        thread2.start();   
        thread3.start();  
  
    }  
}  
  
class Call {  
    private Lock lock = new ReentrantLock();  
  
    void mobileCall() {  
        lock.lock();  
        System.out.println("Mobile call starts");  
        try {  
            Thread.sleep(3000);  
        } catch (InterruptedException e) {  
            throw new RuntimeException(e);  
        } finally {  
            lock.unlock();  
        }  
  
        System.out.println("Mobile call ends");  
  
    }  
  
/// Дургие методы.... Просто менятеся называние и время 
```

Если у нас будет несколько потоков, и какой то 1 из них вызовет 1 из этих методов
MobleCall | SkypeCall |  Whatsapp

То другой поток. Вызвав другой поток, вызвав один из этих методов, увидит что замок(замок) закрыт. И этот замок будет закрыт до тех пор пока первый поток не откроет этот замок.
То есть замок откроется в самом конце этого метода 


В этом примере, можно Lock запросто заменить на synchromized block, и программа будет работать также. 
Принципе цель ReantrentLock-а и synchronized конструкции одинакова, мы добивается того что бы только 1 поток работал только 1 момент времени, остальные потоки ждали.


Какой + есть у synchronized конструкции по отношению к Lock-у
 В Lock постоянно нужно помнить что нужно закрыть его. Иначе Lock так и останется висеть на потоке и не освободит его.

У Lock над synchronized тоже есть свои плюсы

### Еще 1 пример

```java
public class Bankomat {  
    public static void main(String[] args) {  
        Lock lock = new ReentrantLock();  
        Employee employee1 = new Employee("Zhenya", lock);  
        Employee employee2 = new Employee("Zaur", lock);  
        Employee employee3 = new Employee("Sasha", lock);  
        Employee employee4 = new Employee("Masha", lock);  
        Employee employee5 = new Employee("Ben", lock);  
    }  
}  
  
class Employee extends Thread {  
    String name;  
    private Lock lock;  
  
    public Employee(String name, Lock lock) {  
        this.name = name;  
        this.lock = lock;  
        this.start();  
    }  
  
    @Override  
    public void run() {  
        try {  
            System.out.println(name + "ждет... ");  
            lock.lock();  
  
            System.out.println(name + " пользуется банкоматом");  
            Thread.sleep(2000);  
  
            System.out.println(name + " завершил свои дела");  
        } catch (InterruptedException e) {  
            throw new RuntimeException(e);  
        } finally {  
            lock.unlock();  
        }  
    }  
}
```


### Преимущество Lock над Synchronized 

У Lock помимо методов lock() & unlock()
Есть tryLock() 

Метод `tryLock()` пытается захватить блокировку и возвращает `true`, если блокировка была успешно захвачена, и `false`, если нет.

То есть если поток не сможет захватить блокировку, то он просто пройдет и начьнет выполнять последующий код

##### Пример tryLock()

```java
Override  
public void run() {  
    if (lock.tryLock()) {  
        try {  
            System.out.println(name + " пользуется банкоматом");  
            Thread.sleep(2000);  
            System.out.println(name + " завершил свои дела");  
        } catch (InterruptedException e) {  
            throw new RuntimeException(e);  
        } finally {  
            lock.unlock();  
        }  
    } else {  
        System.out.println(name + " не хочет ждать");  
    }  
}
```


Более подробное сравнение

[[Synchronized VS Lock]]  