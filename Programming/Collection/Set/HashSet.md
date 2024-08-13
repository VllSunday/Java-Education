---
создал заметку: 2024-07-17
---
[[Set|HashSet]]

HashSet not-synchronized

HashSet не запоминает порядок добавления элементов.
В основе HashSet лежит HashMap. У элементов данного
HashMap: ключи - это элементы HashSet, значения - это
константа-заглушка.

### Как это работает на практике

1. Когда ты добавляешь элемент в HashSet (`hashSet.add(element)`), на самом деле вызывается метод `put` у внутреннего HashMap:
    
    java
    
    Копировать код
    
```Java
public boolean add(E e) {     return map.put(e, PRESENT) == null; }
```
    
    Здесь `PRESENT` — это специальная константа, которая используется как значение.
    
2. Внутренний HashMap хранит каждый элемент из HashSet в качестве ключа, а `PRESENT` в качестве значения. Это гарантирует, что все элементы в HashSet будут уникальными, потому что HashMap не допускает дублирующихся ключей.
    
3. Когда ты проверяешь, есть ли элемент в HashSet (`hashSet.contains(element)`), на самом деле вызывается метод `containsKey` у внутреннего HashMap:
    
    java
    
    Копировать код
    
    `public boolean contains(Object o) {     return map.containsKey(o); }`
    
4. Когда ты удаляешь элемент из HashSet (`hashSet.remove(element)`), на самом деле вызывается метод `remove` у внутреннего HashMap:
    
    java
    
    Копировать код
    
    `public boolean remove(Object o) {     return map.remove(o) == PRESENT; }`
5. Метода get(); нет, потому что метод get возвращает значения которые нет в Set


В математике над множествами, можно делать некоторые операции: 
1) Это операция объединения: (в математике она называется Union) 
![[SetUnion.png]]

При объединении мы получим одно большое множество: 
(1 , 2 , 3 , 4 , 5 , 7 , 8) - естественно повторяться они не будут так как Set хранит только уникальные значения

В Java это объединение может выполнять addAll 

```java 
Set<Integer> hashSet1 = new HashSet<>();  
hashSet1.add(1);  
hashSet1.add(2);  
hashSet1.add(3);  
  
Set<Integer> hashSet2 = new HashSet<>();  
hashSet2.add(3);  
hashSet2.add(4);  
hashSet2.add(5);  
  
Set<Integer> union = new HashSet<>(hashSet1);  
union.addAll(hashSet2);  
  
System.out.println(union);

//Выведет [1, 2, 3, 4, 5]
```

2) Это операция пересечение: (в математике называется "intersect")
![[intersect.png]]

За это в Java отвечает метод retainAll: 
```java 
Set<Integer> hashSet1 = new HashSet<>();  
hashSet1.add(1);  
hashSet1.add(2);  
hashSet1.add(3);  
  
Set<Integer> hashSet2 = new HashSet<>();  
hashSet2.add(3);  
hashSet2.add(4);  
hashSet2.add(5);  
  
Set<Integer> intersect = new HashSet<>(hashSet1);  
intersect.retainAll(hashSet2)
System.out.println(intersect);

//Выведет [3]
```

3) Это операция разность: (в математике она называется subtract)
![[subtract.png]]

За эту операцию отвечать метод: removeAll 

```java 
Set<Integer> hashSet1 = new HashSet<>();  
hashSet1.add(1);  
hashSet1.add(2);  
hashSet1.add(3);  
  
Set<Integer> hashSet2 = new HashSet<>();  
hashSet2.add(3);  
hashSet2.add(4);  
hashSet2.add(5);  
  
Set<Integer> subtract = new HashSet<>(hashSet1);  
subtract.removeAll(hashSet2)
System.out.println(subtract);

//Выведет [1, 2]
```

То есть останутся только те элементы которые уникальны для 1 hashSet
