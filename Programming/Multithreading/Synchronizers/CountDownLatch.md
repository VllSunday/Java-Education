---
создал заметку: 2024-08-24
---
![[CountDownLatch.gif]]

# CountDownLatch
CountDownLatch - замок с обратным отсчетом. 

Что из себя представляем данный синхронизатор? 

Данный синхронизатор позволяет нескольким потокам ждать до тех пор пока не закончиться обратный отсчет, то есть не настанет 0.

Другими словами CountDawnLatch предоставляет любому кол-ву потоков ожидать до тех пор, пока не завершиться определенное кол-во операций. После окончания этих операций, потоки будут отпущены чтобы продолжить свою деятельность.

В конструктор CountDownLatch обязательно передается  кол-во операций которое должно быть выполнено, что бы замок отпустил  заблокированные потоки 


### Методы

==await()== - если счетчик countDownLatch больше > 0 (То есть не все условия были выполнены)
то поток заблокируется до момента пока счетчик не станет 0.

Другими словами: 
метод ==await()== блокирует поток, вызвавший его, до тех пор, пока счетчик CountDownLatch не станет равен 0
Если же счетчик уже равен 0 тогда наш поток будет без проблем выполнять свою работу

==getCount()== - позывает чему равен счетчик countDownLatch (если 0 то все потоки освободятся)

==countDown()== - уменьшает счетчик countDownLatch на 1 

### Пример 1

```java
public class CountDownLatchEx {  
    static CountDownLatch countDownLatch = new CountDownLatch(3);  
    public static void main(String[] args) throws InterruptedException {  
        Friend friend1 = new Friend("Oleg", countDownLatch);  
        Friend friend2 = new Friend("Mongol", countDownLatch);  
        Friend friend3 = new Friend("Sasha", countDownLatch);  
        Friend friend4 = new Friend("Max", countDownLatch);  
  
        markerStaffIsOnPlace();  
        everythingIsReady();  
        openedMarket();  
    }  
    private static void markerStaffIsOnPlace() throws InterruptedException {  
        Thread.sleep(2000);  
        System.out.println("Персонал пришел на работу");  
        countDownLatch.countDown();  
        System.out.println("countDownLatch = " + countDownLatch.getCount());  
    }  
  
    private static void everythingIsReady() throws InterruptedException {  
        Thread.sleep(3000);  
        System.out.println("Все нормально можно открыть магазин");  
        countDownLatch.countDown();  
        System.out.println("countDownLatch = " + countDownLatch.getCount());  
    }  
  
    private static void openedMarket() throws InterruptedException {  
        Thread.sleep(4000);  
        System.out.println("Магазин открыт");  
        countDownLatch.countDown();  
        System.out.println("countDownLatch = " + countDownLatch.getCount());  
    }  
}  
  
class Friend extends Thread {  
    String name;  
    private CountDownLatch countDownLatch;  
  
    public Friend (String name, CountDownLatch countDownLatch) {  
        this.name = name;  
        this.countDownLatch = countDownLatch;  
        this.start();  
    }  
  
    @Override  
    public void run() {  
        try {  
            countDownLatch.await();  
            System.out.println(name + " преступил(a) к покупкам");  
        } catch (InterruptedException e) {  
            throw new RuntimeException(e);  
        }  
    }  
}
```
Простой симулятор BlackFriday

### Пример 2

```java
import java.util.concurrent.CountDownLatch;  
  
public class Race {  
    //Создаем CountDownLatch на 8 "условий"  
    private static final CountDownLatch START = new CountDownLatch(8);  
    //Условная длина гоночной трассы  
    private static final int trackLength = 500000;  
  
    public static void main(String[] args) throws InterruptedException {  
        for (int i = 1; i <= 5; i++) {  
            new Thread(new Car(i, (int) (Math.random() * 100 + 50))).start();  
            Thread.sleep(1000);  
        }  
  
        while (START.getCount() > 3) //Проверяем, собрались ли все автомобили  
            Thread.sleep(100);              //у стартовой прямой. Если нет, ждем 100ms  
  
        Thread.sleep(1000);  
        System.out.println("На старт!");  
        START.countDown();//Команда дана, уменьшаем счетчик на 1  
        Thread.sleep(1000);  
        System.out.println("Внимание!");  
        START.countDown();//Команда дана, уменьшаем счетчик на 1  
        Thread.sleep(1000);  
        System.out.println("Марш!");  
        START.countDown();//Команда дана, уменьшаем счетчик на 1  
        //счетчик становится равным нулю, и все ожидающие потоки        //одновременно разблокируются    }  
  
    public static class Car implements Runnable {  
        private int carNumber;  
        private int carSpeed;//считаем, что скорость автомобиля постоянная  
  
        public Car(int carNumber, int carSpeed) {  
            this.carNumber = carNumber;  
            this.carSpeed = carSpeed;  
        }  
  
        @Override  
        public void run() {  
            try {  
                System.out.printf("Автомобиль №%d подъехал к стартовой прямой.\n", carNumber);  
                //Автомобиль подъехал к стартовой прямой - условие выполнено  
                //уменьшаем счетчик на 1                START.countDown();  
                //метод await() блокирует поток, вызвавший его, до тех пор, пока  
                //счетчик CountDownLatch не станет равен 0                  START.await();  
                Thread.sleep(trackLength / carSpeed);//ждем пока проедет трассу  
                System.out.printf("Автомобиль №%d финишировал!\n", carNumber);  
            } catch (InterruptedException e) {  
            }        }  
    }  
}
```

