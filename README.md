# OPTIONS-SERVICE

**OPTIONS-SERVICE** является сервисом хранения настроек и мета-даты для [golos.io](https://golos.io).

За каждым пользователем закрепляется MongoDB-документ, хранящий в поле options объект настроек, где ключем, обычно,
является алиас микросервиса, а значением - произвольные данные, формат которых определяет сам микросервис.
Поддерживает запросы и с фронтенд-гейта, форсируя имя юзера, при этом передавать это имя в запросе не нужно.
Поддерживает множественный запрос - достаточно передать параметры в виде массива, на каждый из которых
будет произведен поиск и целевое действие.

API JSON-RPC:

```
get:
  user: <string>       // Имя пользователя
  profile: <string>    // Идентификатор профиля. Если такого профиля нет, то создастся дефолтный

set:
  user: <string>       // Имя пользователя
  profile: <string>    // Идентификатор профиля. Если такого профиля нет, то создастся дефолтный
  data: <any>          // Любые данные для записи.
```

Возможные переменные окружения `ENV`:

-   `GLS_CONNECTOR_HOST` _(обязательно)_ - адрес, который будет использован для входящих подключений связи микросервисов.

-   `GLS_CONNECTOR_PORT` _(обязательно)_ - адрес порта, который будет использован для входящих подключений связи микросервисов.

-   `GLS_METRICS_HOST` _(обязательно)_ - адрес хоста для метрик StatsD.  
    Дефолтное значение при запуске без докера - `127.0.0.1`

-   `GLS_METRICS_PORT` _(обязательно)_ - адрес порта для метрик StatsD.  
    Дефолтное значение при запуске без докера - `8125`

-   `GLS_MONGO_CONNECT` - строка подключения к базе MongoDB.  
    Дефолтное значение - `mongodb://mongo/admin`

-   `GLS_DAY_START` - время начала нового дня в часах относительно UTC.  
    Дефолтное значение - `3` (день начинается в 00:00 по Москве).

Для запуска сервиса достаточно вызвать команду `docker-compose up --build` в корне проекта, предварительно указав необходимые `ENV` переменные.
