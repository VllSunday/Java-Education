---
создал заметку: 2024-07-21
---
`ArrayDeque` (Array Double Ended Queue) — это реализация двунаправленной очереди на основе массива. Вот некоторые его особенности:

- **Реализация**: Основана на массиве. Он используется как циклический буфер, что позволяет эффективно добавлять и удалять элементы с обоих концов.
- **Возможности**: Поддерживает все основные операции очереди (FIFO) и дека (LIFO), такие как вставка, удаление и проверка элементов с обоих концов.
- **Производительность**: O(1) для операций вставки и удаления с обоих концов. Однако, если внутренний массив заполняется, он должен быть увеличен, что приводит к временной затратности O(n).

Методы: 

добавления
addFirst()
addLast()
offerFirst()
offerLast()

удаления:
removeFirst()
removeLast()
pollFirst()
pollLast()

доставания: 
getFirst()
getLast()
peekFirst()
peekLast()

Различия offer и add расписаны в Queue/LinkedList

Методы добавления: 
```java 
public static void main(String[] args) {  
        Deque<Integer> deque = new ArrayDeque<>();  
        deque.addFirst(3);  
        deque.addFirst(1);  
        deque.addLast(4);  
        deque.offerFirst(7);  
        deque.offerLast(9);  
  
        System.out.println(deque);  
}
//[7, 1, 3, 4, 9]
```



