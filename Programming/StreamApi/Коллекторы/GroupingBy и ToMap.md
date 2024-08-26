---
создал заметку: 2024-08-25
---
[[Steam API|GroupingBy и ToMap]]
## Различия между коллекторами потоков GroupingBy и ToMap в Java 8

Источник: [Devops.dev](https://blog.devops.dev/java-8-understanding-java-stream-collectors-groupingby-vs-tomap-5aaae145172f) _В этой статье подробно рассмотрены различия между коллекторами groupingBy и toMap() в Java, а также варианты их использования._ Java Stream API значительно улучшает работу с коллекциями объектов. Два этого есть распространенных инструмента: коллекторы (сборщики) groupingBy и toMap(). Оба они собирают элементы в Map, но делают это по-разному.

### Коллектор GroupingBy

Коллектор groupingBy выполняет группировку по функциям. Он образует группы элементов, которые имеют что-то общее. Вот как работает groupingBy:

`groupingBy(Function<? super T, ? extends K> classifier)`

- classifier: эта функция определяет, к какой группе принадлежит элемент.

Вот наглядный пример, когда у нас есть список людей и мы хотим сгруппировать их по возрасту:

`Map<Integer, List<Person>>peopleByAge =people.stream()     .collect(Collectors.groupingBy(Person::getAge));`

Здесь мы используем Person::getAge для группировки людей по возрасту. Он создает карту (Map), где ключами (key) являются возрасты (age), а значениями (value) — списки (lists) людей этого возраста.

### Коллектор ToMap

Коллектор toMap() напрямую превращает элементы в ключи и значения. Ему нужны две функции: одна для ключей (key) и одна для значений (value). Вот как работает toMap:

[![Python-университет](https://javarush.com/assets/images/site/featured-content/python-university/ru.png)](https://javarush.com/s/global_python_dynamic_banners_unlogged_v1)

`toMap(Functionv? super T, ? extends K> keyMapper,       Function<? super T, ? extends U> valueMapper)`

- keyMapper: Эта функция определяет, каким должен быть ключ.
- valueMapper: Эта функция определяет, каким должно быть значение.

Например, если у нас есть список людей и мы хотим создать Map, где имена — это ключи, а возраст — значения:

`Map<String, Integer> nameToAgeMap =people.stream()     .collect(Collectors.toMap(Person::getName, Person::getAge));`

В данном примере Person::getName дает нам имена (name) в качестве ключей, а Person::getAge предоставляет возраст (age) в качестве значений.

### Ключевые различия между GroupingBy и ToMap

- groupingBy группирует элементы на основе общих характеристик, тогда как toMap сопоставляет элементы непосредственно с ключами и значениями.
- Значения на карте. В версии groupingBy значения на карте (Map) представляют собой списки элементов с одинаковым ключом, тогда как в версии toMap значения берутся непосредственно из элементов.
- Варианты использования: используйте groupingBy, когда вы хотите объединить объекты в группы на основе чего-то общего. Используйте toMap, если вы хотите напрямую сопоставить каждый элемент с парой ключ-значение.

Подводя итоги, можно заметить, что коллекторы groupingBy и toMap делают похожие вещи, но работают они по-разному и используются для разных целей. Понимание этих различий помогает эффективно использовать Java Stream API в различных ситуациях.