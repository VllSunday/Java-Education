---
создал заметку: 2024-08-04
---
Fileinputstream и FileOutputStream используются для
работы с бинарными файлами.

Вот пример работы: 
![[FileInputStream.png]]

```java
public static void main(String[] args) throws IOException {  
    try (BufferedInputStream bufferedInputStream = new BufferedInputStream(new FileInputStream("C:\\Users\\AllSunday\\Desktop\\img.webp"));  
         BufferedOutputStream bufferedOutputStream = new BufferedOutputStream(new FileOutputStream("img.webp")))  
    {  
        int i;  
        while((i = bufferedInputStream.read()) != -1) {  
            bufferedOutputStream.write(i);  
        }  
    }  
}
```

По сути это то же самое что и FileReader & FileWriter 
Только для роботы с бинарными файлами

Так же как и для FileReader & FileWriter 
У FileInputStream & FileOutputStream есть свои стримы для буфиризированного подхода: 

[[BufferedInputStream & BufferedOutputStream]]