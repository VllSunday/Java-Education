---
создал заметку: 2024-09-11
---
XML в настоящее время не пользуется большой популярностью, потому что на данные момент существует более современные форматы.\

Работа с XML состоит из 3х основных частей

#### 1) Написание XML 
XML - это текстовый формат данных, который состоит из 2 основных частей

- Теги 
- Атрибуты

Так как мы можем задавать любой имя тегам и атрибутам, XML получается гибким форматом.
Так же иногда мы должны ограничивать названия XML атрибутов для того чтобы мы могли вводить только допустимые 

Для этого существуют схемы / или валидаторы.

Он бывают 2х разных видов: DTD (устаревший)
	XSD более современный 
То есть все происходит так: мы написали какой то XML, потом идет проверка соответствует ли XSD схеме.


Далее когда принимающая сторона, получает этот XML его нужно распарсить(извлечь)
В Java есть 3 основных парсера: DOM/SAX/StAX
Так же есть универсальный инструмент XPath для работы не только в Java но и других местах.
Представляет из себя что то вроде SQL запросов, с помощью который мы можем извлечь любую информацию XML файла 


