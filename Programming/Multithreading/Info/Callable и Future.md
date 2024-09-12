---
создал заметку: 2024-08-24
---
---
### Интерфейсы Callable и Future

Callable, также как и Runnable, представляет собой определённое
задание, которое выполняется потоком.

В отличии от Runnable Callable:
имеет return type не void;
может выбрасывать Exception.

Метод ==submit== передаёт наше задание (task) в thread pool, для
выполнения его одним из потоков, и возвращает тип ==Future==, в
котором и хранится результат выполнения нашего задания.

Метод ==get== позволяет получить результат выполнения нашего
задания из объекта ==Future==. 

Почему Future так называется? 

Рассмотрим пример: 
```java
public class CallableFactorial {  
    static int factorialResult;  
    public static void main(String[] args)throws InterruptedException {  
  
        ExecutorService service = Executors.newSingleThreadExecutor();  
        Future<Integer> future = service.submit(new CallableImpl(5));  
        try {  
            System.out.println("Хотим получить результат");  
            factorialResult = future.get();  
            System.out.println("получили рельтат");  
        } catch (InterruptedException e) {  
            throw new RuntimeException(e);  
        } catch (ExecutionException e) {  
            System.out.println(e.getCause());  
        } finally {  
            service.shutdown();  
        }  
        System.out.println(factorialResult);  
    }  
}  
  
class CallableImpl implements Callable<Integer> {  
    int f;  
  
    public CallableImpl(int f) {  
        this.f = f;  
    }  
  
    @Override  
    public Integer call() throws Exception {  
        if (f <= 0) {  
            throw new NumberFormatException("Не допустимое число");  
        }  
        int result = 1;  
        for (int i = 1; i <= f; i++) {  
            result *= i;  
            Thread.sleep(1000);  
        }  
        return result;  
    }  
}
```

Для того что бы объект реализующий интерфейс Callable засунуть в ThreadPool мы используем метод submit.

submit() возвращает тип Future в котором и будет храниться наше значение. 

Далее мы в коде пытаемся достать из Future результат выполнения нашего task

Но тут и есть главная особенность Future. 
Future так называется(будущее) потому что не хранит данные в данный момент, а только будет хранить данные ==ПОСЛЕ ТОГО== как task выполниться в потоке. 
Но до тех пор, во Future ничего не храниться. 

Если попытаться достать значения из Future до того как закончилось выполнение task-a. Поток в котором вызывается метод get( ) будет заблокирована до тех пор пока на завершиться выполнение task-a

Почему мы не использовали awaitTermination() ? 
Потому что это не обязательно ведь когда компилятор дошел до этой строки: 

```java
 factorialResult = future.get();
```

поток заблокировался до тех пор пока в переменную factorial не будет записано значение. А значит мы 100% сначала получим значение а уже потом продолжим выполнение main (за исключением случаев когда будет выброшено исключение). 

так же с помощью Future мы можем проверить закончилось ли выполнение task-a

### isDone()

Метод isDone позволяет понять, выполнился ли task.

Вернет true - если да.
Вернет false - если нет.

Пример: 
```java
System.out.println(future.isDone());  
System.out.println("Хотим получить результат");  
factorialResult = future.get();  
System.out.println("получили рельтат");  
System.out.println(future.isDone());
```

```java
false
Хотим получить результат
получили рельтат
true
```

Как вы поняли объект типа Future возвращается если использовать метод ==submit()== 

Так вот, метод Submit мы можем использовать когда мы работаем с Runnable тоже

Пример 
```java
Future future = executorService.submit(factorial);
```

Важно обратить внимание на то что мы уже не можем использовать Generics
Потому что метод run уже не возвращает какой то результат, поэтому  Future здесь используется без Generic. И метод ==get== который будет вызван на Future всегда будет возвращать ==Null== 

Может возникнуть логичный вопрос. Зачем тогда нужно использовать submit c Runnable 

С его помощью мы можем делать cansel нашего task-a 
Или узнавать закончилась ли его работа idDone()

## Важный момент 

#### Runnable
- мы можем использовать как с executorService так и при отдельном создании 
  thread 
#### Callable
 - мы можем использовать только с executor 

# Интересный пример

Найти сумму чисел от 1 до 1_000_000_000 
Используя многопоточность и интерфейс Callable

```java
public class SomeNumbers {  
    private static long value = 1_000_000_000L;  
    private static long sum = 0;  
  
    public static void main(String[] args) throws ExecutionException, InterruptedException {  
        ExecutorService service = Executors.newFixedThreadPool(10);  
        List<Future<Long>> futureResult = new ArrayList<>();  
        long valueDividedBy10 = value / 10;  
        for (int i = 0; i < 10; i ++) {  
            long from = valueDividedBy10 * i + 1;  
            long to = valueDividedBy10 * (i + 1);  
  
            PartialSum task = new PartialSum(from, to);  
            futureResult.add(service.submit(task));  
        }  
  
        for (Future<Long> result : futureResult) {  
            sum += result.get();  
        }  
        System.out.println("Total sum " + sum);  
        service.shutdown();  
    }  
}  
  
class PartialSum implements Callable<Long> {  
    long from;  
    long to;  
    long localSum;  
  
    public PartialSum(long from, long to) {  
        this.from = from;  
        this.to = to;  
    }  
  
    @Override  
    public Long call() throws Exception {  
        for (long i = from; i <= to; i++) {  
            localSum += i;  
        }  
        System.out.println("Sum from " + from + " to " + " = " + localSum);  
        return localSum;  
    }  
}
```
