---
создал заметку: 2024-07-25
---
## Supplier<`T`>

Supplier переводится как поставщик

У этого интерфейса есть метод `T get();`

Интерфейс Supplier поставляет объекты когда его метод вызывается.

Получается что Supplier поставляет объекты определенного типа.
Вот не большой пример: 

```java
public class Main{  
  
    public static ArrayList<Car> makeThreeCar(Supplier<Car> carSupplier){  
            ArrayList<Car> al = new ArrayList<>();  
        for (int i = 0; i < 3; i++) {  
            al.add(carSupplier.get());  
        }  
        return al;  
    }  
  
    public static void main(String[] args) {  
  
        ArrayList<Car> ourCar = makeThreeCar(() -> new Car("Tesla", 300));  
        System.out.println("Out car" + ourCar);  
    }  
}  
  
  
class Car {  
    String model;  
    double enginePower;  
  
    public Car(String model, double enginePower) {  
        this.model = model;  
        this.enginePower = enginePower;  
    }  
  
    @Override  
    public String toString() {  
        return "Car{" +  
                "model='" + model + '\'' +  
                ", enginePower=" + enginePower +  
                '}';  
    }  
}
```
В этом коде мы создали класс Car

И сделали метод который создает 3 машины. Только вот этот метод не знает какие именно машины он создает. 
Потом при вызове этого метода, мы туда можно сказать `поставляем ` машины которые нужно создать. 

### Также немного информации: 
## Интерфейс `Supplier<T>`

### Общая информация

Интерфейс `Supplier<T>` из пакета `java.util.function` представляет собой функциональный интерфейс, который не принимает никаких аргументов и возвращает объект типа `T`.

Основной метод:
```java
T get();
```

### Преимущества и использование

- **Ленивая инициализация**: `Supplier` часто используется для ленивой инициализации объектов, когда объект создается только при первом запросе.
- **Фабричные методы**: Можно использовать `Supplier` для фабричных методов, которые создают новые объекты.
- **Поставщик значений**: `Supplier` удобно использовать в тех случаях, когда необходимо предоставить значение на основе некоторой логики или состояния.
- **Интерфейсы конфигурации**: `Supplier` может использоваться для предоставления конфигурационных значений, которые могут изменяться во времени.

### Примеры использования

1. **Простая ленивое создание объекта**
```java
Supplier<String> stringSupplier = () -> "Hello, World!";
System.out.println(stringSupplier.get()); // Output: Hello, World!
```

Ленивая инициализация

```java
public class LazyInitialization {
    private Supplier<Integer> lazyValue = this::computeValue;

    private int computeValue() {
        // Долгий вычислительный процесс
        return 42;
    }

    public int getValue() {
        return lazyValue.get();
    }

    public static void main(String[] args) {
        LazyInitialization instance = new LazyInitialization();
        System.out.println(instance.getValue()); // Output: 42
    }
}
```

Фабричный метод
```java
public class FactoryExample {
    public static void main(String[] args) {
        Supplier<Car> carSupplier = () -> new Car("Ford", 200);
        Car myCar = carSupplier.get();
        System.out.println(myCar); // Output: Car{model='Ford', enginePower=200.0}
    }
}

class Car {
    String model;
    double enginePower;

    public Car(String model, double enginePower) {
        this.model = model;
        this.enginePower = enginePower;
    }

    @Override
    public String toString() {
        return "Car{" +
                "model='" + model + '\'' +
                ", enginePower=" + enginePower +
                '}';
    }
}
```

Создание коллекции объектов

```java
import java.util.ArrayList;
import java.util.List;
import java.util.function.Supplier;

public class Main {
    public static void main(String[] args) {
        List<Car> cars = makeCars(3, () -> new Car("Tesla", 300));
        cars.forEach(System.out::println);
    }

    public static <T> List<T> makeCars(int count, Supplier<T> supplier) {
        List<T> list = new ArrayList<>();
        for (int i = 0; i < count; i++) {
            list.add(supplier.get());
        }
        return list;
    }
}
```

### Практическое применение

1. **Конфигурационные значения**:
```java
import java.util.function.Supplier;

public class Configuration {
    private Supplier<String> configSupplier;

    public Configuration(Supplier<String> configSupplier) {
        this.configSupplier = configSupplier;
    }

    public String getConfigValue() {
        return configSupplier.get();
    }

    public static void main(String[] args) {
        Configuration config = new Configuration(() -> "Default Config Value");
        System.out.println(config.getConfigValue()); // Output: Default Config Value
    }
}
```

**Создание объектов в потоках**:
```java
import java.util.function.Supplier;
import java.util.stream.Stream;

public class StreamExample {
    public static void main(String[] args) {
        Supplier<Car> carSupplier = () -> new Car("Audi", 250);
        Stream.generate(carSupplier)
              .limit(3)
              .forEach(System.out::println);
    }
}
```

### Рекомендации по использованию

- Используйте `Supplier`, когда вам нужно создать объект или получить значение, не принимая никаких аргументов.
- Предпочитайте `Supplier` в тех случаях, когда создание объекта может быть отложено до момента, когда он действительно понадобится.
- Используйте `Supplier` для фабричных методов, чтобы четко указать, что метод поставляет новый объект.
