# Лабораторная работа №4

## Потоки выполнения, работа с файлами и коллекции в Java

### Теоретический материал

#### 1. Назначение лабораторной работы

В реальных приложениях программа должна уметь:
- выполнять несколько задач одновременно;
- работать с файлами (чтение, запись, хранение данных);
- хранить и обрабатывать наборы объектов удобным способом.

Для этого в Java используются:
- потоки выполнения (Threads);
- файловый ввод‑вывод;
- коллекции.

#### 2. Потоки выполнения (Threads)

##### 2.1 Что такое поток

Поток — это независимая последовательность выполнения инструкций внутри программы.

**Примеры:**
- обработка данных в фоне;
- загрузка файлов;
- ожидание пользователя;
- параллельные вычисления.

По умолчанию программа выполняется в одном потоке — `main`.

##### 2.2 Зачем нужны потоки

**Без потоков:**
- программа «зависает» при долгой операции;
- пользователь не может взаимодействовать с программой.

**С потоками:**
- задачи выполняются параллельно;
- приложение остаётся отзывчивым.

##### 2 Newton Создание потока через `Thread`

```java
class MyThread extends Thread {
    @Override
    public void run() {
        for (int i = 1; i <= 5; i++) {
            System.out.println("Поток работает: " + i);
        }
    }
}
```

**Запуск:**

```java
MyThread thread = new MyThread();
thread.start(); // НЕ run()
```

##### 2.4 Создание потока через `Runnable` (рекомендуется)

```java
class MyTask implements Runnable {
    @Override
    public void run() {
        for (int i = 1; i <= 5; i++) {
            System.out.println("Задача выполняется: " + i);
        }
    }
}
```

**Запуск:**

```java
Thread thread = new Thread(new MyTask());
thread.start();
```

##### 2.5 Пауза потока (`sleep`)

```java
try {
    Thread.sleep(1000); // 1 секунда
} catch (InterruptedException e) {
    System.out.println("Поток был прерван");
}
```

##### 2.6 Основные ошибки при работе с потоками
- вызов `run()` вместо `start()`;
- отсутствие `try‑catch` для `sleep`;
- попытка управлять потоком напрямую;
- хранение логики в `main`.

#### 3. Работа с файлами в Java

##### 3.1 Зачем нужны файлы

Файлы используются для:
- хранения данных между запусками программы;
- логирования;
- конфигураций;
- отчётов.

##### 3.2 Класс `File`

```java
File file = new File("data.txt");

if (file.exists()) {
    System.out.println("Файл существует");
}
```

##### 3.3 Запись в файл (`FileWriter`)

```java
try (FileWriter writer = new FileWriter("data.txt")) {
    writer.write("Привет, файл!");
} catch (IOException e) {
    System.out.println("Ошибка записи в файл");
}
```

##### 3.4 Дозапись в файл

```java
new FileWriter("data.txt", true);
```

##### 3.5 Чтение из файла (`BufferedReader`)

```java
try (BufferedReader reader =
         new BufferedReader(new FileReader("data.txt"))) {

    String line;
    while ((line = reader.readLine()) != null) {
        System.out.println(line);
    }

} catch (IOException e) {
    System.out.println("Ошибка чтения файла");
}
```

##### 3.6 Почему используется `try‑with‑resources`
- файл автоматически закрывается;
- меньше утечек ресурсов;
- код безопаснее.

#### 4. Коллекции в Java

##### 4.1 Почему не массивы

**Массивы:**
- фиксированного размера;
- неудобны для добавления и удаления элементов.

**Коллекции:**
- динамические;
- гибкие;
- удобные для работы с объектами.

#### 5. Интерфейс `List` и класс `ArrayList`

##### 5.1 Создание списка

```java
ArrayList<String> names = new ArrayList<>();
```

##### 5.2 Добавление элементов

```java
names.add("Анна");
names.add("Иван");
```

##### 5.3 Перебор элементов

```java
for (String name : names) {
    System.out.println(name);
}
```

##### 5.4 Коллекции объектов

```java
ArrayList<Employee> employees = new ArrayList<>();
employees.add(new Employee("Иван", 30, 50000));
```

#### 6. Связь коллекций и ООП

Коллекции чаще всего хранят:
- объекты базового класса;
- объекты интерфейса.

```java
ArrayList<Employee> staff = new ArrayList<>();
staff.add(new Manager(...));
staff.add(new Developer(...));
```

Используется полиморфизм.

#### 7. Архитектурный пример (потоки + файлы + коллекции)

**Задача:**
- коллекция сотрудников;
- поток сохраняет их в файл;
- основной поток работает дальше.

```java
class SaveTask implements Runnable {
    private ArrayList<Employee> employees;

    public SaveTask(ArrayList<Employee> employees) {
        this.employees = employees;
    }

    @Override
    public void run() {
        try (FileWriter writer = new FileWriter("employees.txt")) {
            for (Employee e : employees) {
                writer.write(e.getInfo() + "\n");
            }
        } catch (IOException e) {
            System.out.println("Ошибка сохранения");
        }
    }
}
```

#### 9. Типичные ошибки
- использование массива вместо коллекции;
- отсутствие обработки `IOException`;
- запись логики в `main`;
- отсутствие потоков (всё в одном потоке);
- отсутствие разделения ответственности классов.

#### 10. Вопросы для защиты
- Чем поток отличается от обычного метода?
- Почему нельзя вызывать `run()` напрямую?
- Зачем нужен `Runnable`?
- Чем `ArrayList` лучше массива?
- Как работает `try‑with‑resources`?
- Что будет, если файл не существует?
- Как связаны коллекции и полиморфизм?
- Почему работа с файлами — это риск?
