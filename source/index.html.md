---
title: Maker-GPT API Reference

language_tabs: # must be one of https://github.com/rouge-ruby/rouge/wiki/List-of-supported-languages-and-lexers
  - shell
  - javascript

toc_footers:
  - <a href='https://t.me/GptMAKER_ai_bot'>Получить ключ достура</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the Maker GPT API Reference
---

# Введение

Этот документ представляет собой справочник по API Maker-GPT.

Maker-GPT API предоставляет простой доступ к API различных моделей ИИ приводя всё к единому стандарту построенному на базе API OpenAI.

<aside class="notice">
Есть в мире хорошие люди, они перевели документацию по OpenAI API - https://openai-docs.ru/docs.
</aside>

Подключив Maker-GPT API в своё ПО Вы сможете одним и тем же кодом предоставить клиентам и пользователям доступ к моделям:

Провайдер | Модель | Имя модели в Maker AI | Версия модели
-------------- | -------------- | -------------- | --------------
OpenAI | gpt-3.5-turbo | gpt-3.5-turbo | gpt-3.5-turbo-0125
OpenAI | gpt-4o | gpt-4o | gpt-4o-2024-05-13
YandexGPT | yandexgpt-lite | yandexgpt-lite | yandexgpt-lite 22.05.2024
YandexGPT | yandexgpt | yandexgpt | YandexGPT Pro

Мы сами мониторим выпуск новых моделей и интегрируем их в платформу - Вы всегда пользуетесь последними версиями ИИ моделей.
В ближайших планах, подключение к системе векторной базы данных, содержащей всю доступную информацию по продуктам экосистемы 1С.

## Тарификация

