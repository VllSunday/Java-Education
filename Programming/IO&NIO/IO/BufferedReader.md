---
создал заметку: 2024-08-04
---
Для начала. Что такое буферизация? 

Это процесс  (например: музыки, или видео) до их воспроизведения. Эта форма  буферизации позволяет смотреть или слушать медиа файлы почти мгновенно. Загружая только часть файла, затем воспроизводя его, пока оставшееся часть продолжает загружаться.


Использование буферизации в стримах позволяет
достичь большей эффективности при чтении файла
или записи в него.


По сути говоря: BufferedReader & BufferedWriter
==это всего лишь обертки которые добавляют функциональность буферизации для чтения или записи файла==
Имеют вот такой синтаксис: 

```java
BufferedReader reader = new BufferedReader(new FileReader("text.txt"))
```

```java
BufferedWriter writer = new BufferedWriter(new FileWriter("text.txt"))
```

Получается мы просто оборачивает наши FileReader & FileWriter 
для того что бы использовать буферизацию, и путем уменьшения кол-во операций для считывания или записи файла, увеличиваем скорость

Кол - во выполняемых действий при использовании, буферизированного подхода - кратно меньше. Потому что мы можем за одну операцию сразу заполнить буфер данными, и потом уже либо записать либо считать файл. И есть места в буфере не хватило, то эта операция будет произведена повторно.


#### Несколько примеров, для закрепления темы: 

```java
public static void main(String[] args) throws IOException {  
    try (BufferedReader reader = new BufferedReader(new FileReader("text.txt"));  
         BufferedWriter writer = new BufferedWriter(new       FileWriter("text2.txt")))  
    {  
        int character;  
        while((character = reader.read()) != -1){  
            writer.write(character);  
        }  
    }  
}
```

Этот код выполняет копирование содержимого из одного файла (`text.txt`) в другой файл (`text2.txt`). Он использует классы `BufferedReader` и `BufferedWriter` для буферизованного чтения и записи данных.

```java
try (BufferedReader reader = new BufferedReader(new FileReader("text.txt"));
     BufferedWriter writer = new BufferedWriter(new FileWriter("text2.txt")))
```

Используем конструкцию `try-with-resources`, чтобы автоматически закрыть ресурсы после использования. Внутри `try` создаются два ресурса:

1. **`BufferedReader reader`**: Создается объект `BufferedReader`, оборачивающий `FileReader`, для буферизованного чтения из файла `text.txt`.
    
2. **`BufferedWriter writer`**: Создается объект `BufferedWriter`, оборачивающий `FileWriter`, для буферизованной записи в файл `text2.txt`.

#### Чтение и запись символов:
```java
int character; while ((character = reader.read()) != -1) {    writer.write(character); 										
}
```

**Инициализация переменной `character`**:int character;
**Чтение данных из файла**:while ((character = reader.read()) != -1) {
	В цикле `while` читаем символы из файла с помощью метода `read()` объекта `reader`. Метод `read()` возвращает -1, когда достигается конец файла.

**Запись данных в другой файл**:`writer.write(character);`

4. Автоматически закрывает оба файла после завершения операции, независимо от того, было ли выброшено исключение или нет.
#### Еще один способ, используя метод readLine()

```java
public static void main(String[] args) throws IOException {  
    try (BufferedReader reader = new BufferedReader(new FileReader("text.txt"));  
         BufferedWriter writer = new BufferedWriter(new FileWriter("text2.txt")))  
    {  
        String line;  
        while((line = reader.readLine()) != null){  
            writer.write(line);  
            writer.write('\n');  
        }  
    }  
}
```

