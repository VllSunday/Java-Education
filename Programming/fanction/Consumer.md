---
создал заметку: 2024-07-25
---
Cunsumer - переводится как потребитель.

Этот интерфейс противоположен Supplier

У него есть метод:  `void accept (T t);`

То есть интерфейс Supplier он возвращал объект. 
А интерфейс Comsumer только потребляет

Под потреблением мы понимаем что мы хотим что то сделать с этим объектом, то есть как-то его потребить, использовать

### Пример
Воспользуемся тем же кодом, что и в Supplier<`T`> 
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

Давай те сделаем метод который будет менять нашу машину. 
Как именно менять мы будем решать при вызове этого метода.

Вот как будет выглядеть наш метод: 
```java
public static void changeCar(Car car, Consumer<Car> carConsumer){  
    carConsumer.accept(car);  
}  
  
public static void main(String[] args) {  
  
    ArrayList<Car> ourCar = makeThreeCar(() -> new Car("Tesla", 300));  
    System.out.println("Out car" + ourCar);  
  
    changeCar(ourCar.get(0),  
            (car) -> {  
            car.model = "Toyota";  
            car.enginePower = 400;  
            });  
    int count = 1;  
    ourCar.forEach(car -> System.out.println("Машина: №" +  count + car));  
}
```

Что мы тут сделали? 
Мы создали метод, который будет менять машину. 
Мы в аргументы этого метода передали: Car car, Cunsumer<`Car`> carConsumer.

Далее в теле метода мы вызвали у carConsumer метод -  accept, который просто принимает какой то тип.

Далее мы уже использую метод changeCar передаем что именно мы будет делать с этой машиной. 




