---
создал заметку: 2024-08-20
---
![[BinarySearchIterative.png]]


![[BinarySearchRecursive.png]]



# LowerBound
![[BinarySearchLowerBoundImage.png]]

Нижняя граница: 

### Рекурсивные способ
BinarySearchLowerBound
![[Cache/BinarySearchLowerBound.png]]
### Итеративный способ 
![[BinarySearchLowerBound 1.png]]


# UpperBound
![[Pasted image 20240820215333.png]]
У нас есть какой то заданный key, и наша задача найти число в идеале которое будет минимально больше, то есть: the best result. Но все элементы которые больше тоже OK 


### Рекурсивное решение
![[Pasted image 20240820215711.png]]

### Итеративное решение
![[Pasted image 20240820215803.png]]

По сути говоря паттерн один:
1) Смотрим середину
2) Проверяем условия 
3) Прыгаем либо в лево, либо в права
4) И если есть возможность мы пытаемся запомнить лучший вариант



# Monotonic codition

![[Pasted image 20240820220636.png]]

### Наша задача: Найдите левую или правую границу числа

### Найти самый маленький элемент, из одинаковых

 