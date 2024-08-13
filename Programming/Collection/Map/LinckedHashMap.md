---
создал заметку: 2024-07-17
---

[[Programming/Collection/Map/Map|LinkedHashMар]] является наследником HashMap. Хранит
информацию о порядке ==*добавления*== элементов или
порядке их *==использования==*. Производительность
методов немного ниже, чем у методов ***HashMap***.

Под использованием можно понять следующее: 
использование основных методов: get(), put(), remove();
и тд.

По дефолту LinkedHashMap хранит информацию о добавлении в LinkedHashMap. То есть если мы добавили элементы в определенной последовательности храниться они будут точно в такой же последовательности. 


То есть элемент LinkedHashMap содержит информацию не только о элементе, но и информацию о предыдущем и о следующем элементе HashMap


В конструкторе LinkedHashMap можно указать 3 параметра. 
1 - initialCapacity - 16;
2 - loadFactor - 0.75f;
3 - accessOrder - (true / false);
accessOrder - отвечает за то как наши элементы будут храниться в LinckedHashMap. То есть будут ли они храниться в ==порядке добавления==(значение false - по дефолту) или в ==порядке использования== (значение true)


-- Как работает порядок использования? 

Если мы в конструкторе параметр - accessOrder укажем true
Тогда порядок постоянно будет меняться в зависимости от того, какие элементы были использованы и в каком порядке

Пример: 
```java 
Map<Integer, Student> lhm = new LinckedHashMap<>(16, 0.75f, true);

Student st1 = new Student(name:"Sasha", surname: "Kapustin");
Student st2 = new Student(name:"Elena",surname: "Sidorova");
Student st3 = new Student(name:"Misha", surname: "Belui");
Student st4 = new Student(name:"Tom",surname: "Orange");

lhm.put(st1, 8.2);
lhm.put(st2, 7.9);
lhm.put(st3, 5.2);
lhm.put(st4, 9.9);

System.out.print(lhm);

lhm.put(lhm.get(st3));
lhm.put(lhm.get(st2));

System.out.print(lhm);

//Что тут происходит?
//Теперь порядок изменился: 
//Сейчас он выглядит так: st1, st4 , st3 , st2 
//То есть какой элемент был использованн последним тот и оказывается в конце LinckedHashMap
```



