---
создал заметку: 2024-08-25
---
==CopyOnWriteArrayList== имплементирует интерфейс List.

==CopyOnWriteArrayList== следует использовать тогда, когда
вам нужно добиться потокобезопасности, у вас
небольшое количество операций по изменению
элементов и большое количество по их чтению.

Почему важно что бы операций вставки и удаления элементов были не частыми?
Потому что CopyOnWriteArrayList это коллекция типа ArrayList и алгоритмом CopyOnWrite.

Что это значит? 

При каждом изменении элементов в данной коллекции, создается клон копия List-a нового вида

### Пример как работает эта коллекция

```java

public class CopyOnWriteArrayListEx {  
    public static void main(String[] args) throws InterruptedException {  
        CopyOnWriteArrayList<String> list = new CopyOnWriteArrayList<>();  
        list.add("Zaur");  
        list.add("Oleg");  
        list.add("Sasha");  
        list.add("Nikita");  
        list.add("Alex");  
        System.out.println(list);  
  
        Runnable runnable1 = () -> {  
            Iterator<String> iterator = list.iterator();  
            while(iterator.hasNext()) {  
                try {  
                    Thread.sleep(100);  
                } catch (InterruptedException e) {  
                    throw new RuntimeException(e);  
                }  
                System.out.println(iterator.next());  
            }  
        };  
  
        Runnable runnable2 = () -> {  
            try {  
                Thread.sleep(200);  
            } catch (InterruptedException e) {  
                throw new RuntimeException(e);  
            }  
            list.remove(4);  
            list.add("Elena");  
        };  
  
        Thread thread1 = new Thread(runnable1);  
        Thread thread2 = new Thread(runnable2);  
        thread1.start();  
        thread2.start();  
        thread1.join();  
        thread2.join();  
  
        System.out.println(list);  
    }  
}
```
Вывод
```console

[Zaur, Oleg, Sasha, Nikita, Alex]
Zaur
Oleg
Sasha
Nikita
Alex
[Zaur, Oleg, Sasha, Nikita, Elena]

```

Что тут происходит? 
Мы создаем CopyOnWriteArrayList и помещаем туда какие то данные.

Создадим 2 потока. 
1-й поток, будет выводить данные на экран с задержкой в 0.1 сек

2-й поток будет удалять последний элемент и добавлять новый с задержкой в 0.2 сек

Почему же все работает как нужно? Почему не выскочило исключение? 

### Как работает CopyOnWriteArrayList?

Здесь перед тем как мы начали итерироваться по всей коллекции: 
```java
Iterator<String> iterator = list.iterator();  
```

Итератору передалось состояние этой коллекции на момент создания итератора, он это запоминает, и выводит только эти элементы. Что происходит в паралельных потоках ему уже не важно.

В параллельных потока мы удалили элемент и добавили элемент. Итератор об этом не знает.

В этом потоке: 
Когда мы изменили CopyOnWriteArrayList
```java
Runnable runnable2 = () -> {  
            try {  
                Thread.sleep(200);  
            } catch (InterruptedException e) {  
                throw new RuntimeException(e);  
            }  
            list.remove(4);  
            list.add("Elena");  
        }; 
```

Создалась новая копия данной коллекции, потом мы добавили элемент, создалась еще 1 новая копия коллекции.

И только после того как Iterator закончил работу, старая копия уже никому не нужна, и следующая итерация: 

```java
 Thread thread1 = new Thread(runnable1);  
        Thread thread2 = new Thread(runnable2);  
        thread1.start();  
        thread2.start();  
        thread1.join();  
        thread2.join();  
  
        System.out.println(list);  // Вот эта
```

Работает уже с самой новой копией.

Такие образом, процесс создания копий очень затратный.

Таким образом, эту коллекцию стоит использовать тогда когда. В нашей программе не много операций по смене элементов, но много операций по чтению элементов

### Также нужно знать
 Что существует класс CopyOnWriteArraySet который работает по схожему сценарию, так что пристально рассматривать пока не буду.
