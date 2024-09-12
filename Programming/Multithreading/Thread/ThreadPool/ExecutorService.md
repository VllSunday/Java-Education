---
создал заметку: 2024-08-24
---
Threadpool удобнее всего создавать, используя factory
методы класса Executors:
Executors.newFixedThreadPool(int count) - создаст pool c 5-ю потоками;
Executors.newSingleThreadExecutor() - создаст pool с одним потоком.

#### Промежуточный итог

Для работы в Thread pool мы используем ExecutorService 
Для создания потоков внутри ExecutorService мы используем ==класс== Executors
с его ==фабричным== методом newFixedThreadPool(nThread);
Почему фабричный? Потому что в данном случаем мы создали ThreadPool не с помощью 
конструктора, а с помощью метода, поэтому он и называется Factory(фабрика по производству объектов)
 
Который возвращает объект типа ThreadPoolExecutor (который не рекомендовно создавать в ручную) 


Вот пример создания ThreadPool с 5 потоками: 

```java
ExecutorService service = Executors.newFixedThreadPool(5);
```

То есть если в нашей программе нужно что бы мы както ограничили кол-во потоков для выполнения определенной задачи. Мы создаем ThreadPool

### Пример

```java
public class ThreadPoolEx1 {  
    public static void main(String[] args) {  
        ExecutorService service = Executors.newFixedThreadPool(5);  
        for (int i = 0; i < 10; i++) {  
            service.execute(new RunnableIml100());  
        }  
        System.out.println("Main ends");  
    }  
}  
  
class RunnableIml100 implements Runnable{  
    @Override  
    public void run() {  
        System.out.println(Thread.currentThread().getName());  
        try {  
            Thread.sleep(500);  
        } catch (InterruptedException e) {  
            throw new RuntimeException(e);  
        }  
    }  
}
```

Здесь мы используем объект service и его метод execute(принимает реализацию Runnable) для того что бы выполнять задания, в этих самых потоках. 

Если не чего не делать. Программа продолжит работать. Потому что ThreadPool ждет еще задачи для выполнения. 

Если нам нужно завершить работу ==ExecutorServise== мы должны вызвать метод ==shutdown()==

Вызвав shutdown() задач больше не будет и программа завершиться ==только== после того как ==все== уже полученные задания выполнятся 

#### Более точные определения 
Метод ==execute== передаёт наше задание (task) в threadpool,
где оно выполняется одним из потоков.
После выполнения метода shutdown ExecutorService
понимает, что новых заданий больше не будет и, выполнив
поступившие до этого задания, прекращает работу.


### Метод awaitTermination 
Похож по принципу работы на join()
Принуждает поток в котором он вызвался подождать до тех пор, пока не выполнится одно
из двух событий: либо ExecutorService прекратит свою
работу, либо пройдёт время, указанное в параметре
метода awaitTermination

Пример: 

```java
public static void main(String[] args) throws InterruptedException {  
    ExecutorService service = Executors.newFixedThreadPool(5);  
    for (int i = 0; i < 10; i++) {  
        service.execute(new RunnableIml100());  
    }  
    service.shutdown();  
    service.awaitTermination(3, TimeUnit.SECONDS);  
    System.out.println("Main ends");  
}
// Время выполнения 1 задачи = 0.5 сек 
// Время выполнения всех задач = 1 сек
```
```text
pool-1-thread-1
pool-1-thread-3
pool-1-thread-5
pool-1-thread-2
pool-1-thread-4
pool-1-thread-3
pool-1-thread-5
pool-1-thread-1
pool-1-thread-2
pool-1-thread-4
Main ends
```

Получается что потоки выполнили задание быстрее чем, прошло 5 сек от метода 
==awaitTermination== и получается поток Main ждал. 

Рассмотрим другой пример: 

```java
public class ThreadPoolEx1 {  
    public static void main(String[] args) throws InterruptedException {  
        ExecutorService service = Executors.newFixedThreadPool(5);  
        for (int i = 0; i < 10; i++) {  
            service.execute(new RunnableIml100());  
        }  
        service.shutdown();  
        service.awaitTermination(1, TimeUnit.SECONDS);  
        System.out.println("Main ends");  
    }  
}  
  
class RunnableIml100 implements Runnable{  
    @Override  
    public void run() {  
        System.out.println(Thread.currentThread().getName());  
        try {  
            Thread.sleep(2000);  
        } catch (InterruptedException e) {  
            throw new RuntimeException(e);  
        }  
    }  
}
```

```text
pool-1-thread-1
pool-1-thread-3
pool-1-thread-2
pool-1-thread-4
pool-1-thread-5
Main ends
pool-1-thread-3
pool-1-thread-5
pool-1-thread-1
pool-1-thread-2
pool-1-thread-4
```

Здесь получается так что не все задачи успевают выполниться, и как только истекает время в 
```java
awaitTermination(1, TimeUnit.SECONDS);  
```

то поток main продолжает свою работу, в нашем случает но просто завершился, и вывел на экран Main ends

### Еще 1 вид ThreadPool-a

Еще 1 вид ThreadPool-a которые мы можем создать с помощью ExecutorService это 

```java
ExecutorService service = Executors.newSingleThreadExecutor();
```

В этом случае мы просто создали ThreadPool только с 1 потоком который будет выполнять задачи последовательно