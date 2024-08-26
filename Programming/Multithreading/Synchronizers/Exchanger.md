---
создал заметку: 2024-08-25
---
![[Exchanger.gif]]

# Exchanger

Exchanger - это синхронизатор, позволяющий
обмениваться данными между двумя потоками,
обеспечивает то, что оба потока получат информацию
друг от друга одновременно. Для обмена используется метод ==exchange()==  Может понадобиться, для того, чтобы обменяться данными между двумя потоками в определенной точки работы обоих потоков. Обменник — обобщенный класс, он параметризируется типом объекта для передачи.

```java
Exchanger<V>
```

![[Pasted image 20240825123802.png]]

- Если первый поток вызвал метод exchange() и он хочет обменяться информацией
то 2-й поток сразу информацию не увидит. Первый поток заблокируется до тех пор
пока не будет вызван метод exchange() у второго потока


Здесь следует обратить внимание на то что оба эти потока получают информацию обменянную в одно и тоже время. Информация которой они обмениваются, должна быть одного типа данных  

### Пример: Камень, Ножницы, Бумага

```java
public class ExchangerEx {  
    public static void main(String[] args) {  
        Exchanger<Action> exchanger = new Exchanger<>();  
        List<Action> friend1Action = new ArrayList<>();  
        friend1Action.add(Action.PAPER);  
        friend1Action.add(Action.ROCK);  
        friend1Action.add(Action.SCISSORS);  
  
        List<Action> myAction = new ArrayList<>();  
        myAction.add(Action.ROCK);  
        myAction.add(Action.PAPER);  
        myAction.add(Action.ROCK);  
  
        new BestFriend("Ivan" , friend1Action, exchanger);  
        new BestFriend("Sasha" , myAction, exchanger);  
  
  
    }  
}  
  
enum Action {  
    ROCK, PAPER, SCISSORS  
}  
  
class BestFriend extends Thread {  
    private String name;  
    private List<Action> myActions;  
    private Exchanger<Action> exchanger;  
  
    public BestFriend(String name, List<Action> myActions, Exchanger<Action> exchanger) {  
        this.name = name;  
        this.myActions = myActions;  
        this.exchanger = exchanger;  
        this.start();  
    }  
  
    private void whoWins(Action myAction, Action friendAction) {  
        if ((myAction == Action.ROCK && friendAction == Action.SCISSORS) ||  
           (myAction == Action.PAPER && friendAction == Action.ROCK) ||  
           (myAction == Action.SCISSORS && friendAction == Action.PAPER)) {  
            System.out.println(name + " Wins!!!");  
        }  
    }  
  
    @Override  
    public void run() {  
        Action reply;  
        for (Action action : myActions) {  
            try {  
                reply = exchanger.exchange(action);  
                whoWins(action, reply);  
                sleep(2000);  
            } catch (InterruptedException e) {  
                throw new RuntimeException(e);  
            }  
        }  
    }  
}
```
#### Результат: 
Ivan Wins!!!
Sasha Wins!!!
Sasha Wins!!!



### Пример: Доставка 
Рассмотрим следующий пример. Есть два грузовика: один едет из пункта A в пункт D, другой из пункта B в пункт С. Дороги AD и BC пересекаются в пункте E. Из пунктов A и B нужно доставить посылки в пункты C и D. Для этого грузовики в пункте E должны встретиться и обменяться соответствующими посылками.

```java
import java.util.concurrent.Exchanger;  
  
public class Delivery {  
    //Создаем обменник, который будет обмениваться типом String  
    private static final Exchanger<String> EXCHANGER = new Exchanger<>();  
  
    public static void main(String[] args) throws InterruptedException {  
        String[] p1 = new String[]{"{посылка A->D}", "{посылка A->C}"};//Формируем груз для 1-го грузовика  
        String[] p2 = new String[]{"{посылка B->C}", "{посылка B->D}"};//Формируем груз для 2-го грузовика  
        new Thread(new Truck(1, "A", "D", p1)).start();//Отправляем 1-й грузовик из А в D  
        Thread.sleep(100);  
        new Thread(new Truck(2, "B", "C", p2)).start();//Отправляем 2-й грузовик из В в С  
    }  
  
    public static class Truck implements Runnable {  
        private int number;  
        private String dep;  
        private String dest;  
        private String[] parcels;  
  
        public Truck(int number, String departure, String destination, String[] parcels) {  
            this.number = number;  
            this.dep = departure;  
            this.dest = destination;  
            this.parcels = parcels;  
        }  
  
        @Override  
        public void run() {  
            try {  
                System.out.printf("В грузовик №%d погрузили: %s и %s.\n", number, parcels[0], parcels[1]);  
                System.out.printf("Грузовик №%d выехал из пункта %s в пункт %s.\n", number, dep, dest);  
                Thread.sleep(1000 + (long) Math.random() * 5000);  
                System.out.printf("Грузовик №%d приехал в пункт Е.\n", number);  
                parcels[1] = EXCHANGER.exchange(parcels[1]);//При вызове exchange() поток блокируется и ждет  
                //пока другой поток вызовет exchange(), после этого произойдет обмен посылками                System.out.printf("В грузовик №%d переместили посылку для пункта %s.\n", number, dest);  
                Thread.sleep(1000 + (long) Math.random() * 5000);  
                System.out.printf("Грузовик №%d приехал в %s и доставил: %s и %s.\n", number, dest, parcels[0], parcels[1]);  
            } catch (InterruptedException e) {  
            }        }  
    }  
}
```

#### Результат: 

В грузовик №1 погрузили: {посылка A->D} и {посылка A->C}.  
Грузовик №1 выехал из пункта A в пункт D.  
В грузовик №2 погрузили: {посылка B->C} и {посылка B->D}.  
Грузовик №2 выехал из пункта B в пункт C.  
Грузовик №1 приехал в пункт Е.  
Грузовик №2 приехал в пункт Е.  
В грузовик №2 переместили посылку для пункта C.  
В грузовик №1 переместили посылку для пункта D.  
Грузовик №2 приехал в C и доставил: {посылка B->C} и {посылка A->C}.  
Грузовик №1 приехал в D и доставил: {посылка A->D} и {посылка B->D}.  

  
Как мы видим, когда один грузовик (один поток) приезжает в пункт Е (достигает точки синхронизации), он ждет пока другой грузовик (другой поток) приедет в пункт Е (достигнет точки синхронизации). После этого происходит обмен посылками (String) и оба грузовика (потока) продолжают свой путь (работу).