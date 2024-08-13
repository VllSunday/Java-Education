---
создал заметку: 2024-07-15
---
[[Themes for collections|Non-statis & static initialazers]]

![[Non-static & static initialazers.png]]
```java
class Animal{
Animal() {
		(System.out.println("Constructor of Animal");
	}
static {
	System.out.println("Static init in Animal");
}
{
	System.out.println("non-static init in Animal");
}

		
class Mammal extends Animal{
Mammal() {
	(System. out.println("Constructor of Manmal");
 }
static {
	System.out.println("Static init in Mammal");
}
{
	System.out.println("non-static init in Mammal");
}
	 
class Lion extends Mammal{
Lion() {
	System.out.println("Constructor of Lion");)
}
static {
	System.out.println("Static init in Lion");
}
{
	System.out.println("non-static init in Lion");
}



//Вот что будет происходить: 
/* 
Сначала будут инициализарованны static initialazer
Они будут инициализированны 1 раз.

Потом при каждом сосдании класса, например вот так: 
Lion l = new Lion;
будут вызываться non-static initialazer
и конструкторы, каждого додительского класса + класса у которого происходит вызов
*\
```