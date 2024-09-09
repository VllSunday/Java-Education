---
создал заметку: 2024-09-09
---
****
Конечно, я сделаю разбор твоего кода и соберу конспект для использования в Obsidian. Давай рассмотрим код и выполним подробный разбор с объяснениями:

### Серверная часть: `DatagramServerRunner`

```java
public class DatagramServerRunner {
    public static void main(String[] args) throws IOException {
        try (var datagramSocket = new DatagramSocket(7777);) {
            var bytes = new byte[512];
            DatagramPacket packet = new DatagramPacket(bytes, bytes.length);
            datagramSocket.receive(packet);
            System.out.println(new String(bytes));
        }
    }
}
```

#### Разбор:
1. **Создание сокета**:
   - `DatagramSocket datagramSocket = new DatagramSocket(7777);` — Здесь создается UDP сокет, привязанный к порту `7777` на сервере. В отличие от TCP, UDP сокет не требует явного соединения с клиентом.
   
2. **Буфер для приема данных**:
   - `var bytes = new byte[512];` — Создается массив байт, который будет служить буфером для приема данных. Размер буфера выбран равным 512 байтам.
   
3. **DatagramPacket**:
   - `DatagramPacket packet = new DatagramPacket(bytes, bytes.length);` — `DatagramPacket` используется для хранения данных, которые будут приняты через сокет. В конструктор передается буфер (массив `bytes`), который определяет максимальный объем данных для одного пакета.
   
4. **Прием данных**:
   - `datagramSocket.receive(packet);` — Эта строка блокирует выполнение программы до тех пор, пока сервер не получит пакет данных от клиента.
   
5. **Вывод данных**:
   - `System.out.println(new String(bytes));` — Принятые данные выводятся в виде строки. Однако важно понимать, что так как массив всегда имеет фиксированный размер (в данном случае 512 байт), строка может содержать лишние символы (пустые байты).

#### Примечания:
- UDP протокол не гарантирует доставку, а также не проверяет целостность данных, поэтому важно правильно регулировать размер буфера, чтобы избежать потери данных.
- В UDP сервер не "подключается" к клиенту, как это происходит в TCP, а просто ждет получения пакетов.

---

### Клиентская часть: `DatagramRunner`

```java
public class DatagramRunner {
    public static void main(String[] args) throws IOException {
        try (var datagramSocket = new DatagramSocket()) {
            var inetAddress = InetAddress.getByName("localhost");
            var bytes = "Hello from UDP client".getBytes();
            var packet = new DatagramPacket(bytes, bytes.length, inetAddress, 7777);
            datagramSocket.send(packet);
        }
    }
}
```

#### Разбор:
1. **Создание клиента**:
   - `DatagramSocket datagramSocket = new DatagramSocket();` — Создается UDP сокет без привязки к порту. Это клиентская сторона, и порт будет выбран динамически операционной системой.
   
2. **Получение IP-адреса**:
   - `InetAddress inetAddress = InetAddress.getByName("localhost");` — Этот код разрешает доменное имя `"localhost"` в IP-адрес. Здесь клиент пытается отправить пакет на локальный сервер.

3. **Подготовка данных**:
   - `var bytes = "Hello from UDP client".getBytes();` — Сообщение клиента ("Hello from UDP client") преобразуется в массив байт.

4. **Создание и отправка пакета**:
   - `DatagramPacket packet = new DatagramPacket(bytes, bytes.length, inetAddress, 7777);` — Создается пакет, который будет отправлен на IP-адрес сервера (здесь `"localhost"`) и порт `7777`.
   - `datagramSocket.send(packet);` — Отправка пакета через сокет.

#### Примечания:
- Клиент использует порт, который будет автоматически выбран операционной системой.
- Сообщение в UDP ограничено размером буфера, и если оно превышает этот размер, то часть данных может быть потеряна.

---

### Итоги и конспект для Obsidian:

#### **Конспект: UDP-программирование в Java**

##### 1. Основные концепции UDP
- **UDP (User Datagram Protocol)** — это протокол, работающий без установления соединения. Он не гарантирует доставку пакетов и не проверяет их целостность.
- UDP подходит для приложений, где важна скорость передачи, а потеря данных допустима, например, для потокового видео и аудио.

##### 2. Важные отличия между TCP и UDP:
- **TCP**:
  - Надежный, с гарантией доставки.
  - Устанавливается соединение (Socket/ServerSocket).
- **UDP**:
  - Соединение не устанавливается.
  - Используются классы `DatagramSocket` и `DatagramPacket`.

##### 3. Пример сервера на UDP:

```java
public class DatagramServerRunner {
    public static void main(String[] args) throws IOException {
        try (var datagramSocket = new DatagramSocket(7777)) {
            var bytes = new byte[512];
            DatagramPacket packet = new DatagramPacket(bytes, bytes.length);
            datagramSocket.receive(packet);
            System.out.println(new String(bytes));
        }
    }
}
```
- Сервер создает `DatagramSocket`, который привязывается к порту 7777.
- Используется буфер размером 512 байт для хранения полученных данных.
- Программа блокируется на методе `receive()`, ожидая получения данных от клиента.

##### 4. Пример клиента на UDP:

```java
public class DatagramRunner {
    public static void main(String[] args) throws IOException {
        try (var datagramSocket = new DatagramSocket()) {
            var inetAddress = InetAddress.getByName("localhost");
            var bytes = "Hello from UDP client".getBytes();
            var packet = new DatagramPacket(bytes, bytes.length, inetAddress, 7777);
            datagramSocket.send(packet);
        }
    }
}
```
- Клиент создает `DatagramSocket` без явного указания порта.
- Сообщение ("Hello from UDP client") отправляется серверу на порт 7777.

##### 5. Важные моменты:
- Размер буфера должен быть одинаковым на клиенте и сервере, чтобы избежать потерь данных.
- UDP пакеты могут быть потеряны, если превышен размер буфера.
- UDP протокол не гарантирует доставку пакетов, поэтому используется в тех случаях, когда небольшие потери данных допустимы.

---

Этот конспект должен помочь тебе в понимании основных аспектов работы с UDP в Java. Если будут дополнительные вопросы или ты хочешь что-то добавить, дай знать!