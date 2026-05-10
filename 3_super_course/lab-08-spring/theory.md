# Лабораторная работа
# Разработка REST API на Spring Boot

---

# Цель работы

Изучить основы backend‑разработки на Java с использованием Spring Boot, научиться создавать REST API, обрабатывать HTTP‑запросы и организовывать структуру серверного приложения.

---

# Что такое backend?

Backend — это серверная часть приложения.

Backend:
* принимает запросы от клиента;
* обрабатывает данные;
* выполняет вычисления;
* работает с файлами и базами данных;
* возвращает результат клиенту.

---

# Как работает веб‑приложение?

## Общая схема работы

```text
Пользователь → Браузер → HTTP‑запрос → Сервер → Ответ → Браузер
```

**Пример:**

1. Пользователь нажимает кнопку.
2. Браузер отправляет запрос.
3. Сервер получает запрос.
4. Сервер выполняет код.
5. Сервер возвращает ответ.

## Что такое HTTP?

HTTP — это протокол передачи данных между клиентом и сервером.

**Основные HTTP‑методы:**

* **GET** — используется для получения данных.  
  **Пример:** `GET /questions`
* **POST** — используется для отправки данных на сервер.  
  **Пример:** `POST /result`
* **PUT** — используется для изменения данных.
* **DELETE** — используется для удаления данных.

## Что такое REST API?

REST API — это архитектурный стиль взаимодействия клиента и сервера через HTTP.

**Основные признаки REST API:**
* каждый ресурс имеет URL;
* используются HTTP‑методы;
* сервер и клиент разделены;
* данные обычно передаются в JSON.

## Что такое JSON?

JSON — это текстовый формат хранения и передачи данных.

**Пример JSON:**
```json
{
  "name": "Ivan",
  "age": 20
}
```

**JSON‑массив:**
```json
[
  {
    "name": "Ivan"
  },
  {
    "name": "Anna"
  }
]
```

## Что такое Spring Boot?

Spring Boot — это фреймворк для Java, который позволяет быстро создавать серверные приложения.

**Преимущества Spring Boot:**
* быстрый старт;
* встроенный сервер Tomcat;
* удобная работа с REST API;
* минимальная настройка;
* современная архитектура.

## Структура Spring Boot‑проекта

Обычно проект делится на несколько частей:

```
src
└── main
      ├── java
      │     └── controller
      │     └── service
      │     └── model
      └── resources
            └── application.properties
```

**Назначение папок:**

| Папка | Назначение |
|------|-----------|
| controller | Обработка HTTP‑запросов |
| service | Логика приложения |
| model | Классы данных |
| resources | Настройки |

## Создание Spring Boot‑проекта через IntelliJ IDEA

1. `File → New → Project`.
2. Выбрать: *Spring Initializr*.
3. Указать:
   * Project: Maven;
   * Language: Java.
4. Заполнить:
   * Group: `ru.college`;
   * Artifact: `springtest`.
5. Добавить зависимость: *Spring Web*.
6. Нажать *Create*.

## Запуск проекта

**Главный класс:**
```java
@SpringBootApplication
public class SpringtestApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringtestApplication.class, args);
    }
}
```

## Проверка работы сервера

После запуска: `http://localhost:8080`

## Контроллеры

Контроллер — это класс, который принимает HTTP‑запросы.

**Аннотация `@RestController`**

Сообщает Spring: «Этот класс будет обрабатывать запросы».

**Аннотация `@RequestMapping`**

`@RequestMapping("/api")` — задаёт общий путь.

**Пример контроллера:**
```java
@RestController
@RequestMapping("/api")
public class TestController {

    @GetMapping("/hello")
    public String hello() {
        return "Hello!";
    }
}
```
**Полный адрес:** `http://localhost:8080/api/hello`

**Аннотация `@GetMapping`**

`@GetMapping("/test")` — обрабатывает GET‑запрос.

**Пример GET‑запроса:**
```java
@GetMapping("/message")
public String message() {
    return "Сервер работает";
}
```

**Аннотация `@PostMapping`**

`@PostMapping("/result")` — обрабатывает POST‑запрос.

**Что такое `@RequestBody`?**

Позволяет получить JSON из запроса.

**Пример:**
```java
@PostMapping("/result")
public String result(@RequestBody String data) {
    return data;
}
```

## Работа с объектами

**Создание модели:**
```java
public class Question {

    private int id;
    private String text;
}
```

**Конструктор:**
```java
public Question(int id, String text) {
    this.id = id;
    this.text = text;
}
```

**Геттеры:**
```java
public int getId() {
    return id;
}
```

**Почему нужны геттеры?**

Spring использует их для преобразования объекта в JSON.

**Пример возврата объекта:**
```java
@GetMapping("/question")
public Question getQuestion() {
    return new Question(1, "Любишь Java?");
}
```

**Что вернёт сервер?**
```json
{
  "id": 1,
  "text": "Любишь Java?"
}
```

## Service‑слой

**Зачем нужен Service?**

Контроллер должен:
* принимать запрос;
* возвращать ответ.

Логика приложения должна находиться отдельно.

**Пример Service:**
```java
@Service
public class TestService {

    public String getResult(int points) {

        if (points > 10) {
            return "Backend";
        }

        return "Frontend";
    }
}
```

**Использование Service:**
```java
@RestController
public class TestController {

    private final TestService service;

    public TestController(TestService service) {
        this.service = service;
    }
}
```

## Тестирование API

**Через браузер**

Подходит только для GET.

**Через Postman**

Позволяет:
* отправлять GET;
* отправлять POST;
* передавать JSON;
* смотреть ответы сервера.

## Типичные ошибки

* **Сервер не запускается.**  
  Причины:
  * ошибка в коде;
  * неправильная зависимость;
  * занят порт 8080.
* **404 Not Found.**  
  Причины:
  * неправильный URL;
  * ошибка в mapping.
* **JSON не читается.**  
  Причины:
  * неправильный формат;
  * отсутствуют геттеры.

## Рекомендуемая архитектура

```
Controller
↓
Service
↓
Model
```

## Хороший стиль кода

**Используйте:**
* понятные названия;
* разделение логики;
* аккуратное форматирование.

**Не рекомендуется:**
* писать весь код в одном классе;
* дублировать код;
* использовать непонятные имена.

## Вопросы на понимание

* Чем backend отличается от frontend?
* Зачем нужен HTTP?
* Почему REST API удобен?
* Почему контроллер не должен содержать всю логику?
* Зачем нужны модели?
* Почему JSON стал стандартом?
* Чем GET отличается от POST?