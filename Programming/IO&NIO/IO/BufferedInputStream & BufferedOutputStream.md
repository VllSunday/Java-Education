---
создал заметку: 2024-08-04
---
Тоже самое что и BufferedReader & BufferedWriter только для бинарных файлов. Можно сказать что эти стримы просто оболочка для создания буфера, который позволяет быстрее работать с файлами и данными (байтовыми)

Вот не большой пример кода: 
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


![[BufferedInputStream.png]]
