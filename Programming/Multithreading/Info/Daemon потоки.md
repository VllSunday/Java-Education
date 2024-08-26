---
создал заметку: 2024-08-23
---
Daemon потоки предназначены для выполнения фоновых задач и оказания различных сервисов Uber потокам

При завершении работы последнего User потока
программа завершает своё выполнение, не дожидаясь
окончания работы Daemon потоков.

Получается очень простая логика. Если User потоки завершены, то им оказывать никакие сервисы не нужно. А значит можно просто завершить программу до завершения User потоков 

Для того что бы сделать поток Daemon нужно всего лишь вызвать метод setDaemon(true) со значением true;

#### Важно
При создании демон потока, метод setDaemon(true) должен быть вызван до того как будет запущен код. Потому что если попытаться вызвать метод setDaemon после запуска потока, будет выброшен Exception
Коротко: 
После создания потока 
setDaemon(true)
Старт потока: Thread.start();



Пример кода: 

```java
public class DaemonExample {  
    public static void main(String[] args) {  
        System.out.println("Main thread starts");  
        UserThread userThread = new UserThread();  
        userThread.setName("user_thread");  
  
        DaemonThread daemonThread = new DaemonThread();  
        daemonThread.setName("daemon_thread");  
        daemonThread.setDaemon(true);  
  
        userThread.start();  
        daemonThread.start();  
        System.out.println("Main thread ends");  
    }  
}  
  
class UserThread extends Thread{  
    @Override  
    public void run() {  
        System.out.println(Thread.currentThread().getName()  + " is daemon" + isDaemon());  
        for(char i = 'A'; i <= 'J'; i++){  
            try {  
                sleep(300);  
                System.out.println(i);  
            } catch (InterruptedException e) {  
                throw new RuntimeException(e);  
            }  
        }  
    }  
}  
  
class DaemonThread extends Thread{  
    @Override  
    public void run() {  
        System.out.println(Thread.currentThread().getName()  + " is daemon" + isDaemon());  
        for(int i = 1; i <= 1000; i++){  
            try {  
                sleep(100);  
                System.out.println(i);  
            } catch (InterruptedException e) {  
                throw new RuntimeException(e);  
            }  
        }  
    }  
}
```

