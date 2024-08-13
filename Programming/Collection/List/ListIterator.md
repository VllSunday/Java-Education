---
tags: 
создал заметку: 2024-07-14
---
[[List|ListIterator]]

### Краткое описание
ListIterator - расширяет возможности простого Iterator. В нем можно работать не только с каждым следующим элементом(использую метод **next**, но и с предыдущим (использую метод **previus**)


Просто пример использования ListIterator
```Java
public class ListIteratorExample {  
    public static void main(String[] args) {  
        String s = "madam";  
  
        List<Character> list = new LinkedList<>();  
        for (char ch : s.toCharArray()){  
            list.add(ch);  
        }  
        System.out.println(list);  
  
        ListIterator<Character> iterator = list.listIterator();  
        ListIterator<Character> reverseIterator = list.listIterator(list.size());  
  
        boolean isPalindrome = true;  
  
        while(iterator.hasNext() && reverseIterator.hasPrevious()){  
            if (iterator.next() != reverseIterator.previous()){  
                isPalindrome = false;  
                break;            }  
        }  
  
        if (isPalindrome){  
            System.out.println("Palindrome");  
        } else {  
            System.out.println("Is not palindrome");  
        }  
    }  
}
```

В этом примере мы создали 2 итератора, которые будут идти с разных сторон слова, и проверять символы на равенство. 

Метод previous идет в обратном направлении и метод next идет вперед.
![[Pasted image 20240714164859.png]]


