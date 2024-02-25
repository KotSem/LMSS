Распределенный вычислитель арифметических выражений
Это система, предназначенная для выполнения арифметических операций над большими выражениями, разделяя их на более мелкие части, которые могут быть обработаны параллельно на нескольких компьютерах или вычислительных узлах.

Состоит из 2 элементов:

Сервер, который принимает арифметическое выражение, переводит его в набор последовательных задач и обеспечивает порядок их выполнения. Далее будем называть его оркестратором.

Вычислитель, который может получить от оркестратора задачу, выполнить его и вернуть серверу результат. Далее будем называть его агентом.

Оркестратор - Сервак, который имеет следующие endpoint-ы:=
Добавление вычисления арифметического выражения.
Получение списка выражений со статусами.
Получение значения выражения по его идентификатору.
Получение списка доступных операций со временем их выполения.
Настройка времени выполения операций.

Агент - Демон, который получает выражение для вычисления с сервера, вычисляет его и отправляет на сервер результат выражения. При старте демон запускает несколько горутин, каждая из которых выступает в роли независимого вычислителя. Количество горутин регулируется переменной среды.

Деплой
На ПК должен быть установлен docker актуальной версии. Ссылка на установщик (https://hub.docker.com)
Настроить переменные среды можно изменив файл конфигурации:
docker-compose.yml

Создайте образы и поднимите контейнеры:

```sh
$ docker compose up --build
```

## API

### Получение выражения по его идентификатору

#### Request

```http
GET http://localhost:8080/expression?id=1
```

| Parameter | Type | Description |
| :--- | :--- | :--- |
| `id` | `int` | **Required**. идентификатор выражения |

#### Response

```javascript
{
    "ID": 1,
    "Expression": "2+2*2",
    "Result": "6",
    "Status": "ok",
    "CreatedAt": "2024-02-16T17:48:15.446296Z",
    "EvaluatedAt": "2024-02-16T17:48:15.450403Z"
}
```

### Добавление вычисления арифметического выражения

#### Request

```http
POST http://localhost:8080/expression
```

```javascript
{
    "expression": "2+2*2"
}
```

#### Response

```
Приняли к обработке
id: 1
```

### Получение списка выражений со статусами

#### Request

```http
GET http://localhost:8080/expressions
```

#### Response

```javascript
[
    {
        "ID": 1,
        "Expression": "2+2*2",
        "Result": "6",
        "Status": "ok",
        "CreatedAt": "2024-02-16T17:48:15.446296Z",
        "EvaluatedAt": "2024-02-16T17:48:15.450403Z"
    },
    {
        "ID": 2,
        "Expression": "2+2*2",
        "Result": "6",
        "Status": "ok",
        "CreatedAt": "2024-02-16T17:52:01.050951Z",
        "EvaluatedAt": "2024-02-16T17:52:01.052477Z"
    }
]
```

### Получение списка доступных операций со временем их выполения

#### Request

```http
GET http://localhost:8080/operations
```

#### Response

```javascript
[
    {
        "operator": "+",
        "execution_time": 10
    },
    {
        "operator": "-",
        "execution_time": 10
    },
    {
        "operator": "*",
        "execution_time": 10
    },
    {
        "operator": "/",
        "execution_time": 10
    }
]
```

### Настройка времени выполения операций

#### Request

```http
POST http://localhost:8080/operations
```

```javascript
{
    "add_time": 10,
    "sub_time": 10,
    "mul_time": 10,
    "div_time": 10
}
```