Базовые принципы тарификации описываются на примере телеграм бота - [https://t.me/GptMAKER_ai_bot](https://t.me/GptMAKER_ai_bot/?start=utfromapidocs). Работа пользователя через Maker-GPT API тарифицируется абсолютно аналогично.

При первом входе на баланс нового пользователя зачисляется 100р и он уменьшается в соответствии с тарифами при каждом запросе.
При достижении нуля - включается ограничение по количеству токенов в день - 10000 токенов на все запросы ко любым моделям.

При первой оплате, пользователь переходит на тариф Base - включаются функции создания собственных кастомных ролей и возможности сохранения и загрузки контекста. Эти возможности сохраняются за пользователем на всегда, независимо от дальнейшей оплаты токенов.

Тарификация осуществляется в зависимости от модели:

Модель | Контекст | Генерация
-------------- | -------------- | --------------
gpt-3.5-turbo | 0.12 руб / 1000 токенов | 0.35 руб / 1000 токенов
gpt-4o | 1.2 руб / 1000 токенов | 2.5 руб / 1000 токенов
yandexgpt-lite | 0.3 руб / 1000 токенов | 0.3 руб / 1000 токенов
yandexgpt-pro | 3 руб / 1000 токенов | 3 руб / 1000 токенов

Стоимость запроса списыается с баланса пользователя в Maker-GPT сразу после совершения запроса.

> Все запросы производятся методом POST на адрес https://api.maker-gpt.pro/[endpoint].

# Авторизация

> Авторизация:

```shell
# With shell, you can just pass the correct header with each request
curl https://api.maker-gpt.pro/v1/system/models \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer meowmeowmeow" \
    -d '{ }'
```

```javascript
    const queryAPI = async function (path, data) {
        const apiUrl = 'https://api.maker-gpt.pro';
        const token = 'meowmeowmeow';
        const headers = {
            'Authorization': `Bearer ${token}`,
            'Content-Type': 'application/json'
        };
        const _reply = await axios.post(`${apiUrl}${path}`, data, { headers })
        return _reply.data;
    }
```

> Замените `meowmeowmeow` на Ваш ключ API.

Для доступа к Maker-GPT API необходимо получить ключ доступа. Перейдите в [Telegram бот платформы](https://t.me/GptMAKER_ai_bot), зайдите в "Мой кабинет", и получите ключ доступа к API.

Ключ API должен быть включен во все запросы к серверу, в заголовке, который выглядит следующим образом:

`Authorization: Bearer meowmeowmeow`

<aside class="notice">
Замените <code>meowmeowmeow</code> на Ваш ключ API.
</aside>


Даллее, работа с API без идентификации пользователя не возможна. Пользователь должен самостоятельно получить ключ доступа с помощью бота https://t.me/GptMAKER_ai_bot, либо на странице авторизации - https://api.maker-gpt.pro/auth.

В боте - надо зайти в "Мой кабинет" и отобразить скрытый текст в сообщении.

На странице авторизации пользователю будет предложено ввести адрес электронной почты, подтвердить его с помощью кода. Ключ доступа будет показан на странице https://api.maker-gpt.pro/auth после проверки кода и продублирован пользователю на электронную почту.

Доступ к созданию пользователей возможен в двух случаях:

1. Для "доверенных" систем доступно создание пользователя по API. Для этого необходимо использовать привелигированный ключ доступа к апи. Признак доступа к привелигированному функционалу устанавливается администрацией проекта.
2. У Вас уже есть доступ к системе и Вы можете получить пользователя по API. В таком случае, Вы можете использовать параметр commonKey - по этому параметру баланс вновь созданного пользователя прикрепляется к балансу пользователя, чей commonKey Вы передали в запросе создания. Пользователи будут иметь все настройки отдельно, но использовать баланс пользователя с заданным commonKey.

# Пользователи

## Создание пользователя

> Создание пользователя:

```shell
curl https://api.maker-gpt.pro/v1/user/create \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer meowmeowmeow" \
    -d '{
      "user": {
        "systemName": "mysystem",
        "first_name": "Иван",
        "last_name": "Иванов",
        "username": "ivan",
        "fio": "Иванов Иван",
        "email": "ivan@mail.ru",
        "phone": "880000000",
        "companyName": "ООО Рога и Копыта",
        "commonKey": "1234"
      }
    }'
```

```javascript
const queryAPI = async function (path, data) {
        const apiUrl = 'https://api.maker-gpt.pro';
        const token = 'meowmeowmeow';
        const headers = {
            'Authorization': `Bearer ${token}`,
            'Content-Type': 'application/json'
        };
        const _reply = await axios.post(`${apiUrl}${path}`, data, { headers })
        return _reply.data;
    }
const _user = await queryAPI('/v1/user/create', {
  user: {
        systemName: "mysystem",
        first_name: "Иван",
        last_name: "Иванов",
        username: "ivan",
        fio: "Иванов Иван",
        email: "ivan@mail.ru",
        phone: "880000000",
        companyName: "ООО Рога и Копыта",
        commonKey: "1234"
      }
});
```

> В ответ Вы получите ответ с кодом 200 и JSON:

```json
{
  "user": {
    "accessKey": "1234",
    "systemName": "mysystem",
    "first_name": "Иван",
    "last_name": "Иванов",
    "username": "ivan",
    "fio": "Иванов Иван",
    "email": "ivan@mail.ru",
    "phone": "880000000",
    "companyName": "ООО Рога и Копыта",
    "contextEnabled": true,
    "context": [ ],
    "role": {
      "name": "",
      "role": "system",
      "content": ""
      },
    "model": "gpt-4o",
    "tarif": "free",
    "personalRoles": [],
    "commonKey": "1234",
    "balance": 100,
    "creationDate": "2024-08-01T23:22:52.490Z",
    },
  "usage": {
    "todayUsedTokens": 0
    }
}
```

> Пример массива контекста:

```json
{
  "context" :
  [
    {
        "role" : "user",
        "content" : "Вопрос"
    },
    {
        "role" : "assistant",
        "content" : "Ответ"
    }
  ]
}
```

> Пример объекта роли:

```json
{
"role": {
      "name": "Программист JavaScript",
      "role": "system",
      "content": "ты - опытный программист на javascript."
    }
}
```

> Пример массива персональных ролей:

```json
[
    {
        "name" : "Проектировщик",
        "creationDate" : "2024-08-16T09:53:38.012Z",
        "role" : {
            "name" : "Проектировщик",
            "role" : "system",
            "content" : "Ты проектировщик архитектуры проектов на экосистеме javascript. Рекомендуешь лучшие практики создания программного обеспечения, поясняешь почему так лучше."
        }
    }
]
```

> В случае ошибке будет отдан ответ с кодом 500 и описанием ошибки:

```json
{
  "error": true,
  "message": "описание ошибки"
}
```

Эндпоинт создаёт нового пользователя. Дальнейшая работа с апи без идентификации пользователя не возможна.

<aside class="warning">
Нужен "специальный" ключ API.
</aside>

### HTTP Request

`POST https://api.maker-gpt.pro/v1/user/create`

### Query Parameters

Parameter | Description
--------- | ------- | -----------
user | Объект описывающий пользователя в системе. Параметр обязательный.
user.systemName | Система - клиент Maker AI API. Пользователи уникальны в рамках Системы. Параметр обязательный.
user.first_name | Имя пользователя в клиенте.
user.last_name | Фамилия пользователя в клиенте.
user.fio | Полниое ФИО пользователя в клиенте.
user.email | Email пользователя в клиенте.
user.phone | Телефон пользователя в клиенте.
user.companyName | Наименование компании.
user.commonKey | commonKey пользователя - владельца баланса

Кроме systemName все параметры не обязательны и носят информационный характер. Параметр systemName обозначает Вашу систему для Maker-GPT.

### Response

Parameter | Default | Description
--------- | --------- | -----------
user.accessKey |  | Ключ доступа пользователя к системе. Уникален в рамках всей системы.
user.systemName |  | Система - клиент Maker AI API. Пользователи уникальны в рамках Системы.
user.contextEnabled | <code>false</code> | Сохранять или нет контекст пользователя.
user.context | <code>[]</code> | Массив текущего контекста пользователя.
user.role | <code>{}</code> | Состав текущей роли.
user.model | <code>gpt-4o</code> | Установленная роль.
user.tarif | <code>free</code> | Имя тарифа пользователя.
user.personalRoles | <code>[]</code> | Массив собственных ролей пользователя.
user.commonKey |  | Общий ключ пользователя. Используется для объединения пользователя при работе из разных систем - клиентов Maker AI API
user.balance | <code>100</code> | Персональный баланс пользователя в рублях
user.balanceOwner |  | commonKey родительского пользователя, балансом которого пользуетс текущий.
user.parentBalance |  | Баланс родительского пользователя.
user.creationDate | <code>[]</code> | Дата создания пользователя.
user.commonKey | <code>[]</code> | commonKey пользователя - владельца баланса
usage | | Статистика использования.
usage.todayUsedTokens | 0 | Количество потраченных за сегодня токенов на всех моделях.

"username", "first_name", "last_name", "phone", "email" на данный момент носят информационных характер.

<aside class="success">
Дальше, в javascript коде будем использовать функцию queryAPI из первого примера.
</aside>

## Получение пользователя

> Получение пользователя

```shell
curl https://api.maker-gpt.pro/v1/user/get \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer meowmeowmeow" \
    -d '{
      "user": {
        "accessKey": "1234"
      }
    }'
```

```javascript
const _user = await queryAPI('/v1/user/get', {
  user: {
        accessKey: "1234"
      }
});
```

> В ответ Вы получите ответ с кодом 200 и JSON:

```json
{
  "user": {
    "accessKey": "1234",
    "systemName": "mysystem",
    "first_name": "Иван",
    "last_name": "Иванов",
    "username": "ivan",
    "fio": "Иванов Иван",
    "email": "ivan@mail.ru",
    "phone": "880000000",
    "companyName": "ООО Рога и Копыта",
    "contextEnabled": true,
    "context": [ ],
    "role": {
      "name": "",
      "role": "system",
      "content": ""
      },
    "model": "gpt-4o",
    "tarif": "free",
    "personalRoles": [],
    "commonKey": "",
    "balance": 100,
    "creationDate": "2024-08-01T23:22:52.490Z",
    },
  "usage": {
    "todayUsedTokens": 0
    }
}
```

> В случае ошибке будет отдан ответ с кодом 500 и описанием ошибки:

```json
{
  "error": true,
  "message": "описание ошибки"
}
```

Эндпоит отдаёт пользователя по его идентификатору.

### HTTP Request

`POST https://api.maker-gpt.pro/v1/user/get`

### POST Parameters

Parameter | Description
--------- | -----------
user | Объект описывающий пользователя в системе. Параметр обязательный.
user.accessKey | Ключ доступа пользователя к системе. Параметр обязательный.

### Response

Parameter | Default | Description
--------- | --------- | -----------
user.accessKey |  | Ключ доступа пользователя к системе. Уникален в рамках всей системы.
user.systemName |  | Система - клиент Maker AI API. Пользователи уникальны в рамках Системы.
user.contextEnabled | <code>false</code> | Сохранять или нет контекст пользователя.
user.context | <code>[]</code> | Массив текущего контекста пользователя.
user.role | <code>{}</code> | Состав текущей роли.
user.model | <code>gpt-4o</code> | Установленная роль.
user.tarif | <code>free</code> | Имя тарифа пользователя.
user.personalRoles | <code>[]</code> | Массив собственных ролей пользователя.
user.commonKey |  | Общий ключ пользователя. Используется для объединения пользователя при работе из разных систем - клиентов Maker AI API
user.balance | <code>100</code> | Персональный баланс пользователя в рублях
user.balanceOwner |  | commonKey родительского пользователя, балансом которого пользуетс текущий.
user.parentBalance |  | Баланс родительского пользователя.
user.creationDate | <code>[]</code> | Дата создания пользователя.
usage | | Статистика использования.
usage.todayUsedTokens | 0 | Количество потраченных за сегодня токенов на всех моделях.

"username", "first_name", "last_name", "phone", "email" на данный момент носят информационных характер.

## Установка модели

> Установка модели:

```shell
curl https://api.maker-gpt.pro/v1/user/model/set \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer meowmeowmeow" \
    -d '{
      "user": {
        "accessKey": "1234"
      },
      "model": "gpt-4o"
    }'
```

```javascript
const _user = await queryAPI('/v1/user/model/set', {
  user: {
        accessKey: "1234"
      },
  model: "gpt-4o"
});
```

> В ответ Вы получите ответ с кодом 200 и JSON:

```json
{
  "user": {} // JSON с пользователем
}
```


> В случае ошибке будет отдан ответ с кодом 500 и описанием ошибки:

```json
{
  "error": true,
  "message": "описание ошибки"
}
```

Эндпоит устанавливает модель пользователя. Список моделей доступных в системе можно получить запросом https://api.maker-gpt.pro/v1/system/model

### HTTP Request

`POST https://api.maker-gpt.pro/v1/user/model/set`

### POST Parameters

Parameter | Description
--------- | -----------
user | Объект описывающий пользователя в системе. Параметр обязательный.
user.accessKey | Ключ доступа пользователя к системе. Параметр обязательный.
model | Имя модели. Параметр обязательный.


## Переключение контекста

> Переключение контекста:

```shell
curl https://api.maker-gpt.pro/v1/user/context/switch \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer meowmeowmeow" \
    -d '{
      "user": {
        "accessKey": "1234"
      }
    }'
```

```javascript
const _user = await queryAPI('/v1/user/context/switch', {
  user: {
        accessKey: "1234"
      }
});
```

> В ответ Вы получите ответ с кодом 200 и JSON:

```json
{
  "contextEnabled": true
}
```


> В случае ошибке будет отдан ответ с кодом 500 и описанием ошибки:

```json
{
  "error": true,
  "message": "описание ошибки"
}
```

Эндпоит переключает параметр сохранения контекста пользоателя. При любом переключении текущий контекст сбрасывается.

### HTTP Request

`POST https://api.maker-gpt.pro/v1/user/context/switch`

### POST Parameters

Parameter | Description
--------- | -----------
user | Объект описывающий пользователя в системе. Параметр обязательный.
user.accessKey | Ключ доступа пользователя к системе. Параметр обязательный.


## Очистка текущего контекста

> Очистка текущего контекста:

```shell
curl https://api.maker-gpt.pro/v1/user/context/clear \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer meowmeowmeow" \
    -d '{
      "user": {
        "accessKey": "1234"
      }
    }'
```

```javascript
const _user = await queryAPI('/v1/user/context/clear', {
  user: {
        accessKey: "1234"
      }
});
```

> В ответ Вы получите ответ с кодом 200 и JSON:

```json
{
  "user": {}
}
```


> В случае ошибке будет отдан ответ с кодом 500 и описанием ошибки:

```json
{
  "error": true,
  "message": "описание ошибки"
}
```

Эндпоит удаляет все сообщения из массива контекста пользоателя, начинается новый диалог.

### HTTP Request

`POST https://api.maker-gpt.pro/v1/user/context/clear`

### POST Parameters

Parameter | Description
--------- | -----------
user | Объект описывающий пользователя в системе. Параметр обязательный.
user.accessKey | Ключ доступа пользователя к системе. Параметр обязательный.

## Контексты пользователя

> получение сохранённых ранее контекстов:

```shell
curl https://api.maker-gpt.pro/v1/user/context/getsaved \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer meowmeowmeow" \
    -d '{
      "user": {
        "accessKey": "1234"
      }
    }'
```

```javascript
const _user = await queryAPI('/v1/user/context/getsaved', {
  user: {
        accessKey: "1234"
      }
});
```

> В ответ Вы получите ответ с кодом 200 и JSON:

```json
{
  "context_id": "", // идентификатор контекста
  "name": "", // заданное имя
  "contextLength": 0, // количество сообщений в контексте
  "creationDate": "" // дата создания
}
```


> В случае ошибке будет отдан ответ с кодом 500 и описанием ошибки:

```json
{
  "error": true,
  "message": "описание ошибки"
}
```

Эндпоит отдаёт краткий список сохранённых пользователем контекстов.

### HTTP Request

`POST https://api.maker-gpt.pro/v1/user/context/getsaved`

### POST Parameters

Parameter | Description
--------- | -----------
user | Объект описывающий пользователя в системе. Параметр обязательный.
user.accessKey | Ключ доступа пользователя к системе. Параметр обязательный.

### Response

Parameter | Description
--------- | -----------
context_id | идентификатор контекста
name | заданное имя
contextLength | количество сообщений в контексте
creationDate | дата создания

## Установка контекста

> Установка выбранного контекста:

```shell
curl https://api.maker-gpt.pro/v1/user/context/set \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer meowmeowmeow" \
    -d '{
      "user": {
        "accessKey": "1234"
      },
      "context_id": ""
    }'
```

```javascript
const _user = await queryAPI('/v1/user/context/set', {
  user: {
        accessKey: "1234"
      },
  context_id: ""
});
```

> В ответ Вы получите ответ с кодом 200 и JSON:

```json
{
  "success": true
}
```


> В случае ошибке будет отдан ответ с кодом 500 и описанием ошибки:

```json
{
  "error": true,
  "message": "описание ошибки"
}
```

Эндпоит устанавливает ранне сохранённый контекст. context_id получаем из user/context/getsaved.

### HTTP Request

`POST https://api.maker-gpt.pro/v1/user/context/set`

### POST Parameters

Parameter | Description
--------- | -----------
user | Объект описывающий пользователя в системе. Параметр обязательный.
user.accessKey | Ключ доступа пользователя к системе. Параметр обязательный.
context_id | context_id ранне сохранённого контекста. Получаем из user/context/getsaved.

## Сохранение контекста

> Сохранение текущего контекста:

```shell
curl https://api.maker-gpt.pro/v1/user/context/add \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer meowmeowmeow" \
    -d '{
      "user": {
        "accessKey": "1234"
      },
      "context": {
        "name": ""
      }
    }'
```

```javascript
const _user = await queryAPI('/v1/user/context/add', {
  user: {
        accessKey: "1234"
      },
  context: {
    name: "" // имя контекста
  }
});
```

> В ответ Вы получите ответ с кодом 200 и JSON:

```json
{
  "success": true
}
```


> В случае ошибке будет отдан ответ с кодом 500 и описанием ошибки:

```json
{
  "error": true,
  "message": "описание ошибки"
}
```

Сохраняет текущий контекст под заданным именем для дальнейшего использования.

### HTTP Request

`POST https://api.maker-gpt.pro/v1/user/context/add`

### POST Parameters

Parameter | Description
--------- | -----------
user | Объект описывающий пользователя в системе. Параметр обязательный.
user.accessKey | Ключ доступа пользователя к системе. Параметр обязательный.
context.name | Имя контекста.

## Удаление контекста

> Удаление сохранённого контекста:

```shell
curl https://api.maker-gpt.pro/v1/user/context/remove \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer meowmeowmeow" \
    -d '{
      "user": {
        "accessKey": "1234"
      },
      "context_id": ""
    }'
```

```javascript
const _user = await queryAPI('/v1/user/context/remove', {
  user: {
        accessKey: "1234"
      },
  context_id: ""
});
```

> В ответ Вы получите ответ с кодом 200 и JSON:

```json
{
  "success": true
}
```


> В случае ошибке будет отдан ответ с кодом 500 и описанием ошибки:

```json
{
  "error": true,
  "message": "описание ошибки"
}
```

Удалит ранее сохранённый контекст с заданным context_id.

### HTTP Request

`POST https://mapi.maker-gpt.pro/v1/user/context/remove`

### POST Parameters

Parameter | Description
--------- | -----------
user | Объект описывающий пользователя в системе. Параметр обязательный.
user.accessKey | Ключ доступа пользователя к системе. Параметр обязательный.
context_id | id контекста.

## Установка роли

> Установка роли:

```shell
curl https://api.maker-gpt.pro/v1/user/role/set \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer meowmeowmeow" \
    -d '{
      "user": {
        "accessKey": "1234"
      },
      "role": {
        "name": "Программист JavaScropt",
        "role": "system",
        "content": "ты - опытный программист на javascript уровня senior."
      }
    }'
```

```javascript
const _user = await queryAPI('/v1/user/role/set', {
  user: {
        accessKey: "1234"
      },
  role: {
        "name": "Программист JavaScropt",
        "role": "system",
        "content": "ты - опытный программист на javascript уровня senior."
      }
});
```

> В ответ Вы получите ответ с кодом 200 и JSON:

```json
{
  "user": {}  // объект пользователя
}
```


> В случае ошибке будет отдан ответ с кодом 500 и описанием ошибки:

```json
{
  "error": true,
  "message": "описание ошибки"
}
```

Эндпоит устанавливает роль пользователя.

### HTTP Request

`POST https://api.maker-gpt.pro/v1/user/role/set`

### POST Parameters

Parameter | Description
--------- | -----------
user | Объект описывающий пользователя в системе. Параметр обязательный.
user.accessKey | Ключ доступа пользователя к системе. Параметр обязательный.
role | Объект описывающий роль. Параметр обязательный.
user.name | Имя роли, если есть.
user.role | "system"
user.content | Описание роли для ИИ

## Добавление персональной роли

> Добавление пользовательской, персональной роли:

```shell
curl https://api.maker-gpt.pro/v1/user/role/add\
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer meowmeowmeow" \
    -d '{
      "user": {
        "accessKey": "1234"
      },
      "role": {
        "name": "Программист JavaScropt",
        "role": "system",
        "content": "ты - опытный программист на javascript."
      }
    }'
```

```javascript
const _user = await queryAPI('/v1/user/role/add', {
  user: {
        accessKey: "1234"
      },
  role: {
        "name": "Программист JavaScropt",
        "role": "system",
        "content": "ты - опытный программист на javascript."
      }
});
```

> В ответ Вы получите ответ с кодом 200 и JSON:

```json
{
  "user": {}  // объект пользователя
}
```


> В случае ошибке будет отдан ответ с кодом 500 и описанием ошибки:

```json
{
  "error": true,
  "message": "описание ошибки"
}
```

Эндпоит добавляет персональную роль пользователя. Персональные роли отдаются массивом в user.personalRoles.

### HTTP Request

`POST https://api.maker-gpt.pro/v1/user/role/add`

### POST Parameters

Parameter | Description
--------- | -----------
user | Объект описывающий пользователя в системе. Параметр обязательный.
user.accessKey | Ключ доступа пользователя к системе. Параметр обязательный.
role | Объект описывающий роль. Параметр обязательный.
user.name | Имя роли, если есть.
user.role | "system"
user.content | Описание роли для ИИ

## Удаление персональной роли

> Удаление пользовательской, персональной роли:

```shell
curl https://api.maker-gpt.pro/v1/user/role/remove\
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer meowmeowmeow" \
    -d '{
      "user": {
        "accessKey": "1234"
      },
      "role_id": ""
    }'
```

```javascript
const _user = await queryAPI('/v1/user/role/remove', {
  user: {
        accessKey: "1234"
      },
  role_id: "" // _id роли из обхекта user.personalRoles
});
```

> В ответ Вы получите ответ с кодом 200 и JSON:

```json
{
  "user": {}  // объект пользователя
}
```


> В случае ошибке будет отдан ответ с кодом 500 и описанием ошибки:

```json
{
  "error": true,
  "message": "описание ошибки"
}
```

Эндпоит удаляет персональную роль пользователя по _id из user.personalRoles.

### HTTP Request

`POST https://api.maker-gpt.pro/v1/user/role/remove`

### POST Parameters

Parameter | Description
--------- | -----------
user | Объект описывающий пользователя в системе. Параметр обязательный.
user.accessKey | Ключ доступа пользователя к системе. Параметр обязательный.
role_id | id роли для удаления. Параметр обязательный.


# Системные

## Доступные модели

> Получение списка доступных, общих для всех в системе моделей:

```shell
curl https://api.maker-gpt.pro/v1/system/models \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer meowmeowmeow" \
    -d '{}'
```

```javascript
const mai_query = {};
const maiAnswer = await queryAPI('/v1/system/models', mai_query);
```

> В ответ получаете массив JSON с списком доступных моделей и их параметрами

```json
[
  {
    "name": "gpt-3.5-turbo",
    "title": "gpt-3.5-turbo",
    "description": "Чат-бот с генеративным искусственным интеллектом, разработанный компанией OpenAI.",
    "max_capacity": 10000,
    "cost_context": 0.12,
    "cost_completion": 0.35
  },
  {
    "name": "gpt-4o",
    "title": "gpt-4o",
    "description": "Чат-бот с генеративным искусственным интеллектом, разработанный компанией OpenAI.",
    "max_capacity": 10000,
    "cost_context": 1.2,
    "cost_completion": 2.5
  }
]
```

В ответ получаете массив JSON с списком доступных моделей и их параметрами.

### HTTP Request

`POST https://api.maker-gpt.pro/v1/system/models`

## Доступные общие роли

> Получение списка доступных в системе ролей:

```shell
curl https://api.maker-gpt.pro/v1/system/roles \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer meowmeowmeow" \
    -d '{}'
```

```javascript
const mai_query = {};
const maiAnswer = await queryAPI('/v1/system/roles', mai_query);
```

> В ответ получаете массив JSON с списком доступных ролей

```json
[
  {
    "role": {
      "name": "Программист JavaScropt",
      "role": "system",
      "content": "ты - опытный программист на javascript уровня senior. всегда четко поясняешь свой код, покрываешь его тестами и используешь самые популярные библиотеки."
    },
    "name": "Программист JavaScropt",
    "creationDate": "2024-08-16T08:57:07.874Z"
  },
  {
    "role": {
      "name": "Нейтральная",
      "role": "system",
      "content": "Ты - полезный помощник в широком круге вопросов."
    },
    "name": "Нейтральная",
    "creationDate": "2024-08-16T09:02:32.255Z"
  }
]
```

В ответ получаете массив JSON с списком доступных ролей.

### HTTP Request

`POST https://api.maker-gpt.pro/v1/system/roles`


## Доступные базы данных

> Получение списка доступных пользователю баз данных для использования в RAG:

```shell
curl https://api.maker-gpt.pro/v1/db/u/get \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer meowmeowmeow" \
    -d '{
      "user": {
        "accessKey": "1234"
      }'
```

```javascript
const mai_query = {
                user: {
                        accessKey: "1234"
                      }
                  };
const maiAnswer = await queryAPI('/v1/db/u/get', mai_query);
```

> В ответ получаете массив JSON с списком доступных баз данных

```json
[
  {
    "readableName": "",
    "name": "",
    "description": "",
    "documents": [{
      "name": "",
      "fileName": "",
      "pages": 0
    }],
    "creationDate": "2024-08-16T08:57:07.874Z"
  }
]
```

В ответ получаете массив JSON с списком доступных баз данных

### HTTP Request

`POST https://api.maker-gpt.pro/v1/db/u/get`

### POST Parameters

Parameter | Description
--------- | -----------
user | Объект описывающий пользователя в системе. Параметр обязательный.
user.accessKey | Ключ доступа пользователя к системе. Параметр обязательный.


# Работа с чатом

## Запрос к Maker AI API

> зпрос ответа от текущей заданной у пользователя модели:

```shell
curl https://api.maker-gpt.pro/v1/chat/completions \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer meowmeowmeow" \
    -d '{
      "user": {
        "accessKey": "1234"
      },
      "content": "How are you?",
      "dbName": 'TeX50yXIRyhbcJRQ',
      "ragInstruction": "При ответе используй эти данные: ",
      "pages": 3
    }'
```

```javascript
const mai_query = {
                content: "How are you?",
                user: {
                        accessKey: "1234"
                      },
                dbName: 'TeX50yXIRyhbcJRQ',
                ragInstruction: 'При ответе используй эти данные: ',
                pages: 3
            };
const maiAnswer = await queryAPI('/v1/chat/completions', mai_query);
```

> В ответ получаете JSON с ответом указанной модели и статистикой использования

```json
{
  "content": "Спасибо за вопрос! Я готов помочь с любыми вопросами или задачами, связанными с программированием на JavaScript.",
  "usage": {
    "prompt_tokens": 48,
    "completion_tokens": 50,
    "total_tokens": 98,
    "model": "gpt-4o-2024-05-13",
    "todayUsedTokens": 14667,
    "prompt_cost": 0,
    "completion_cost": 0
  }
}
```

Отправляет запрос к указанной модели и возвращает ответ. Роль, текущий контекст, прочие настройки берутся из настроек пользователя.

### HTTP Request

`POST https://api.maker-gpt.pro/v1/chat/completions`

### POST Parameters

Parameter | Description
--------- | -----------
content | Запрос пользователя
user | Объект описывающий пользователя в системе. Параметр обязательный.
user.accessKey | Ключ доступа пользователя к системе. Параметр обязательный.
dbName | Имя базы данных для RAG. Параметр не обязательный.
pages | Количество страниц найденных в базе данных для подмешивания. Параметр не обязательный.
ragInstruction | Иструкция подмешивания. Параметр не обязательный.

